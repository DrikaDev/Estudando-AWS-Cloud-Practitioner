## 🧪 Lab 175 - Usar o Auto Scaling na AWS (Linux)

## Índice

- [Visão Geral do Laboratório](#visão-geral-do-laboratório)
- [Arquitetura inicial](#arquitetura-inicial-mostrando-a-infraestrutura-da-aws-com-uma-instância-command-host-em-uma-sub-rede-pública)
- [Arquitetura final](#arquitetura-final-mostrando-o-balanceador-de-carga-elástico-e-instâncias-do-ec2-em-um-grupo-do-auto-scaling-em-sub-redes-privadas-distribuídas-por-duas-zonas-de-disponibilidade)
- [Objetivos](#objetivos)
- [Tarefa 1: Criar uma AMI para o Amazon EC2 Auto Scaling](#tarefa-1-criar-uma-ami-para-o-amazon-ec2-auto-scaling)
  - [Tarefa 1.1: Conectar à instância Command Host](#tarefa-11-conectar-à-instância-command-host)
  - [Tarefa 1.2: Configurar a AWS CLI](#tarefa-12-configurar-a-aws-cli)
  - [Tarefa 1.3: Criar uma instância do EC2](#tarefa-13-criar-uma-instância-do-ec2)
  - [Tarefa 1.4: Criar uma AMI personalizada](#tarefa-14-criar-uma-ami-personalizada)
- [Tarefa 2: Criar um ambiente de Auto Scaling](#tarefa-2-criar-um-ambiente-de-auto-scaling)
  - [Tarefa 2.1: Criar um Application Load Balancer](#tarefa-21-criar-um-application-load-balancer)
  - [Tarefa 2.2: Criar um modelo de execução](#tarefa-22-criar-um-modelo-de-execução)
  - [Tarefa 2.3: Criar um grupo do Auto Scaling](#tarefa-23-criar-um-grupo-do-auto-scaling)
- [Tarefa 3: Verificar a configuração do Auto Scaling](#tarefa-3-verificar-a-configuração-do-auto-scaling)
- [Tarefa 4: Testar a configuração do Auto Scaling](#tarefa-4-testar-a-configuração-do-auto-scaling)
- [Conclusão:](#conclusão)

---

### Visão Geral do Laboratório

Neste laboratório, você vai:

- Usar a **AWS Command Line Interface (AWS CLI)** para criar uma instância do **Amazon Elastic Compute Cloud (EC2)** e hospedar
  um servidor da web.  
- Criar uma **Imagem de Máquina da Amazon (AMI)** a partir dessa instância.  
- Utilizar essa AMI como base para iniciar um sistema com **dimensionamento automático** sob uma carga variável, usando o
  **Amazon EC2 Auto Scaling**.  
- Criar um **Elastic Load Balancer (ELB)** para distribuir a carga entre instâncias do EC2 em várias **Zonas de Disponibilidade**
  por meio da configuração do Auto Scaling.  

### Arquitetura inicial mostrando a infraestrutura da AWS com uma instância **Command Host** em uma sub-rede pública:  
<img width="1196" height="690" alt="image" src="https://github.com/user-attachments/assets/32169e7d-efba-46dd-9521-43865e8a7654" />

### Arquitetura final mostrando o balanceador de carga elástico e instâncias do EC2 em um grupo do Auto Scaling em sub-redes privadas 
distribuídas por duas Zonas de Disponibilidade:  
<img width="1704" height="1038" alt="image" src="https://github.com/user-attachments/assets/a69b6495-7f7d-40ed-977a-9a0d0f19f5b9" />

---

### Objetivos

Depois de concluir este laboratório, você poderá:

- Criar uma instância do **EC2** usando um comando da **AWS CLI**.  
- Criar uma **AMI** usando a **AWS CLI**.  
- Criar um **modelo de execução** do Amazon EC2.  
- Criar uma **configuração de execução** do Amazon EC2 Auto Scaling.  
- Configurar as **políticas de scaling** e criar um **grupo do Auto Scaling** para reduzir ou aumentar a quantidade de servidores com
  base em uma carga variável.

---

## Tarefa 1: Criar uma AMI para o Amazon EC2 Auto Scaling

Nesta tarefa, vamos iniciar uma nova instância do **EC2** e criará uma **AMI** com base nessa instância em execução.  
Você usará a **AWS CLI** na instância do **EC2 Command Host** para executar todas essas operações.  

---

#### Tarefa 1.1: Conectar à instância Command Host  

Nesta tarefa, vamos usar o **EC2 Instance Connect** para conectar-se à instância do **EC2 Command Host** que foi criada quando o 
laboratório foi provisionado.  
Vamos usar essa instância para executar comandos da **AWS CLI**.  

1. No **Console de Gerenciamento da AWS**, na barra **Pesquisar**, insira e escolha **EC2**.  
2. No painel de navegação, selecione **Instâncias**.  
3. Na lista de instâncias, selecione a instância **Command Host**.  
4. Clique em **Conectar-se**.  

<img width="1420" height="208" alt="image" src="https://github.com/user-attachments/assets/08ad47bc-f23b-4f5b-858a-6fbe75dfddea" />

5. Na guia **EC2 Instance Connect**, selecione **Conectar-se**.

<img width="1417" height="178" alt="image" src="https://github.com/user-attachments/assets/67180b58-187c-45b1-805a-0306dd3cd714" />

<img width="1110" height="320" alt="image" src="https://github.com/user-attachments/assets/44485d3e-bbe6-4f60-9aa2-ba01c2408633" />

Agora que você já estabeleceu conexão com a instância **Command Host**, é possível configurar e usar a **AWS CLI** para chamar os 
serviços da AWS.  

---

#### Tarefa 1.2: Configurar a AWS CLI  

A **AWS CLI** é pré-configurada na instância **Command Host**.  

Para confirmar que a Região na qual a instância Command Host está em execução é a mesma que a do laboratório (**us-west-2**), 
execute o seguinte comando:  `curl http://169.254.169.254/latest/dynamic/instance-identity/document | grep region`  

Você usará as informações dessa Região nas próximas etapas.  

<img width="1003" height="105" alt="image" src="https://github.com/user-attachments/assets/7cd88115-de76-4a37-b612-97d4351ff5a2" />

Para atualizar o software da AWS CLI com as credenciais corretas, execute o seguinte comando: `aws configure`  

Nos prompts, insira as seguintes informações:

- **ID de chave de acesso da AWS**: pressione **Enter**.  
- **AWS Secret Access Key**: pressione **Enter**.  
- **Default region name**: insira o nome da Região das etapas anteriores desta tarefa (por exemplo, `us-west-2`).  
  - Se a Região já estiver sendo exibida, pressione **Enter**.  
- **Default output format**: insira `json`.  

Agora está tudo pronto para acessar e executar os scripts detalhados nas etapas a seguir.  

Para acessar esses scripts, insira o seguinte comando para navegar até o diretório: `cd /home/ec2-user/`

---

#### Tarefa 1.3: Criar uma instância do EC2  

Nesta tarefa, vamos usar a **AWS CLI** para criar uma instância que hospede um servidor da web.  

Para inspecionar o script **UserData.txt** instalado como parte da criação da **Command Host**, execute o seguinte comando: 
`more UserData.txt`

Este script executa várias tarefas de inicialização, incluindo:

- A **atualização de todos os softwares** instalados na instância.  
- A instalação de um **aplicativo web PHP pequeno**, que pode ser usado para simular uma **alta carga de CPU** na instância.  

As seguintes linhas aparecem **perto do fim do script**:

```
find -wholename /root/.*history -wholename /home/*/.*history -exec rm -f {} \;
find / -name 'authorized_keys' -exec rm -f {} \;
rm -rf /var/lib/cloud/data/scripts/*
```
Estas linhas apagam todo o **histórico** ou informações de **segurança** que possam ter sido acidentalmente deixadas na instância 
quando a imagem foi capturada.  

<img width="1424" height="273" alt="image" src="https://github.com/user-attachments/assets/bb15ac78-18df-4a7e-97e9-7b4d8c20c16f" />

Na parte superior do Lab, selecione **Detalhes** e **Mostrar**.  
Copie os valores **KEYNAME**, **AMIID**, **HTTPACCESS** e **SUBNETID** em um editor de texto e clique em **X** para fechar o painel 
Credenciais.  

No script a seguir, substitua o texto correspondente pelos valores obtidos na etapa anterior:

```
aws ec2 run-instances \
  --key-name KEYNAME \
  --instance-type t3.micro \
  --image-id AMIID \
  --user-data file:///home/ec2-user/UserData.txt \
  --security-group-ids HTTPACCESS \
  --subnet-id SUBNETID \
  --associate-public-ip-address \
  --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=WebServer}]' \
  --output text \
  --query 'Instances[*].InstanceId'
```

Insira o script modificado na janela do terminal e execute-o.  

A saída desse comando fornecerá um **InstanceId**.  
As etapas subsequentes deste laboratório se referem a esse valor como **NEW-INSTANCE-ID**.  
Substitua esse valor conforme necessário no decorrer do laboratório.  

Copie e cole o valor **InstanceId** em um editor de texto para usar mais tarde.  

<img width="1429" height="204" alt="image" src="https://github.com/user-attachments/assets/554e2587-9d45-4a13-9647-75960e07b9db" />

Use o comando `aws ec2 wait instance-running` para monitorar o status dessa instância.  
Substitua **NEW-INSTANCE-ID** no comando a seguir pelo valor de **InstanceId** que você copiou na etapa anterior e execute o comando 
modificado: `aws ec2 wait instance-running --instance-ids NEW-INSTANCE-ID`

Aguarde o comando **aws ec2 wait instance-running** retornar para o prompt antes de prosseguir.  

A instância inicia um **novo servidor da web**.  
Para testar se o servidor da web foi instalado corretamente, obtenha o **nome do DNS público**.  

Para receber o nome do DNS público, use o seguinte comando.  
Substitua **NEW-INSTANCE-ID** pelo valor copiado anteriormente e execute o comando modificado:

```
aws ec2 describe-instances \
  --instance-ids NEW-INSTANCE-ID \
  --query 'Reservations[0].Instances[0].PublicDnsName' \
  --output text
```

Copie a saída do comando **describe-instances** sem as aspas `ec2-54-214-124-193.us-west-2.compute.amazonaws.com`  

<img width="1427" height="31" alt="image" src="https://github.com/user-attachments/assets/91830118-f0ff-411f-a883-bfcd019bdff1" />

O valor dessa saída será o seu **PUBLIC-DNS-ADDRESS** para acessar o servidor web.  

Em uma nova guia do navegador, substitua **PUBLIC-DNS-ADDRESS** pelo valor copiado e acesse o servidor web: 
`http://PUBLIC-DNS-ADDRESS/index.php`  

http://ec2-54-214-124-193.us-west-2.compute.amazonaws.com/index.php

<img width="1188" height="289" alt="image" src="https://github.com/user-attachments/assets/807fcab8-23c7-40ec-be3a-589868c761e1" />

> ⚠️ Pode levar alguns minutos para que o servidor da web seja instalado.
> Aguarde pelo menos **cinco minutos** antes de passar para as próximas etapas.  
> ❌ Não selecione **Iniciar stress** nesta fase.  

---

#### Tarefa 1.4: Criar uma AMI personalizada  

Nesta tarefa, você criará uma **AMI** baseada na instância que acabou de criar.  

Para isso, utilize o comando abaixo, substituindo **NEW-INSTANCE-ID** pelo valor que você copiou anteriormente:  
`aws ec2 create-image --name WebServerAMI --instance-id NEW-INSTANCE-ID`

<img width="1426" height="86" alt="image" src="https://github.com/user-attachments/assets/66d4ea27-b7f3-45d9-99fe-c749e1aace72" />

> ⚠️ Observação:
> Por padrão, o comando aws ec2 create-image reiniciará a instância atual antes de criar a AMI, garantindo a integridade da
> imagem no sistema de arquivos.

---

## Tarefa 2: Criar um ambiente de Auto Scaling  

Nesta seção, vamos criar um **balanceador de carga** que agrupa instâncias do **Amazon EC2** em um único endereço de **DNS**.  

Vamos usar o **Auto Scaling** para criar um pool **dinamicamente dimensionável** de instâncias do EC2, com base na **imagem (AMI)** 
criada na tarefa anterior.  

Além disso, será criado um **conjunto de alarmes** que aumentará ou reduzirá a quantidade de instâncias no grupo do balanceador de 
carga sempre que o desempenho da **CPU** de qualquer máquina do grupo **exceder** ou **cair abaixo** de limites pré-estabelecidos.  

📌 **Observação:**  
A tarefa pode ser executada usando a **AWS CLI** ou o **Console de Gerenciamento da AWS**.  
👉🏻 Para este laboratório, utilize o **Console de Gerenciamento da AWS**.  

---

#### Tarefa 2.1: Criar um Application Load Balancer  

Nesta tarefa, vamos criar um **Application Load Balancer (ALB)** que pode balancear o tráfego entre várias instâncias do **EC2** e 
diferentes **Zonas de Disponibilidade**.  

1. No **Console de Gerenciamento do EC2**, no painel de navegação à esquerda, localize a seção **Balanceamento de carga** e selecione
   **Balanceadores de carga**.  

2. Clique em **Criar balanceador de carga**.  

3. Em **Tipos de balanceador de carga**, na opção **Application Load Balancer**, clique em **Criar**.  

4. Configuração básica:  
- Nome do balanceador de carga: `WebServerELB`  

5. Mapeamento de rede:  
- VPC: selecione `VPC do laboratório`.  
- Mapeamentos: selecione as **duas Zonas de Disponibilidade** listadas.  
  - Primeira Zona: `Sub-rede pública 1`  
  - Segunda Zona: `Sub-rede pública 2`  

🔎 Isso garante que o balanceador opere em **alta disponibilidade**.  

6. Grupos de segurança:  
- Remova o grupo de segurança padrão (**clique no X**).  
- Em **Grupos de segurança**, selecione `HTTPAccess`.  
  - Esse grupo já foi criado para permitir o **acesso HTTP**.  

7. Listeners e roteamento:  
- Em **Listeners e roteamento**, selecione o link azul **Criar grupo de destino**.  
  > Observação: o link abrirá uma **nova guia** do navegador.  

- Na página **Especificar detalhes do grupo** → **Configuração básica**:  
  - **Tipo de destino**: `Instâncias`  
  - **Nome do grupo de destino**: `webserver-app`  

- Na seção **Verificações de integridade**:  
  - **Tipo de verificação de integridade**: `/index.php`  

- Clique em **Próximo** e depois em **Criar grupo de destino**.  
- Após criar com sucesso, **feche a guia Grupos de destino**.  

8. De volta à guia do navegador **Balanceadores de carga**, na seção **Listeners e roteamento**:  
   - Em **Ação padrão**, clique em **Atualizar**.  
   - Em **Encaminhar para**, selecione `webserver-app`.
   > Esta ação irá vincular grupo de destino ao ALB  

9. Na parte inferior da página, clique em **Criar balanceador de carga**.  

10. Para visualizar o balanceador de carga **WebServerELB** que você criou, selecione:  
    - Clique em **Balanceador de carga**.  
    - Copie o **Nome DNS** do balanceador (usando a opção de cópia).  
    - Cole esse nome em um editor de texto → será necessário em etapas futuras do laboratório.  

<img width="1159" height="203" alt="image" src="https://github.com/user-attachments/assets/e552f799-1e71-4e37-885e-ac231705a222" />

---

#### Tarefa 2.2: Criar um modelo de execução  

Nesta tarefa, vamos criar um **modelo de execução** para o grupo do **Auto Scaling**.  
Esse modelo define como as instâncias do **Amazon EC2** serão iniciadas (AMI, tipo de instância, par de chaves, grupo de segurança e 
discos).  

1. No **Console de Gerenciamento do EC2**, no painel de navegação à esquerda, localize a seção **Instâncias** e selecione **Modelos de
   execução**.  

2. Clique em **Criar modelo de execução**.  

3. Nome e descrição:  
- Nome do modelo de execução (obrigatório): `web-app-launch-template`  
- Descrição da versão do modelo: `A web server for the load test app`  
- Orientação sobre o Auto Scaling: selecione  
  `Fornecer orientação para me ajudar a configurar um modelo que eu possa usar com o EC2 Auto Scaling`  

4. Imagem (AMI):  
  - Em **Imagens da aplicação e do sistema operacional (imagem de máquina da Amazon)**, selecione a guia **Minhas AMIs**.  
  - Confirme que a **WebServerAMI** já está selecionada.  

5. Tipo de instância:  
- **Tipo de instância**: `t3.micro`  

6. Par de chaves (login):  
- Confirme que a lista suspensa **Nome do par de chaves** está definida como:  
  `Não incluir no modelo de execução`  

> ℹ️ **Observação:**  
> O Amazon EC2 usa criptografia de chave pública para login.  
> Neste laboratório, você **não precisará se conectar à instância**, portanto não será necessário configurar o par de chaves.  

7. Configurações de rede:  
- Em **Grupos de segurança**, selecione: `HTTPAccess`  

8. Finalizar:  
  - Clique em **Criar modelo de execução**.  
  - Você deve receber a mensagem:  
<img width="1389" height="76" alt="image" src="https://github.com/user-attachments/assets/b5c25cd2-7e8c-4fc8-84c5-2902ccbba80f" />

9. Por fim, clique no botão **Visualizar modelos de execução**.

<img width="1418" height="272" alt="image" src="https://github.com/user-attachments/assets/50df012f-abe2-401f-bfc7-059330759d42" />

---

#### Tarefa 2.3: Criar um grupo do Auto Scaling  

Nesta tarefa, vamos usar o **modelo de execução** criado anteriormente para configurar um **grupo do Auto Scaling**.  

1. Selecione o modelo `web-app-launch-template`.  

2. Na lista suspensa **Ações**, escolha **Criar grupo do Auto Scaling**.  

<img width="1418" height="312" alt="image" src="https://github.com/user-attachments/assets/1ca1d3e9-6e6c-4b86-b075-baefeb6bb18d" />

3. Nome do grupo:  
- Nome do grupo do Auto Scaling: `Web App Auto Scaling Group`  
- Clique em **Próximo**.  

4. Opções de execução de instância (Rede):  
- VPC: selecione `VPC do laboratório`.  
- **Zonas de disponibilidade e sub-redes**:  
  - `Sub-rede privada 1 (10.0.2.0/24)`  
  - `Sub-rede privada 2 (10.0.4.0/24)`  
- Clique em **Próximo**.  

5. Opções avançadas:  
- Balanceamento de carga – opcional: selecione `Anexar a um balanceador de carga existente`.  
- Anexar a um balanceador de carga existente:  
  - Selecione `Escolha entre seus grupos de destino de balanceador de carga`.  
  - Na lista suspensa, escolha: `webserver-app | HTTP`.  
- Verificações de integridade: `Ative as verificações de integridade do Elastic Load Balancing`.  
- Clique em **Próximo**.  

6. Políticas de scaling e tamanho do grupo:  
- Tamanho do grupo (opcional):  
  - Capacidade desejada: `2`  
  - Capacidade mínima: `2`  
  - Capacidade máxima: `4`  

- **Ajuste de escala automática (opcional)**:  
  - Selecione `Política de dimensionamento com monitoramento do objetivo`.  
  - **Tipo de métrica**: `Média de utilização da CPU`  
  - **Valor de destino**: `50`  

> Isso instruirá o Auto Scaling a manter a **utilização média da CPU em 50%**.  
> Novas instâncias serão adicionadas ou removidas automaticamente conforme necessário para manter esse nível.  

- Clique em **Próximo**.  

7. Notificações (opcional):  
- Clique em **Próximo**.  

8. Tags (opcional):  
- Clique em **Adicionar tag**.  
  - Chave: `Name`  
  - Valor: `WebApp`  
- Clique em **Próximo**.  

9. Análise e criação:  
- Clique em **Criar grupo do Auto Scaling**.  

#### 📌 Resultado esperado  
- As instâncias do **EC2** serão iniciadas em **sub-redes privadas** nas duas Zonas de Disponibilidade.  
- Inicialmente, o grupo exibirá contagem de **instâncias = 0**, mas em seguida criará **2 instâncias** para atingir a capacidade
  desejada.  

<img width="1412" height="303" alt="image" src="https://github.com/user-attachments/assets/cd0631fe-9904-492d-9788-1cb17e9fef01" />

⚠️ **Observação:**  
Se ocorrer erro indicando que o tipo de instância `t3.micro` não está disponível, refaça a tarefa usando `t2.micro`.  

---

## Tarefa 3: Verificar a configuração do Auto Scaling  

Nesta tarefa, vamos verificar se a configuração do **Auto Scaling** e do **balanceador de carga** está funcionando corretamente.  

Para isso, será utilizado um **script pré-instalado** em um dos servidores que consumirá ciclos de CPU, acionando o **alarme de 
aumento da quantidade de instâncias**.  

1. No painel de navegação à esquerda, selecione **Instâncias**.  

2. Observe que duas novas instâncias identificadas como **WebApp** estão sendo criadas como parte do grupo do Auto Scaling.  

   - Durante a criação, o campo **Verificação de status** exibirá:  
     `Inicializando`  
   - Aguarde até que o status seja alterado para:  
     `Aprovado em 2/2 verificações`  

<img width="1415" height="299" alt="image" src="https://github.com/user-attachments/assets/0855ce5d-77f2-4159-aea1-0da04d9a1b44" />

   > 💡 Talvez seja necessário clicar em **Atualizar** para ver o status atualizado.  

3. Após a inicialização das instâncias:  
   - No painel de navegação à esquerda, vá até **Balanceamento de carga** → **Grupos de destino**.  
   - Selecione o grupo de destino: `webserver-app`.  

4. Na guia **Destinos**, verifique se as duas instâncias estão listadas.  
   - Atualize a lista até que o **Status da verificação** dessas instâncias mude para **Healthy**.  

<img width="1417" height="623" alt="image" src="https://github.com/user-attachments/assets/32eb22d2-80af-4223-a283-52fbea1c3c4a" />

5. Resultado esperado:  
Assim que as instâncias estiverem com status **Íntegro**, você poderá **testar o aplicativo web** acessando-o por meio do
**balanceador de carga**.  

---

## Tarefa 4: Testar a configuração do Auto Scaling  

Nesta tarefa, vamos testar se o **Auto Scaling** responde corretamente ao aumento de utilização da CPU.  

1. Abra uma nova guia do navegador da web.  

2. Cole o **Nome do DNS do balanceador de carga** na barra de endereços e pressione **Enter**.  

<img width="1192" height="293" alt="image" src="https://github.com/user-attachments/assets/d1302e28-7a38-4503-9098-1d133c40751b" />

3. Na página exibida, clique em **Iniciar stress**.  
   - Essa ação executará a aplicação `stress` em segundo plano.  
   - A utilização da **CPU** da instância que atendeu à solicitação subirá para **100%**.  

4. Verificar no Console da AWS:  
- No **Console de Gerenciamento do EC2**, no painel de navegação à esquerda, vá até **Auto Scaling** → **Grupos do Auto Scaling**.  
- Selecione o grupo: **Grupo do Auto Scaling do aplicativo web**.  

<img width="1124" height="214" alt="image" src="https://github.com/user-attachments/assets/c3d46580-0c2e-4df7-a34c-4521c0a5eb4b" />

5. Vá até a guia **Atividade**.  
- Após alguns minutos, o grupo de Auto Scaling deverá **adicionar uma nova instância**.  
- Isso acontece porque o **Amazon CloudWatch** detectou que a **utilização média da CPU** do grupo ultrapassou **50%**, disparando a política de expansão configurada.  

<img width="1195" height="408" alt="image" src="https://github.com/user-attachments/assets/a1c8070e-5218-4572-906c-209a4691ea94" />

6. Resultado esperado:  
- Uma **nova instância EC2** será criada automaticamente para atender ao aumento da carga.  
- Também é possível visualizar as novas instâncias no **Painel do EC2**.  

<img width="1416" height="252" alt="image" src="https://github.com/user-attachments/assets/5d599def-be09-4c2e-844a-fe5bf709ef0f" />

---

## Conclusão: 

Concluimos com êxito as seguintes tarefas:

- 🚀 Criamos uma instância do **EC2** usando um comando da **AWS CLI**.  
- 📦 Criamos uma **AMI** usando a **AWS CLI**.  
- 📝 Criamos um **modelo de execução** do Amazon EC2.  
- ⚙️ Criamos uma **configuração de execução** do **Amazon EC2 Auto Scaling**.  
- 📊 Configuramos as **políticas de scaling** e criar um **grupo do Auto Scaling** para reduzir ou aumentar a quantidade de servidores
  com base em uma carga variável.  

---
