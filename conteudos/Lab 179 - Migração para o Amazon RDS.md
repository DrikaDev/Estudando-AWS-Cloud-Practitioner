🧪 Lab 179 - Migração para o Amazon RDS

## Visão geral do laboratório  

Neste laboratório, vamos migrar um banco de dados MySQL/MariaDB para o **Amazon RDS**.  
Será criado um ambiente seguro, com sub-redes públicas e privadas, grupos de segurança e instância RDS.  

O objetivo é entender a criação de recursos na AWS, conectar via CLI e testar a migração de dados.

---

## Arquitetura inicial  
O diagrama a seguir ilustra a topologia do ambiente de runtime do aplicativo web da cafeteria **antes da migração**:  

<img width="816" height="904" alt="image" src="https://github.com/user-attachments/assets/3acec2c0-0d32-4d4e-ac38-01047ed0ec5d" />

- O banco de dados da aplicação é executado em uma instância **Linux** do **Amazon EC2**, utilizando o stack **LAMP** (Linux, Apache, MySQL e PHP) com o código da aplicação.  
- A instância é do tipo **T3 small** e está em uma **sub-rede pública**, permitindo acesso dos clientes da internet ao site.  
- Uma instância **CLI Host** reside na mesma sub-rede para viabilizar a administração usando a **AWS Command Line Interface (AWS CLI)**.  

---

## Arquitetura final  
Após a migração, a topologia do ambiente do aplicativo web da cafeteria ficará da seguinte forma:  

<img width="1516" height="878" alt="image" src="https://github.com/user-attachments/assets/3f19d60b-7d98-41ca-9fdc-3423d6640260" />

- O **banco de dados local** será migrado para um **banco de dados do Amazon RDS**.  
- O **Amazon RDS** será implantado dentro da mesma **VPC** da instância do aplicativo.  
- O banco de dados ficará **fora da instância EC2**, garantindo maior escalabilidade, segurança e disponibilidade.  

---

## Tarefa 1: Gerar dados de pedidos no site da cafeteria

Nesta tarefa, vamos **acessar o site da cafeteria** e fazer alguns pedidos que ficarão armazenados no banco de dados existente.  
Esses pedidos servirão como **dados iniciais** da aplicação antes da migração para a nova instância do **Amazon RDS**.  

1. **Acesse o aplicativo web da cafeteria**:  
   - Vá até o início destas instruções.  
   - Selecione **Detalhes** → **Mostrar**.  
   - Copie o valor de **CafeInstanceURL** e cole-o em uma nova guia do navegador.  

   > ℹ️ O valor **CafeInstanceURL** será semelhante a:  
   > `34.55.102.33/cafe`

2. **Copie os outros valores da tabela** e cole-os em um editor de texto para utilizá-los durante todo o laboratório.  

3. **Faça pedidos no site da cafeteria**:  
   - No site, selecione **Menu (Cardápio)**.  
   - Adicione **pelo menos um de cada item** ao pedido.  
   - Clique em **Submit order (Enviar pedido)**.  

4. **Verifique o histórico de pedidos**:  
   - Acesse a página **Histórico de pedidos**.  
   - Registre o **número de pedidos feitos**.  

## 📌 Observação importante  
Posteriormente, vamos **comparar esse número de pedidos** com o número de pedidos armazenados no **banco de dados migrado** para o 
Amazon RDS.  

---

## Tarefa 2: Criar uma instância do Amazon RDS usando a AWS CLI

### Tarefa 2.1: Conectar-se à instância CLI Host

Nesta tarefa, vamos usar o **EC2 Instance Connect** para conectar-se à instância **CLI Host** do **Elastic Compute Cloud (EC2)**.  
Vamos usar a instância para executar comandos da **AWS CLI**.

1. No **Console de Gerenciamento da AWS**, na barra de pesquisa, insira e escolha **EC2** para abrir o console.
2. No painel de navegação, selecione **Instâncias**.
3. Na lista de instâncias, selecione a instância **CLI Host**.
4. Clique em **Conectar-se**.
5. Na guia **EC2 Instance Connect**, clique novamente em **Conectar-se**.

---

### Tarefa 2.2: Configurar a AWS CLI

Nesta tarefa, vamos configurar a **AWS CLI** fornecendo os parâmetros de configuração disponíveis quando o laboratório foi provisionado.  
Após a configuração, será possível executar os comandos da AWS CLI para interagir com os serviços da AWS.

1. Conectar à instância CLI Host  
   - Use o **EC2 Instance Connect** para estabelecer a conexão segura com a instância.  

2. Para configurar a AWS CLI, execute o comando: ` aws configure `  
   E insira os valores solicitados:  
     - **AWS Access Key ID**  
     - **AWS Secret Access Key**  
     - **Default region name** (ex.: `us-east-2`)  
     - **Default output format** (ex.: `json`)

---

### Tarefa 2.3: Criar componentes obrigatórios

Nesta tarefa, vamos criar os componentes de infraestrutura obrigatórios para a instância do **Amazon RDS**.  
Os componentes são:  

- **CafeDatabaseSG** (grupo de segurança para o banco de dados do Amazon RDS)  
- **Sub-rede privada 1 CafeDB**  
- **Sub-rede privada 2 CafeDB**  
- **Grupo de sub-redes do CafeDB** (grupo de sub-redes de banco de dados)  

---

### Criar o grupo de segurança CafeDatabaseSG

O grupo de segurança protege a instância do Amazon RDS, permitindo apenas solicitações do **MySQL** (porta TCP 3306) das instâncias 
associadas ao **CafeSecurityGroup**.

Execute o comando abaixo, substituindo `<CafeInstance VPC ID>` pelo valor registrado anteriormente:  

  ```
  aws ec2 create-security-group \
    --group-name CafeDatabaseSG \
    --description "Security group for Cafe database" \
    --vpc-id <CafeInstance VPC ID>
  ```

👉🏻 Anote o GroupId retornado, pois será necessário para vincular à instância RDS.

---

### Criar a regra de entrada do grupo de segurança

Substitua **<CafeDatabaseSG Group ID>** pelo GroupId do grupo de segurança criado e **<CafeSecurityGroup Group ID>** pelo valor 
registrado do grupo de segurança da instância:  
```
aws ec2 authorize-security-group-ingress \
--group-id <CafeDatabaseSG Group ID> \
--protocol tcp --port 3306 \
--source-group <CafeSecurityGroup Group ID>
```

Confirmar a regra de entrada:
```
aws ec2 describe-security-groups \
--query "SecurityGroups[*].[GroupName,GroupId,IpPermissions]" \
--filters "Name=group-name,Values='CafeDatabaseSG'"
```

> A saída deve mostrar que o CafeDatabaseSG permite conexões na porta TCP 3306 da instância associada ao CafeSecurityGroup.
<img width="1841" height="525" alt="image" src="https://github.com/user-attachments/assets/6f0412aa-b973-4ff3-b453-b906974a1124" />
No resultado temos:
O grupo de segurança **CafeDatabaseSG** foi criado corretamente (sg-059b4224ac6160f97).
A regra de entrada foi aplicada corretamente: permite tráfego TCP na porta 3306.
A origem da conexão está restrita ao grupo CafeSecurityGroup (sg-09892d886d6836298).

> Isso significa que somente instâncias associadas ao CafeSecurityGroup podem acessar o banco de dados, que é exatamente o comportamento
  esperado.

---

### Criar sub-redes privadas

**Sub-rede privada 1 CafeDB**  
Escolha um bloco CIDR que não se sobreponha às sub-redes existentes. Exemplo: 10.200.2.0/23.  
Substitua <CafeInstance VPC ID> e <CafeInstance Availability Zone> pelos valores registrados:  
```
aws ec2 create-subnet \
--vpc-id <CafeInstance VPC ID> \
--cidr-block 10.200.2.0/23 \
--availability-zone <CafeInstance Availability Zone>
```
> Anote o valor de SubnetId para uso posterior.

**Sub-rede privada 2 CafeDB**  
Escolha outro bloco CIDR que não se sobreponha, por exemplo: 10.200.10.0/23.  
Use uma Zona de Disponibilidade diferente da primeira sub-rede:  
```
aws ec2 create-subnet \
--vpc-id <CafeInstance VPC ID> \
--cidr-block 10.200.10.0/23 \
--availability-zone <availability-zone>
```
> Anote o valor de SubnetId da segunda sub-rede.  

---

### Criar o grupo de sub-redes do CafeDB
O grupo de sub-redes de banco de dados incluirá as duas sub-redes privadas criadas acima.  
Substitua <Cafe Private Subnet 1 ID> e <Cafe Private Subnet 2 ID> pelos valores registrados:  
```
aws rds create-db-subnet-group \
--db-subnet-group-name "CafeDB Subnet Group" \
--db-subnet-group-description "DB subnet group for Cafe" \
--subnet-ids <Cafe Private Subnet 1 ID> <Cafe Private Subnet 2 ID> \
--tags "Key=Name,Value=CafeDatabaseSubnetGroup"
```
> Após a execução, a saída exibirá os atributos do grupo de sub-redes do banco de dados.

---

### Tarefa 2.4: Criar a instância do MariaDB do Amazon RDS

Agora vamos criar a **CafeDBInstance**, a instância de banco de dados mostrada na arquitetura final.  

**Definições da instância:**

- **Identificador da instância de banco de dados:** CafeDBInstance  
- **Mecanismo:** MariaDB  
- **Versão do mecanismo:** 10.5.13  
- **Classe da instância:** db.t3.micro  
- **Armazenamento alocado:** 20 GB  
- **Zona de disponibilidade:** CafeInstanceAZ  
- **Grupo de sub-redes de banco de dados:** CafeDB Subnet Group  
- **Grupos de segurança da VPC:** CafeDatabaseSG  
- **Acessibilidade pública:** não  
- **Nome de usuário:** root  
- **Senha:** Re:Start!9  

---

**Comando para criar a instância**

Substitua `<CafeInstance Availability Zone>` pelo valor da zona de disponibilidade da sua instância e `<CafeDatabaseSG Group ID>` 
pelo valor do grupo de segurança registrado anteriormente:

```
aws rds create-db-instance \
--db-instance-identifier CafeDBInstance \
--engine mariadb \
--engine-version 10.5.13 \
--db-instance-class db.t3.micro \
--allocated-storage 20 \
--availability-zone <CafeInstance Availability Zone> \
--db-subnet-group-name "CafeDB Subnet Group" \
--vpc-security-group-ids <CafeDatabaseSG Group ID> \
--no-publicly-accessible \
--master-username root \
--master-user-password 'Re:Start!9'
```

> O comando exibirá algumas informações imediatamente, mas a instância pode levar até 10 minutos para se tornar disponível.

**Monitorar o status da instância**  
Para acompanhar o status do banco de dados, execute:  
```
aws rds describe-db-instances \
--db-instance-identifier CafeDBInstance \
--query "DBInstances[*].[Endpoint.Address,AvailabilityZone,PreferredBackupWindow,BackupRetentionPeriod,DBInstanceStatus]"
```

O comando retorna:
- Endereço do endpoint  
- Zona de Disponibilidade
- Janela de backup preferencial
- Período de retenção de backup
- Status do banco de dados

> Inicialmente, o status será `creating`, depois `modifying`, `backing-up` e finalmente `available`.
Quando o status estiver `available`, registre o endpoint da instância RDS no seguinte formato:
`RDS Instance Database Endpoint Address: cafedbinstance.xxxxxxx.us-west-2.rds.amazonaws.com`

---

## Tarefa 3: Migrar dados da aplicação para a instância do Amazon RDS

Nesta tarefa, vamos migrar os dados do banco de dados local existente para o banco de dados recém-criado do **Amazon RDS**.  

### Passo 1: Conectar-se à CafeInstance

Siga as instruções utilizadas anteriormente para se conectar à instância CLI Host.

---

### Passo 2: Criar backup do banco de dados local

Use o utilitário **mysqldump** para criar um backup do banco de dados `cafe_db`.  

```
mysqldump --user=root --password='Re:Start!9' \
--databases cafe_db --add-drop-database > cafedb-backup.sql
```
O comando gera um arquivo cafedb-backup.sql contendo comandos SQL para recriar o banco de dados, tabelas, índices e dados.  

Para examinar o arquivo: `less cafedb-backup.sql`

**Navegação no less:**  
Teclas de seta ↑ ↓ → mover uma linha  
Page Up / Page Down → mover uma página  
q → sair  

---

### Passo 3: Restaurar o backup na instância do Amazon RDS

Use o comando **mysql** e substitua `<RDS Instance Database Endpoint Address>` pelo endpoint registrado da instância do RDS:

```
mysql --user=root --password='Re:Start!9' \
--host=<RDS Instance Database Endpoint Address> \
< cafedb-backup.sql
```

> Esse comando conecta-se à instância do RDS e executa todas as declarações SQL do arquivo de backup.

---

### Passo 4: Verificar a migração de dados

Abra uma sessão interativa do MySQL na instância do RDS:

```bash
mysql --user=root --password='Re:Start!9' \
--host=<RDS Instance Database Endpoint Address> \
cafe_db
```

**Recupere os dados da tabela product:**  
select * from product;  

> A consulta deve exibir todas as linhas da tabela, correspondendo ao número de itens do banco de dados original.

---

### Passo 5: Sair da sessão MySQL
`exit`

Deixe a janela SSH da CafeInstance aberta para uso posterior.  

---

## Tarefa 4: Configurar o site para usar a instância do Amazon RDS

Agora vamos configurar o site da cafeteria para usar a instância do **Amazon RDS**. O site já foi projetado para externalizar as informações de conexão do banco de dados usando o **AWS Systems Manager Parameter Store**.

---

### Passo 1: Acessar o Systems Manager

1. No **Console de Gerenciamento da AWS**, na barra de pesquisa, insira e escolha **Systems Manager**.  
2. No painel de navegação à esquerda, selecione **Armazenamento de parâmetros**.

---

### Passo 2: Atualizar o parâmetro dbUrl

1. Na lista **Meus parâmetros**, selecione `/cafe/dbUrl`.  
   - O valor atual do parâmetro será exibido junto com a descrição e outros metadados.  
2. Selecione **Editar**.  
3. Na página **Detalhes do parâmetro**, substitua o conteúdo da caixa **Valor** pelo **endpoint da instância do RDS** registrado anteriormente.  
4. Selecione **Salvar alterações**.

> O parâmetro `dbUrl` agora aponta para a instância do RDS em vez do banco de dados local.

---

### Passo 3: Testar o site

1. Abra uma nova janela do navegador e cole o **URL da CafeInstance** que você registrou no início do laboratório.  
2. A página inicial do site deve ser carregada corretamente.  
3. Selecione a guia **Histórico de pedidos** e verifique o número de pedidos no banco de dados.  
   - Compare com o número registrado antes da migração; eles devem ser iguais.  

**Opcional:**  
- Faça alguns novos pedidos e confirme que o site está funcionando corretamente.  
- Após os testes, feche a guia do navegador.

---

## Tarefa 5: Monitorar o banco de dados do Amazon RDS

O Amazon RDS permite monitorar o desempenho de uma instância de banco de dados automaticamente.  
Ele envia métricas para o **Amazon CloudWatch** a cada minuto para cada banco de dados ativo.

---

### Passo 1: Acessar o RDS

1. No **Console de Gerenciamento da AWS**, na barra de pesquisa, insira e escolha **RDS**.  
2. No painel de navegação à esquerda, selecione **Bancos de dados**.  
3. Na lista, selecione **cafedbinstance**. Informações detalhadas sobre o banco de dados serão exibidas.

---

### Passo 2: Visualizar métricas de monitoramento

1. Selecione a guia **Monitoramento**.  
   - Por padrão, a guia exibe várias métricas importantes da instância no CloudWatch.  
   - Cada métrica inclui um gráfico mostrando seu comportamento ao longo do tempo.

2. Algumas métricas exibidas incluem:  
   - **CPUUtilization**: porcentagem de utilização da CPU  
   - **DatabaseConnections**: número de conexões de banco de dados em uso  
   - **FreeStorageSpace**: espaço de armazenamento disponível  
   - **FreeableMemory**: memória RAM disponível na instância  
   - **WriteIOPS**: média de operações de E/S de gravação por segundo  
   - **ReadIOPS**: média de operações de E/S de leitura por segundo  

> Dica: algumas métricas podem estar em páginas adicionais. Use os números 2 ou 3 à direita da caixa de pesquisa para navegar.

---

### Passo 3: Monitorar a métrica DatabaseConnections

1. Abra uma sessão SQL interativa na instância `cafe_db` do RDS (substitua `<RDS Instance Database Endpoint Address>` pelo endpoint
   registrado):  

```
mysql --user=root --password='Re:Start!9' \
--host=<RDS Instance Database Endpoint Address> \
cafe_db
```

2. Recupere os dados da tabela `product`:
` select * from product; `

> O grafo DatabaseConnections no console do RDS agora mostrará 1 conexão em uso.  
> Dica: se não aparecer, aguarde 1 minuto (intervalo de amostragem) e selecione Atualizar.  

3. Feche a conexão da sessão SQL interativa: `exit`

4. Aguarde um minuto e atualize o grafo DatabaseConnections. Ele deve mostrar que o número de conexões em uso voltou a 0.

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
