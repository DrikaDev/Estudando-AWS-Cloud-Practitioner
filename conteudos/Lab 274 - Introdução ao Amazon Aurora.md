## 🧪 Lab 274 - Introdução ao Amazon Aurora  

O **Aurora** é um mecanismo de banco de dados relacional, compatível com **MySQL** e totalmente gerenciado.  
Ele combina o desempenho e a confiabilidade de bancos de dados comerciais avançados com a simplicidade e economia de bancos de dados de código aberto.  
O Aurora oferece **até cinco vezes mais desempenho que o MySQL**, sem exigir alterações na maioria das aplicações existentes que usam bancos de dados MySQL.

### Índice  

- [Tarefa 1: Criar uma instância do Aurora](#tarefa-1-criar-uma-instância-do-aurora)  
- [Tarefa 2: Conectar-se a uma instância do Amazon EC2 Linux](#tarefa-2-conectar-se-a-uma-instância-do-amazon-ec2-linux)  
- [Tarefa 3: Configurar a instância do Amazon EC2 Linux para conectar-se ao Aurora](#tarefa-3-configurar-a-instância-do-amazon-ec2-linux-para-conectar-se-ao-aurora)  
- [Tarefa 4: Criar uma tabela e inserir e consultar registros](#tarefa-4-criar-uma-tabela-e-inserir-e-consultar-registros)  

---

### Tarefa 1: Criar uma instância do Aurora  

Nesta tarefa, vamos criar uma instância de banco de dados (DB) do **Aurora**.  

1. No **Console de Gerenciamento da AWS**, use a barra de pesquisa e selecione **RDS**.

2. No menu de navegação à esquerda, escolha **Bancos de dados**.

3. Clique em **Criar banco de dados** e configure as opções:  

- Em **Escolher um método de criação de banco de dados**, selecione 'Criação padrão'.  

- Em **Opções de mecanismo**, escolha 'Aurora (MySQL Compatible)'.  

<img width="1387" height="464" alt="image" src="https://github.com/user-attachments/assets/d4e26cb2-d2c6-411a-b69b-27de6254c192" />

- Em **Versão do mecanismo**, escolha a versão especificada como 'default for major version 8.0'.

<img width="1385" height="158" alt="image" src="https://github.com/user-attachments/assets/bc2990aa-0654-4fe0-bea4-5a84b97cb66a" />

- Em **Modelos**, selecione 'Dev/teste'.

<img width="1384" height="161" alt="image" src="https://github.com/user-attachments/assets/93e21f45-4d40-4098-bb34-3dba3d3b4fb5" />

4. Na seção **Configurações**, defina as seguintes opções:  

- Em **Identificador do cluster de banco de dados:** `aurora`  

<img width="1384" height="187" alt="image" src="https://github.com/user-attachments/assets/aac1d870-e53e-486d-b7ae-2b735f30b9f6" />

- Em **Nome do usuário principal:** `admin`  
- Em **Senha principal:** `admin123`  
- Em **Confirmar senha:** `admin123`  

<img width="1385" height="530" alt="image" src="https://github.com/user-attachments/assets/55de0d6a-9557-4f3f-9b07-78497dde26eb" />

5. Na seção **Configuração da instância**:

- Em **Classe da instância de banco de dados:** `db.t3.medium` (Classes com capacidade de intermitência)

<img width="1383" height="336" alt="image" src="https://github.com/user-attachments/assets/0cfb7ba3-e793-4d82-acbd-d7e90282add8" />

6. Na seção **Disponibilidade e durabilidade**:  
- para **Implantação Multi-AZ:** selecione "Não criar uma réplica do Aurora"  

> Observação: Implantações Multi-AZ oferecem alta disponibilidade e durabilidade, **mas não são necessárias para este laboratório**.  

<img width="1385" height="168" alt="image" src="https://github.com/user-attachments/assets/7615be6c-f1cc-4565-8514-f1458fc9556e" />  

7. Na seção **Conectividade**, configure as seguintes opções e mantenha a valor padrão em todas que não forem mencionadas:  
- **VPC:** `LabVPC`  
- **Grupo de sub-redes:** `dbsubnetgroup`

<img width="1382" height="282" alt="image" src="https://github.com/user-attachments/assets/c08553da-988c-45bd-8bcd-b1183827d156" />

- **Acesso público:** Não  
- **Grupo de segurança da VPC:** Escolher existente → `DBSecurityGroup` (e remover o padrão / default)  

<img width="1384" height="374" alt="image" src="https://github.com/user-attachments/assets/fd4c71cd-ba8d-4a18-8a34-5d291a9ebaf5" />

> Observação:
> Sub-redes são segmentos de um intervalo de endereço IP da VPC.
> Um grupo de sub-redes de banco de dados permite designar sub-redes específicas às instâncias do banco de dados.

**A considerar:**  
É possível usar o serviço Amazon Virtual Private Cloud (Amazon VPC) para iniciar recursos da AWS em uma rede virtual definida por você.  
Essa rede virtual é semelhante a uma rede tradicional que você operaria no seu data center, com as vantagens de usar a infraestrutura dimensionável da AWS.  

8. Na seção **Monitoramento**, desmarque a caixa de 'Habilitar monitoramento avançado'.  

<img width="1384" height="139" alt="image" src="https://github.com/user-attachments/assets/b5a7b071-c909-4ff9-8631-6f5a23738651" />

9. Expanda a seção **Configuração adicional**.  
- Em **Nome do banco de dados inicial**, insira 'world'.  

<img width="1381" height="192" alt="image" src="https://github.com/user-attachments/assets/17e593d8-412a-4431-8541-adb194110c19" />

10. Na seção **Backup**, desmarque a caixa de verificação de 'Habilitar criptografia'.  

<img width="1383" height="203" alt="image" src="https://github.com/user-attachments/assets/c79e684c-4850-4980-936c-bb6a34b75cff" />

**Observação:**  
É possível criptografar as instâncias e os snapshots em repouso do Amazon RDS habilitando a opção de criptografia para a instância de banco de dados do RDS.  
Os dados em repouso criptografados incluem o armazenamento subjacente de uma instância de banco de dados, seus backups automatizados, suas réplicas de leitura 
e seus snapshots.  

11. Na seção **Manutenção**, desmarque a caixa de seleção de 'Habilitar a atualização automática da versão secundária'.  

<img width="1385" height="111" alt="image" src="https://github.com/user-attachments/assets/07d22fb3-f60b-476b-a588-0ea1ba034577" />

12. Role até a parte inferior da tela e selecione **Criar banco de dados**.  

> A instância de banco de dados do Aurora está no processo de inicialização e pode levar até cinco minutos para abrir.  

🤩 **Tarefa concluída:** você criou uma instância do Aurora!  

<img width="1120" height="239" alt="image" src="https://github.com/user-attachments/assets/05c1a6f2-a5ab-40d1-8964-0b78c220408c" />

---

### Tarefa 2: Conectar-se a uma instância do Amazon EC2 Linux  

Nesta tarefa, você fará login na sua **instância Windows do Amazon EC2**, que foi iniciada automaticamente ao começar o laboratório usando o **CloudFormation**.

1. No **Console de Gerenciamento da AWS**, use a barra de pesquisa e selecione **EC2**.

2. No menu de navegação à esquerda, clique em **Instances (Instâncias)**.

3. Selecione a caixa **Command Host**, e escolha 'Conectar'.  

<img width="1383" height="168" alt="image" src="https://github.com/user-attachments/assets/dae63a7e-0d22-409f-a6b0-dc9df45fb2f3" />  

4. Na tela **Conectar-se à instância**, selecione **Gerenciador de sessões**.

5. Clique em **Connect (Conectar)** para abrir uma janela do terminal.

<img width="975" height="90" alt="image" src="https://github.com/user-attachments/assets/869ae908-69f1-440c-9fce-68b0b5402317" />

🤩 **Tarefa concluída:** você se conectou à instância do Amazon EC2 chamada Command Host (Host de comando).  

--- 

### Tarefa 3: Configurar a instância do Amazon EC2 Linux para conectar-se ao Aurora.  

Nesta tarefa, vamos configurar a **instância do Amazon EC2 Linux** para se conectar ao banco de dados **Aurora**.  
Vamos utilizar o **gerenciador de pacotes `yum`** para instalar o cliente MariaDB, que será usado para se conectar ao Aurora nas etapas posteriores.

1. Execute o seguinte comando no terminal da instância EC2: ` sudo yum install mariadb -y `

2. Abra uma **nova guia no navegador**, acesse o **Console de Gerenciamento da AWS** e pesquise por **RDS**.

3. No menu de navegação à esquerda, clique em **Bancos de dados**.

4. Aguarde até que **aurora-instance-1** mostre **Disponível**.

5. Selecione a instância **aurora**.

6. Vá para a guia **Segurança e Conexão** e, na seção **Endpoints**, copie o **Nome do endpoint da instância** para um editor de texto.  
   - Exemplo de endpoint:  
     `aurora.cluster-cabcdefghijklm.us-west-2.rds.amazonaws.com`

<img width="1382" height="545" alt="image" src="https://github.com/user-attachments/assets/7bc96aad-c7ff-450b-b787-f0b84981c8a2" />

> **Existem dois tipos principais de endpoints:**  

> ### Endpoint de cluster
> - Conecta-se à instância primária do cluster.  
> - Único endpoint para operações de **gravação** (inserções, atualizações, exclusões e DDL).  
> - Permite failover automático se a instância primária falhar.  
> - Exemplo: `mydbcluster.cluster-123456789012.us-west-2.rds.amazonaws.com:3306`  

> ### Endpoint de leitor
> - Conecta-se a uma das **réplicas Aurora** do cluster.  
> - Permite balanceamento de carga para operações **somente leitura**.  
> - Exemplo: `mydbcluster.cluster-ro-123456789012.us-west-2.rds.amazonaws.com:3306`  

7. No comando a seguir, substitua <endpoint_goes_here> pelo endpoint do aurora: `mysql -u admin --password='admin123' -h <endpoint_goes_here>`  

O comando deve ser semelhante ao seguinte: `mysql -u admin --password='admin123' -h mydbcluster.cluster-123456789012.us-west-2.rds.amazonaws.com`  
=> Quando o comando for atualizado, copie-o para sua área de transferência.  

<img width="1204" height="260" alt="image" src="https://github.com/user-attachments/assets/ce30b067-c91a-4f2b-bde2-05efe280a9fc" />

8. Executar o comando na Instância EC2:
Retorne à guia do navegador do Gerenciador de sessões que foi usada para se conectar à Instância Command Host (Host de comando).
Para se conectar à instância do Aurora, execute o comando que você copiou na etapa anterior.  

<img width="1169" height="404" alt="image" src="https://github.com/user-attachments/assets/89124a6a-b149-4b80-85e9-be736a074037" />

🤩 **Tarefa concluída:** você configurou a instância do Amazon EC2 Linux para se conectar ao Aurora.  

--- 

### Tarefa 4: Criar uma tabela e inserir e consultar registros  

Nesta tarefa, vamos **criar uma tabela em um banco de dados**, carregar dados e executar uma consulta.

1. Listar os bancos de dados disponíveis: `SHOW DATABASES;`  

<img width="285" height="227" alt="image" src="https://github.com/user-attachments/assets/46061e60-1ee2-4df8-b598-b3547c71b9c2" />  

2. Selecionar o bando de dados **world**: `- USE world;`

<img width="243" height="59" alt="image" src="https://github.com/user-attachments/assets/64591358-4335-4783-928d-e78055893c54" />  

3. Criar a tabela **country** no banco de dados **world**:  

```
CREATE TABLE `country` (
`Code` CHAR(3) NOT NULL DEFAULT '',
`Name` CHAR(52) NOT NULL DEFAULT '',
`Continent` enum('Asia','Europe','North America','Africa','Oceania','Antarctica','South America') NOT NULL DEFAULT 'Asia',
`Region` CHAR(26) NOT NULL DEFAULT '',
`SurfaceArea` FLOAT(10,2) NOT NULL DEFAULT '0.00',
`IndepYear` SMALLINT(6) DEFAULT NULL,
`Population` INT(11) NOT NULL DEFAULT '0',
`LifeExpectancy` FLOAT(3,1) DEFAULT NULL,
`GNP` FLOAT(10,2) DEFAULT NULL,
`GNPOld` FLOAT(10,2) DEFAULT NULL,
`LocalName` CHAR(45) NOT NULL DEFAULT '',
`GovernmentForm` CHAR(45) NOT NULL DEFAULT '',
`Capital` INT(11) DEFAULT NULL,
`Code2` CHAR(2) NOT NULL DEFAULT '',
PRIMARY KEY (`Code`)
);
```

4. Inserir novos registros na tabela **country**:  

```
INSERT INTO `country` VALUES ('GAB','Gabon','Africa','Central Africa',267668.00,1960,1226000,50.1,5493.00,5279.00,'Le Gabon','Republic',902,'GA');
INSERT INTO `country` VALUES ('IRL','Ireland','Europe','British Islands',70273.00,1921,3775100,76.8,75921.00,73132.00,'Ireland/Éire','Republic',1447,'IE');
INSERT INTO `country` VALUES ('THA','Thailand','Asia','Southeast Asia',513115.00,1350,61399000,68.6,116416.00,153907.00,'Prathet Thai','Constitutional Monarchy',3320,'TH');
INSERT INTO `country` VALUES ('CRI','Costa Rica','North America','Central America',51100.00,1821,4023000,75.8,10226.00,9757.00,'Costa Rica','Republic',584,'CR');
INSERT INTO `country` VALUES ('AUS','Australia','Oceania','Australia and New Zealand',7741220.00,1901,18886000,79.8,351182.00,392911.00,'Australia','Constitutional Monarchy, Federation',135,'AU');
```

5. Consultar a tabela **country**: `SELECT * FROM country WHERE GNP > 35000 and Population > 10000000;`

> A consulta deve retornar dois registros: Australia e Thailand.

<img width="1168" height="280" alt="image" src="https://github.com/user-attachments/assets/2e2d5473-3433-4de4-bf62-208c45fb452c" />  

🤩 **Tarefa concluída:** você criou uma tabela chamada `country`, inseriu dados e executou uma consulta que retornou dois resultados.  

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒

