## 🧪 Lab 175 - Usar o Auto Scaling na AWS (Linux)

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

<img width="1426" height="150" alt="image" src="https://github.com/user-attachments/assets/693d53d7-f08e-4164-af7e-4cf30cd47ed8" />

Use o comando `aws ec2 wait instance-running` para monitorar o status dessa instância.  
Substitua **NEW-INSTANCE-ID** no comando a seguir pelo valor de **InstanceId** que você copiou na etapa anterior e execute o comando 
modificado: `aws ec2 wait instance-running --instance-ids NEW-INSTANCE-ID`

Aguarde o comando **aws ec2 wait instance-running** retornar para o prompt antes de prosseguir.  

A instância inicia um **novo servidor da web**.  
Para testar se o servidor da web foi instalado corretamente, obtenha o **nome do DNS público**.  

Para receber o nome do DNS público, use o seguinte comando.  
Substitua **NEW-INSTANCE-ID** pelo valor copiado anteriormente e execute o comando modificado:

`aws ec2 describe-instances \
  --instance-ids i-05bccbd510717b704 \
  --query 'Reservations[0].Instances[0].NetworkInterfaces[0].Association.PublicDnsName' \
  --output text`

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











