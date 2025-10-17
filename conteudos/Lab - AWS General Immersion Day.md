## 🧪 Lab - AWS General Immersion Day

Neste **AWS General Immersion Day**, realizado nos dias 14 e 15 de Outubro, vamos aprender várias funcionalidades dos **serviços mais básicos da AWS**.

Os módulos básicos são constituídos pelos tópicos a seguir:

- 💻 **Computação** – Amazon EC2  
- 🌐 **Redes** – Amazon VPC  
- 🔒 **Segurança** – AWS IAM  
- 📈 **Monitoramento** – Amazon CloudWatch  
- 🗄️ **Banco de Dados** – Amazon RDS  
- ☁️ **Armazenamento** – Amazon S3  
- ⚙️ **Provisionamento** – AWS CloudFormation  

Através de cada **laboratório prático**, exploraremos as características de cada serviço.

## Computação - Amazon EC2 

<img width="1912" height="1082" alt="image" src="https://github.com/user-attachments/assets/f6444e57-9d2d-48a6-82dc-c6b730ceedba" />

A **computação** é a base para a construção e execução de aplicações — seja para criar aplicações corporativas, aplicativos móveis ou executar clusters para projetos de análise de dados.  

A **AWS** oferece um **portfólio vasto de serviços de computação**, permitindo que você desenvolva, faça deploy, execute e escale suas aplicações na nuvem pública mais segura e propícia à inovação do mundo.

<img width="657" height="473" alt="image" src="https://github.com/user-attachments/assets/dd1a86ce-f0b6-4108-aacb-3495f77de55e" />

## Criar um novo par de chaves (Key Pair)

Neste laboratório, vamos criar uma instância EC2 usando um **SSH Key Pair**.  

As etapas a seguir descrevem a criação de um SSH key pair exclusivo para este laboratório.

1. Faça login no **AWS Management Console** e abra o console do **Amazon EC2**.  

2. No canto superior direito, confirme se você está na **região desejada da AWS**.  

3. Clique em **Key Pairs** na seção *Network & Security* no menu à esquerda.  

4. Clique em **Create key pair**.  

5. No campo **Nome do Key Pair**, digite: `[Seu nome]-ImmersionDay` na caixa de texto Nome do Key Pair: e clique no botão Create key pair.

6. Para usuários **Windows**, selecione o formato de arquivo **.ppk**.  

7. Clique em **Create key pair**.  

8. O arquivo `[SeuNome]-ImmersionDay.pem` será baixado automaticamente.  

9. **Salve o arquivo** no local padrão de downloads e lembre-se do **caminho completo** para utilizá-lo mais tarde.  

<img width="1912" height="344" alt="image" src="https://github.com/user-attachments/assets/c7f5eaf5-ba18-423e-aca9-f9afc5b6b3fb" />

## Inicie uma instância de servidor Web  

Lançaremos uma instância do **Amazon Linux 2**, inicializaremos o **Apache/PHP**, e instalaremos uma **página da web básica** que exibirá 
informações sobre nossa instância.  

1. Acesse o **EC2 Dashboard** no menu à esquerda e clique em **Launch instance**.  

2. Em **Name**, insira: `Servidor Web para IMD`  

3. Selecione a imagem padrão (**Amazon Machine Image - Amazon Linux 2**).  

4. Em **Instance Type**, escolha `t2.micro`.  

5. Selecione o **key pair** criado no início deste laboratório.  

6. Clique em **Edit** nas configurações de rede para ajustar o espaço em que o EC2 estará localizado.  
- Verifique a **Default VPC** e a **subnet**.  
- Confirme que **Auto-assign public IP** está definido como **Enable**.  

7. Crie um **Security Group** (firewall de rede):  
- Nome: `Immersion Day - Web Server`  
- Descrição: `Permitir acesso HTTP`  
- Adicione a regra:  
  - **Tipo:** HTTP  
  - **Protocolo:** TCP  
  - **Porta:** 80  
  - **Origem:** My IP  

8. Expanda a seção **Advanced Details**.  
- Em **Meta Data version**, selecione: ` V2 only (token required) `  
<img width="1223" height="95" alt="image" src="https://github.com/user-attachments/assets/1ac55504-88ad-459d-9d2c-a2780d4adcc9" />

9. No campo **User data**, insira o script de inicialização (User Data).  

```
#!/bin/sh
#Install a LAMP stack
dnf install -y httpd wget php-fpm php-mysqli php-json php php-devel
dnf install -y mariadb105-server
dnf install -y httpd php-mbstring
#Start the web server
chkconfig httpd on
systemctl start httpd
​#Install the web pages for our lab
if [ ! -f /var/www/html/immersion-day-app-php7.zip ]; then
   cd /var/www/html
   wget -O 'immersion-day-app-php7.zip'
'https://static.us-east-1.prod.workshops.aws/1d96a398-5c4e-4ba0-9d5a-3856f0f0759e/assets/immersion-day-app-php7.zip?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy8xZDk2YTM5OC01YzRlLTRiYTAtOWQ1YS0zODU2ZjBmMDc1OWUvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTc2MTI1NjcwNn19fV19&Signature=WtjwsvTLDT9hK4l4aexWXPHoBCsg-VAL3EfqQXZcx9QVbudjkVIhEo4jenwqjYcr3XogZuD6CMqTbwn0o8vELKBW0vXBJ77ezuTWj0PGwmDc7VwfW5RAQFry6VKI70utELLFmOTfABt-T0Pt3M~bBbNrMOPyse12Gfq9a~Xu7sVyTN2QDdioow-liw1Zhq8kaCVNeIsG9zR6IM0cewZXc6Jm9UEGJXjos3q2rlxyobzwJugdtynLem8AXMR6B7GbkJ-PBITTYanS8wAR~pq0HbTAjYXnZu3yYDZjWlaOJ1MXBZx4Ct1YHMXQY4pXKXssvQCxoOQ0twhRRthWCbXIkQ__'
   unzip immersion-day-app-php7.zip
fi
#Install the AWS SDK for PHP
if [ ! -f /var/www/html/aws.zip ]; then
   cd /var/www/html
   mkdir vendor
   cd vendor
   wget https://docs.aws.amazon.com/aws-sdk-php/v3/download/aws.zip
   unzip aws.zip
fi
# Update existing packages
dnf update -y
```

10. Clique em **Launch instance** para iniciar seu servidor web.

11. Clique no botão **View Instances** localizado na parte inferior direita da tela para visualizar a lista de instâncias do **Amazon EC2**.  

12. Após a inicialização da sua instância, você verá as seguintes informações:
   - O **nome do seu web server**;  
   - A **zona de disponibilidade (Availability Zone)** em que a instância está;  
   - O **nome DNS público** (publicamente roteável).  

13. Clique na **caixa de seleção** ao lado do seu web server para visualizar os **detalhes completos** dessa instância do EC2.

## Navegar pelo Servidor Web

1. Aguarde até que a instância **passe pelas verificações de status** para concluir o carregamento.  

2. Abra uma **nova guia do navegador** e acesse o servidor web inserindo o **nome DNS público** da instância do EC2 na barra de endereços.  

3. O **nome DNS público** pode ser encontrado no console do EC2, na linha **DNS IPv4 público**, conforme destacado na interface.  

<img width="1056" height="82" alt="image" src="https://github.com/user-attachments/assets/34082388-c915-4407-bd82-e291a1649637" />

<img width="1196" height="318" alt="image" src="https://github.com/user-attachments/assets/db1e343d-794c-4d36-bb29-af9d4d8fb083" />

## Conecte-se à Sua Instância Linux Usando o AWS Systems Manager Session Manager

O **Session Manager** é um recurso totalmente gerenciado do **AWS Systems Manager** que permite gerenciar suas instâncias do **Amazon EC2** por meio de 
um **shell interativo baseado em navegador**, com apenas um clique, ou via **AWS CLI**.  

Você pode usar o **Session Manager** para iniciar uma sessão com uma instância em sua conta.  
Depois que a sessão for iniciada, será possível **executar comandos bash** como faria com qualquer outro tipo de conexão SSH.  

## Criar um Perfil de Instância do IAM para o Systems Manager  

1. Faça login no **AWS Management Console** e abra o console do **IAM**.  

2. No painel de navegação, selecione **Roles** e clique em **Create role**.  

3. Em **Trusted entity type**, escolha **AWS service**.  

<img width="1889" height="396" alt="image" src="https://github.com/user-attachments/assets/ddc8e662-32c9-4218-9927-4b60f1ff0ef3" />

4. Em **Choose the service that will use this role**, selecione **EC2** e clique em **Next**.  

<img width="1411" height="356" alt="image" src="https://github.com/user-attachments/assets/b78951a4-7355-4375-bd84-a4cc0f22ebdd" />

5. Na página **Add permissions policies**, siga as etapas abaixo:  
   - No campo **Search**, digite `AmazonSSMManagedInstanceCore`.  
   - Selecione a caixa ao lado desse nome.  
   - Clique em **Next**.  

<img width="1434" height="614" alt="image" src="https://github.com/user-attachments/assets/ac773688-1fea-4b96-a25d-740681706ebf" />

6. Em **Role name**, insira um nome para o seu novo perfil de instância, por exemplo: `SSMInstanceProfile`  

<img width="1878" height="670" alt="image" src="https://github.com/user-attachments/assets/113835c0-9ea3-402f-816f-feeeba9cde86" />

7. Clique em **Create role**.  

8. O sistema retornará à página de **Roles**.  

9. **Anote o nome da role** criada — você precisará selecioná-la ao criar novas instâncias que deseja gerenciar com o Systems Manager.

## Anexar o Perfil da Instância do Systems Manager a uma Instância Existente

1. Faça login no **AWS Management Console** e abra o console do **Amazon EC2**.  

2. No painel de navegação, selecione **Instances**.  

3. Escolha sua instância do EC2 na lista e clique em **Actions**.  

4. No menu **Actions**, vá até **Security → Modify IAM role**.  

<img width="1883" height="417" alt="image" src="https://github.com/user-attachments/assets/317ff130-dd03-4a41-b950-8a34014e8724" />

5. Para **IAM role**, selecione o perfil de instância criado anteriormente (`SSMInstanceProfile`).  

<img width="1894" height="520" alt="image" src="https://github.com/user-attachments/assets/dfa70d34-3262-4b78-96ac-a73ac1475030" />

6. Clique em **Update IAM role**.

## Conectar-se à Instância Linux via SSH

1. No **console do Amazon EC2**, selecione a instância à qual você deseja se conectar.  
2. Clique no botão **Connect**.  
3. Na página **Connect to instance**, selecione a opção **SSH client**.  

## Ajustar Permissões da Chave Privada

Navegue até o diretório onde sua **chave privada** está localizada.  
Em seguida, execute o comando abaixo no terminal, substituindo **[SeuNome]** pelo nome que você especificou ao criar a chave:  
`ssh -i "[Seu nome]-ImmersionDay.pem" ec2-user@<Public IPv4 DNS>`

> Uma nova sessão será iniciada em uma nova guia do navegador.  
> A partir daí, você poderá **executar comandos bash** como faria em qualquer outro tipo de conexão SSH.  

## Desprovisionamento dos recursos

Para excluir a instância EC2 que você criou, selecione a instância e, a partir de Instance state, selecione Terminate instance.

<img width="1420" height="239" alt="image" src="https://github.com/user-attachments/assets/e88526aa-d931-4a0a-b330-0e203a1970ab" />

---

## Auto Scaling na AWS

O **AWS Auto Scaling** monitora seus aplicativos e **ajusta automaticamente a capacidade** para manter um desempenho estável e previsível **com o menor custo possível**.  

Com o **AWS Auto Scaling**, é fácil configurar o escalonamento de aplicativos para **vários recursos e serviços da AWS** em apenas alguns minutos.  
O serviço oferece uma **interface de usuário simples e poderosa**, permitindo criar **planos de escalabilidade** para os seguintes recursos:

- 🖥️ **Instâncias e frotas Spot do Amazon EC2**  
- 🐳 **Tarefas do Amazon ECS**  
- 📊 **Tabelas e índices do Amazon DynamoDB**  
- 🧩 **Réplicas do Amazon Aurora**

## Benefícios do AWS Auto Scaling

- **Escalabilidade simplificada** com recomendações automáticas para otimizar desempenho, custo ou o equilíbrio entre ambos.  
- **Integração com o Amazon EC2 Auto Scaling**, permitindo escalar não apenas instâncias EC2, mas também outros serviços AWS.  
- **Aplicativos sempre dimensionados corretamente**, garantindo os **recursos certos no momento certo**.  

> 💡 **Em resumo:**  
> O AWS Auto Scaling ajuda a manter o desempenho ideal do seu ambiente na nuvem, reduzindo custos e garantindo que suas aplicações cresçam (ou diminuam) de forma automática conforme a demanda.

## Pré-requisitos do Laboratório

Antes de criar uma **AMI (Amazon Machine Image)** para o grupo de **Auto Scaling**, precisamos configurar um **web host**.  
Vamos gerar uma AMI a partir da instância e, em seguida, escalá-la automaticamente usando um **balanceador de carga**.  

## Baixe e inicie o modelo do CloudFormation

1. Faça o download do modelo **“EC2-Auto-Scaling-Lab.yaml”** clicando com o botão direito e salve-o localmente.  
2. No console da **AWS**, pesquise **CloudFormation** ou acesse pelo menu `Services → CloudFormation` (categoria: *Gerenciamento e Governança*).

## Criação da Stack no CloudFormation

1. No console do CloudFormation, clique em **Create Stack → With new resources (standard)**.  

<img width="1425" height="189" alt="image" src="https://github.com/user-attachments/assets/67e0f385-41d7-4531-9610-7b3ed95a168e" />

2. Em **Specify template**, selecione **Upload a template file → Choose file** e carregue o arquivo `EC2-Auto-Scaling-Lab.yaml`.  

   <img width="1413" height="546" alt="image" src="https://github.com/user-attachments/assets/0627052f-1024-4494-99fe-abd74ee9b5a2" />

   Depois, clique em **Next**.  
 
## Especificar detalhes da pilha (Specify stack details)

Preencha os seguintes campos:

- **Stack name:** `[Suas Iniciais]-EC2-Web-Host`  
- **AmiD:** deixe o valor padrão (usará a versão mais recente da AMI).  
- **InstanceType:** escolha `m5.large` ou `t2.micro`.  
  > 💡 *Recomendado: m5.large (melhor desempenho para simular ambiente real).*  
- **MyIP:** insira o IP da sua máquina seguido de `/32`(isso bloqueará a porta HTTP 80 em sua máquina).  
  - Exemplo: `201.55.66.77/32`  
  - Você pode descobrir seu IP pesquisando **“Qual é o meu IP”** no navegador.  
- **MyVPC:** selecione sua **VPC padrão** (ou a desejada).  
- **PublicSubnet:** escolha uma sub-rede com acesso à Internet (todas as sub-redes padrão geralmente são públicas).  

## Revisar e criar

- Clique em **Next** nas próximas páginas (“Configure stack options” e “Review”).  
- Clique em **Create stack** para iniciar a criação do servidor web.  
- Aguarde até o status da pilha `[Suas iniciais]-EC2-web-host` mudar para **CREATE_COMPLETE**.  

> ⏱️ *A criação leva aproximadamente 3 minutos.*

<img width="1223" height="304" alt="image" src="https://github.com/user-attachments/assets/84b07a26-bb2b-4561-af74-85a5465d4fc4" />

## Confirmar a configuração da instância

1. Acesse o serviço **EC2 → Instances**.  

2. Selecione sua instância `[Suas iniciais]-EC2-Web-Host`.  

<img width="1889" height="376" alt="image" src="https://github.com/user-attachments/assets/8699804c-6731-4efd-9095-9acf9a3b6efd" />

3. Copie o **DNS IPv4 público** clicando no ícone de copiar ao lado.  

4. Cole o endereço em uma nova guia do navegador (**use http:// e não https://**).  

> ⚠️ O link HTTPS resultará em erro, pois não há certificado SSL configurado.  

Se a página não carregar, aguarde até a **verificação de status 2/2** ser concluída e tente novamente.  
Você deve ver a página “**Metadados da instância do EC2**”.

<img width="1893" height="527" alt="image" src="https://github.com/user-attachments/assets/2daff1a4-e397-4d6f-8eca-b4f0849209b3" />

## Criar uma AMI personalizada do servidor web (EC2 - Linux Lab)

Agora que o servidor web está pronto, vamos criar uma **imagem de máquina (AMI)** que será usada no grupo de **Auto Scaling**.

1. No console do **EC2 → Instances**, clique com o botão direito sobre a instância `[Suas iniciais]-Web Server`.  

2. Escolha **Image and templates → Create image**.  

<img width="1880" height="738" alt="image" src="https://github.com/user-attachments/assets/7f7f89d1-3270-46b6-aa48-6e0ba8077a81" />

4. Defina:
   - **Image name:** `[Suas iniciais]_Auto_Scaling_Webhost`
   - **Descrição:** opcional.  
   - Deixe os volumes padrão e clique em **Create Image**.  
   
5. No menu à esquerda, acesse **Images → AMIs** e verifique sua imagem.  
   - O status mudará de *pending* para *available* em alguns minutos.  

<img width="1895" height="342" alt="image" src="https://github.com/user-attachments/assets/2ce30247-b17d-47d0-b539-d10af7cc2da4" />

> 🎉 Agora você tem sua AMI personalizada pronta para uso no Auto Scaling!

## Criar um novo grupo de segurança (Security Group)

Antes de configurar o **Launch Template**, precisamos criar um grupo de segurança específico para o Auto Scaling.

1. No console **EC2**, acesse `Network & Security → Security Groups`.  

2. Clique em **Create security group**.  

3. Configure:
   - **Security group name:** `[Suas iniciais]-Auto-Scaling-SG`
   - **Description:** mesmo nome do grupo
   - **VPC:** selecione a correta (geralmente *Default VPC*)

4. Em **Inbound Rules**, não adicione nenhuma regra ainda (faremos isso mais adiante).  

5. As **Outbound Rules** podem permanecer com permissão total.  

6. Clique em **Create security group**.  

<img width="1514" height="177" alt="image" src="https://github.com/user-attachments/assets/48f3992e-d412-4ef1-b54c-8a354fd0fb06" />

Você concluiu todos os **pré-requisitos** do laboratório!  
Agora está pronto para seguir para a próxima etapa:  
👉 **Criar um Launch Template** para configurar seu grupo de Auto Scaling.

## Criando um Launch Template

### Arquitetura do Laboratório de Auto Scaling

Abaixo está o diagrama da arquitetura final que iremos construir:

<img width="2278" height="1262" alt="image" src="https://github.com/user-attachments/assets/b7149543-fd67-427a-bd4f-d4440aea2dae" />

## Componentes principais do EC2 Auto Scaling

O **EC2 Auto Scaling** é composto por **três componentes principais**:

### 1️⃣ Launch Template
Um **modelo de inicialização** é um recurso do EC2 Auto Scaling que permite armazenar parâmetros de execução, evitando a necessidade de defini-los a cada nova instância.

> Exemplos de parâmetros: AMI, tipo de instância, armazenamento e configurações de rede.  
> Cada modelo pode ter **múltiplas versões numeradas**, permitindo variações conforme necessário.

### 2️⃣ Auto Scaling Groups
As instâncias do EC2 são organizadas em **grupos de Auto Scaling**, que funcionam como uma unidade lógica.  
Você define:
- **Número mínimo** de instâncias
- **Número máximo** de instâncias
- **Capacidade desejada**

> Assim, o grupo escala automaticamente conforme a demanda, substituindo instâncias não saudáveis e otimizando o desempenho.

### 3️⃣ Políticas de Escalabilidade
As **políticas de escalabilidade** determinam **quando e como** o Auto Scaling deve ajustar os recursos.

O escalonamento pode ser:
- Manual  
- Agendado  
- Sob demanda  
- Ou automatizado via métricas do **CloudWatch**

💡 **Benefícios:**
- Ajuste dinâmico de recursos conforme o uso.  
- Redução de custos evitando o provisionamento excessivo.  
- Manutenção automática da integridade do ambiente.

## Parâmetros de dimensionamento

- **Número mínimo de instâncias:** evita que o grupo fique abaixo de um limite crítico.  
- **Número máximo de instâncias:** impede o uso excessivo de recursos.  
- **Capacidade desejada:** define o número ideal de instâncias saudáveis.  
- **Políticas de escalabilidade:** ajustam automaticamente a capacidade conforme as métricas de carga.  

## Criando um Launch Template

O primeiro passo para configurar o **Auto Scaling Group** é criar um **Launch Template**, que servirá como base para o provisionamento automático das instâncias.

1. No console da **AWS**, acesse `Services → EC2`.  

2. No painel esquerdo, clique em **Instances → Launch Templates**.  

3. Clique em **Create launch template**.  

## Configurações do Template

Na página **Create launch template**, preencha os campos:

### 🔹 Launch template name and description
- **Launch template name:** `[Suas iniciais]-scaling-template`  
- **Description:** opcional  
- **Auto Scaling guidance:** marque a opção para habilitar orientação automática  

### 🔹 Launch Template Contents

#### a. Amazon Machine Image (AMI)
- Selecione **My AMIs → Owned by me**
- Escolha a AMI personalizada criada anteriormente:  
  `[Suas iniciais]_Auto_Scaling_Webhost`  

<img width="1881" height="747" alt="image" src="https://github.com/user-attachments/assets/5c0d76e1-a6c7-4947-bbb1-23820c0a77dc" />

#### b. Instance type
- Selecione: `t2.micro`

#### c. Key Pair (Login)
- Escolha o par de chaves criado no primeiro laboratório:  
  `[Suas iniciais]-KeyPair`

#### d. Network settings
- **Subnet:** *não inclua no template*  
- **Security group:** selecione **Select existing security group**  
  - Escolha o grupo criado anteriormente: `[Suas iniciais]-Auto Scaling SG`

<img width="1243" height="750" alt="image" src="https://github.com/user-attachments/assets/d15389b0-de38-4f31-b2da-53756b4a819f" />

#### e. Storage
- Deixe a configuração **padrão**

#### f. Resource tags
- Nenhuma tag necessária neste momento

### 🔹 Advanced Details
1. Expanda a seção **Advanced Details**  
2. Em **Detailed CloudWatching monitoring**, selecione:  
   ✅ **Enable**  

<img width="1237" height="147" alt="image" src="https://github.com/user-attachments/assets/a57c4eb4-3047-4bb6-bc5e-9461a036b848" />

> Isso ativa o **monitoramento detalhado** do CloudWatch, que coleta métricas em intervalos de **1 minuto** (em vez dos 5 minutos padrão).  
> Dessa forma, o grupo de Auto Scaling reage mais rapidamente às variações de carga.

Após revisar as configurações:

1. Clique em **Create launch template**  
2. Em seguida, clique em **View launch templates**  

🎉 Pronto! Você concluiu a criação do **Launch Template** e está preparado para o próximo passo:  
👉 **Configurar o grupo de Auto Scaling**

## Configurando um Auto Scaling Group

Agora que você criou um **Launch Template**, que define os parâmetros de inicialização das instâncias, é hora de configurar um **Auto Scaling Group (ASG)**.  
Com ele, você poderá definir **quantas instâncias do EC2** devem ser iniciadas e **onde** elas serão executadas.

1. No **console da AWS**, acesse o serviço **EC2**.  

2. No painel de navegação à esquerda, localize **Auto Scaling** → **Auto Scaling Groups**.  

3. Clique em **Create an Auto Scaling group**.  

4. Dê o nome: `[Suas iniciais]-Lab-AutoScaling-Group`  

5. No menu **Launch Template**, selecione o template criado anteriormente: `[Suas iniciais]-scaling-template`  

6. Clique em **Next**.

## Configurações de Rede

Na página de configuração, preencha os seguintes campos:

- **VPC:** selecione sua VPC (geralmente a *default*).  
- **Subnet:** escolha as subnets que o Auto Scaling Group utilizará para iniciar as instâncias.  

> 💡 *Boas práticas:*  
> Use **subnets privadas** para as instâncias que estarão atrás de um **Load Balancer**.  
> Para este laboratório, você pode usar **subnets públicas** ou privadas.

<img width="1354" height="662" alt="image" src="https://github.com/user-attachments/assets/5c155d0b-31d2-4a99-84fd-91e22c3653a0" />

Clique em **Next** para continuar.

## Configuração do Load Balancer e Health Checks

1. **Load balancing:** conecte-se a um novo load balancer.  

<img width="1361" height="287" alt="image" src="https://github.com/user-attachments/assets/28a1cbe6-a5b5-49c0-af38-c19c1f1442c6" />

2. **Tipo:** Application Load Balancer.  

3. **Nome:** `[Suas Iniciais]-Application-Load-Balancer`  

<img width="1361" height="397" alt="image" src="https://github.com/user-attachments/assets/b91caf4a-960c-45ee-92da-08ba7f9e6209" />

4. **Esquema:** Internet-facing  

5. **Mapeamento de rede:** verifique se todas as subnets e zonas de disponibilidade selecionadas anteriormente aparecem.  

<img width="1357" height="612" alt="image" src="https://github.com/user-attachments/assets/2f653023-4e6a-4088-b648-cb5f7a5f513f" />

6. **Listeners e roteamento:**  
   - Porta: **80**  
   - Em *Default routing (forward to)*, clique em **Create a target group**  
   - Nome do Target Group: **[Suas Iniciais]-Target-Group**

<img width="1359" height="277" alt="image" src="https://github.com/user-attachments/assets/56760b58-5eac-456d-b105-18339ff10ab8" />

> 🔸 O **Target Group** é onde o Load Balancer localizará as instâncias registradas para distribuir o tráfego automaticamente.

7. Deixe as opções de **Health checks e configurações adicionais** como padrão e clique em **Next**.

## Tamanho do Grupo e Políticas de Escalabilidade

### 🔹 Tamanho do grupo
Configure da seguinte forma:

| Configuração        | Valor |
|---------------------|-------|
| Desired capacity     | 1 |
| Minimum capacity     | 1 |
| Maximum capacity     | 5 |

<img width="1384" height="444" alt="image" src="https://github.com/user-attachments/assets/a4d2d700-9502-4904-88e8-9c66a9d46f5f" />

Essas configurações manterão uma instância ativa, a menos que uma política de escalabilidade seja acionada.

## Políticas de escalabilidade

Selecione: **Target tracking scaling policy**

<img width="1359" height="258" alt="image" src="https://github.com/user-attachments/assets/6bf80bb0-aabd-44c8-b215-c70d9a9f6a35" />

- **Tipo de métrica:** Average CPU Utilization  
- **Target value:** 25  

<img width="1358" height="379" alt="image" src="https://github.com/user-attachments/assets/414407ed-c243-4533-8a60-6920ee089f56" />

> ⚡ Dica: Definimos a meta de CPU como **baixa** para acelerar o teste durante o laboratório.

Clique em **Next**.

## 🔔 Notificações

Você pode configurar notificações para receber alertas (por e-mail, por exemplo) quando eventos ocorrerem, como:
- Criação ou encerramento de instâncias,
- Falhas em inicializações ou encerramentos.

<img width="1423" height="279" alt="image" src="https://github.com/user-attachments/assets/ba725469-1aff-4b5b-9359-c2162500cf89" />

> Para este laboratório, pule esta etapa e clique em **Next**.

## 🏷️ Adicionando Tags

Adicione uma tag de identificação:

| Key | Value |
|-----|--------|
| Name | [Suas iniciais] - Auto Scaling Group |

<img width="1356" height="319" alt="image" src="https://github.com/user-attachments/assets/a523bba2-0502-4a4f-99ff-dde883d0b580" />

Clique em **Next**.

## ✅ Revisão e Criação

1. Revise todas as suas configurações.  
2. Clique em **Create Auto Scaling Group**.

Agora, o AWS criará automaticamente:
- O **Auto Scaling Group**
- O **Target Group**
- E o **Load Balancer**

## 🔍 Verificando o Provisionamento

Após a criação:

- Você verá uma nova instância EC2 criada automaticamente, com o nome tag: `[Suas Iniciais] - Auto Scaling Group`  
  > Pode ser necessário atualizar a página para vê-la.

<img width="1893" height="387" alt="image" src="https://github.com/user-attachments/assets/e9739d78-ac51-4684-a328-c969f9c5d063" />

- No menu à esquerda, em **Load Balancing → Load Balancers**, verifique o provisionamento do seu **Application Load Balancer**.

<img width="1897" height="408" alt="image" src="https://github.com/user-attachments/assets/2788d1d1-06fe-426d-816e-8a7dac700bc3" />

## 🚦 Próximo Passo

Agora que seu **Auto Scaling Group** está ativo, prossiga para a próxima etapa:  
👉 **Configurar Grupos de Segurança (Security Groups)** para permitir o tráfego entre o **ALB** e os **web hosts**.

## Configurando Security Groups

Nesta etapa, vamos configurar os **Security Groups** para o **Load Balancer** e o **Auto Scaling Group**, garantindo que o tráfego seja seguro e restrito apenas às instâncias necessárias.

## Criando o Security Group do Load Balancer

Quando o **Load Balancer** foi provisionado, ele recebeu o grupo de segurança padrão da VPC.  
Para permitir o acesso via **DNS público** e controlar o tráfego de saída, precisamos criar um novo **Security Group**.

1. No console da AWS, acesse **EC2** → **Security Groups** (Menu à esquerda).  
2. Clique em **Create security group**.

### Detalhes básicos

- **Security group name:** `[Suas Iniciais]-SG-Load-Balancer`  
- **Description:** `[Suas Iniciais]-SG-Load-Balancer`  
- **VPC:** Selecione sua VPC (provavelmente Default)

### Regras de entrada (Inbound rules)

1. Clique em **Add rule**  
2. **Type:** HTTP  
3. **Source:** Custom → `[Seu endereço IP]/32`  
   > Para encontrar seu IP, pesquise “Qual é o meu IP”.

### Regras de saída (Outbound rules)

1. Delete a regra padrão **All traffic**.  
2. Clique em **Add rule**  
3. **Type:** HTTP  
4. **Destination:** Custom → `[Suas Iniciais]-Auto Scaling SG`  

> 💡 Comece digitando `sg` para localizar o Security Group correto.

5. Clique em **Create security group** quando terminar.  

<img width="1532" height="175" alt="image" src="https://github.com/user-attachments/assets/bcffcf82-e937-47a7-8025-94758ce01502" />

## Anexando o Security Group ao Load Balancer

1. No menu **Load Balancing → Load Balancers**, selecione o seu Load Balancer.  

2. Na guia **Security** e clique em **Edit**.  

3. Marque apenas o Security Group recém-criado: `[Suas Iniciais]-SG-Load-Balancer`  

4. Desmarque qualquer outro grupo de segurança e clique em **Save**.  

<img width="1895" height="611" alt="image" src="https://github.com/user-attachments/assets/86557bd8-e300-4164-8e0e-4bfbe875a986" />

## Configurando regras de entrada para o Auto Scaling Group

Agora, permitiremos que apenas o **Load Balancer** tenha acesso ao **Auto Scaling Group**.

1. No console da AWS, vá para **Security Groups**.  
2. Selecione o Security Group do Auto Scaling: `[Suas Iniciais]-Auto Scaling SG`  
3. Na guia **Inbound rules**, clique em **Edit inbound rules** → **Add rule**.  
4. Configure a regra:  
   - **Type:** HTTP  
   - **Source:** Custom → `[Suas Iniciais]-SG-Load-Balancer`  
5. Clique em **Save rules**.  

<img width="1898" height="609" alt="image" src="https://github.com/user-attachments/assets/9150f79e-ccdd-4b8b-93d4-f0dcc158f969" />

## Testando o Load Balancer

1. Acesse **Load Balancers** no menu à esquerda.  
2. Na guia **Details**, copie o **DNS** do Load Balancer.  
3. Cole o DNS em um navegador.  

> Você deve ver o site sendo servido pelo seu **Auto Scaling Group**.  

✅ Agora seu **Load Balancer** e **Auto Scaling Group** estão configurados corretamente.  

Próxima etapa: **Testando o Auto Scaling Group**

## Testando um Auto Scaling Group

Agora que você criou o **Auto Scaling Group** e o **Load Balancer**, vamos testá-los para garantir que tudo esteja funcionando corretamente.

## Acessando o site via Load Balancer

1. Verifique se você está acessando o site pelo **DNS do Load Balancer** configurado na etapa anterior.  

2. Na parte inferior da página inicial, clique em **Start CPU Load Generation**.  
   > Quando a carga da CPU ultrapassar 25% por um período prolongado, a política de Auto Scaling começará a ativar novas instâncias conforme definido no **Launch Template**.  
   > Talvez seja necessário clicar duas vezes se a primeira vez não gerar carga suficiente.

## Observando novas instâncias

1. No console do **EC2**, acesse a seção **Instances**.  

2. Atualize a página e observe **novas instâncias** sendo criadas automaticamente pelo **Auto Scaling**.  

<img width="1889" height="428" alt="image" src="https://github.com/user-attachments/assets/beaa2756-40d1-4cde-8635-bd28ecc387ee" />

<img width="1885" height="427" alt="image" src="https://github.com/user-attachments/assets/7c7b9ba8-6515-4b75-9d30-01fdf907a5de" />

3. Selecione uma instância chamada `[Suas Iniciais] - Auto Scaling Group` e vá até a guia **Monitoring** para acompanhar a **CPU Utilization**.

## Verificando pelo Auto Scaling Group

1. Acesse a página do Auto Scaling Group:  

2. Selecione seu **Auto Scaling Group**: `[Suas Iniciais]-Lab-AutoScaling-Group`  

3. Na guia **Instance management**, veja quantas instâncias existem no grupo atualmente.  

4. A guia **Monitoring** mostra métricas como: tamanho do grupo, instâncias pendentes, total de instâncias e muito mais.

## Arquitetura atual

Sua arquitetura agora deve se parecer com a imagem abaixo:

<img width="2278" height="1262" alt="image" src="https://github.com/user-attachments/assets/bcc20191-fa1f-41d3-a7b6-93e8acf883f0" />

## Testando o balanceamento de carga

Depois que várias instâncias forem iniciadas com sucesso (provavelmente 3 ou 4):

1. Atualize repetidamente o navegador do site.  

2. Observe que o **ID da instância**, a **zona de disponibilidade** e o **IP privado** mudam à medida que o **Load Balancer** distribui as solicitações pelo **Auto Scaling Group**.

🎉 **Parabéns!**  
Você criou com sucesso um **grupo EC2 Auto Scaling** por trás de um **Application Load Balancer**.

> Se você precisar limpar este laboratório, vá para a seção: **Limpeza do laboratório de Auto Scaling**

## Desprovisionamento dos recursos

É importante que você **exclua os recursos criados neste laboratório** na ordem correta.

## Excluindo seu Load Balancer

1. No console da AWS, navegue até o serviço **EC2**.  
2. No menu à esquerda, em **Load Balancing**, selecione **Load Balancers**.  
3. Selecione o balanceador de carga chamado `[Suas Iniciais]-Application-Load-Balancer`.  
4. Clique em **Actions** → **Delete**.  
5. Na janela pop-up, selecione **Yes, Delete**.

> O Load Balancer deve desaparecer imediatamente se a exclusão for bem-sucedida.

## Excluindo seu Target Group

1. No console do EC2, no menu à esquerda, selecione **Target Groups**.  
2. Selecione o target group chamado `[Suas iniciais]-Grupo-alvo`.  
3. Clique em **Actions** → **Delete**.  
4. Na janela pop-up, selecione **Yes, Delete**.

> O target group agora deve ser excluído.

## Excluindo seu Auto Scaling Group

1. No console do EC2, no menu à esquerda, selecione **Auto Scaling Groups**.  
2. Selecione o grupo chamado `[Suas iniciais]-Lab-Autoscaling-group`.  
3. Clique em **Delete**.  
4. Na janela pop-up, digite `delete` e selecione **Delete**.

> Todas as instâncias do grupo serão encerradas. Isso pode levar alguns minutos.

## Excluindo seu Launch Template

1. No console do EC2, no menu à esquerda, selecione **Launch Templates**.  
2. Selecione o template chamado `[Suas iniciais]-scaling-template`.  
3. Clique em **Actions** → **Delete template**.  
4. Na janela pop-up, digite `Delete` e selecione **Delete**.

> O Launch Template agora deve ser excluído.

## Excluindo seus Security Groups

1. No console do EC2, no menu à esquerda, selecione **Security Groups**.  
2. Selecione `[Suas iniciais]-SG-Load-Balancer` e clique em **Edit inbound rules**.
3. Clique em **Delete** para remover a regra e depois em **Save rules**.
4. Selecione a guia **Outbound rules**, clique em **Edit outbound rules**, exclua todas as regras e salve.
5. Repita o mesmo processo para `[Suas iniciais]-Auto Scaling SG` (não é necessário remover regras de saída).  
6. Selecione ambos os Security Groups `[Suas iniciais]-SG-Load-Balancer` e `[Suas iniciais]-Auto Scaling SG`.  
7. Clique em **Actions** → **Delete security groups**, digite `delete` e confirme.

> Seus grupos de segurança agora devem ser excluídos.  
> Se não conseguir remover, verifique se todas as regras foram deletadas corretamente.

## Excluindo sua pilha do CloudFormation

1. No console da AWS, abra **CloudFormation**.  
2. Selecione a pilha chamada `[Suas iniciais]-EC2-Web-host`.  

<img width="1900" height="433" alt="image" src="https://github.com/user-attachments/assets/d35257e3-55a3-4027-a161-ea4ffdce5c0d" />

3. Clique em **Delete**.  
4. No pop-up, selecione **Delete stack**.

> A exclusão pode levar alguns minutos. Atualize a tela para confirmar que a pilha não está mais visível.

🎉 **Parabéns!**  
Você concluiu com sucesso o **Auto Scaling Lab**!

---

## Redes - Amazon VPC

<img width="2487" height="1367" alt="image" src="https://github.com/user-attachments/assets/413f6f0a-e3d9-41ed-86b4-7469c74f6df4" />

## Visão Geral da Amazon VPC

A **Amazon Virtual Private Cloud (Amazon VPC)** permite que você execute recursos da AWS em uma **rede virtual que você definiu**. Essa rede virtual é similar a uma rede tradicional que você operaria em seu próprio data center, com os benefícios de usar a **infraestrutura escalável da AWS**.

A Amazon VPC permite que você provisione uma **seção logicamente isolada da nuvem** onde você pode executar recursos da AWS em uma rede virtual que você define. Você tem **controle completo** sobre seu ambiente de rede virtual, incluindo:

- Seleção do seu próprio **intervalo de endereços IP**  
- Criação de **sub-redes**  
- Configuração de **tabelas de rotas** e **gateways**  

Você pode usar **IPv4** e **IPv6** em sua VPC para acesso fácil e seguro para recursos e aplicações.

## Como criar uma VPC

Clique em **Create VPC** para iniciar o assistente.  
O Launch VPC Wizard facilita a criação de VPCs com diferentes configurações padrão.

Em **VPC Settings**, configure os seguintes parâmetros:

- **Name**: `VPC-Lab`  
- **CIDR**: `10.0.0.0/16`  
- Selecione **1 Availability Zone (AZ)**: `ap-northeast-2a`  
- Número de subredes: `1`  
- Subnet CIDR: `10.0.10.0/24`  
- Subredes privadas: `0`  

Clique em **Create VPC**.

> ⚠️ Ao definir o CIDR da VPC, assegure-se de que os IPs **não se sobreponham** com redes que eventualmente se conectarão a esta VPC. Além disso, escolha uma quantidade de endereços suficiente para expansões futuras.

## Renomear a VPC e Subrede

- Após a criação, renomeie a VPC para `VPC-Lab`.  
- Vá na guia **Subnets** e renomeie a subrede criada para `Public Subnet A`.  

Agora a arquitetura deve se parecer com:
```
VPC-Lab
└─ Public Subnet A
```

## Entendendo CIDR

**CIDR (Classless Inter-Domain Routing)** é a forma de expressar o endereço e o tamanho de uma rede.

- A VPC criada acima usa o intervalo de endereços IP `/16`.  
- Isso permite **65.536 endereços IP** (2^16) para recursos.

Ao especificar o CIDR de uma VPC, é permitido usar de `/16` (65.536 IPs) até `/28` (16 IPs).  

> ⚠️ Os primeiros 4 endereços IP e o último endereço IP **não podem ser atribuídos a instâncias**.

Por exemplo, em uma subrede `10.0.0.0/24`, os 5 endereços reservados são:

| IP           | Descrição                                   |
|--------------|--------------------------------------------|
| 10.0.0.0     | Endereço da rede                            |
| 10.0.0.1     | Reservado para roteadores da VPC da AWS    |
| 10.0.0.2     | Endereço do servidor DNS                    |
| 10.0.0.3     | Reservado para uso futuro da AWS           |
| 10.0.0.255   | Endereço de broadcast da rede              |

## Criando subredes adicionais

Para manter alta disponibilidade, é importante fazer o deploy de serviços em múltiplas **Zonas de Disponibilidade (AZs)**. Neste laboratório, você criará uma subrede na **Zona de Disponibilidade C**, diferente da Zona de Disponibilidade A, onde a subrede criada anteriormente está localizada.

## Passo 1: Criar nova Subnet

1. No menu lateral esquerdo, clique em **Subnets**.  
2. Clique no botão **Create Subnet**.

## Passo 2: Configurar a Subnet

- **VPC ID**: selecione a VPC que você acabou de criar (`VPC-Lab`).  
- **Subnet settings**:

| Chave             | Valor             |
|------------------|-----------------|
| Subnet name       | public subnet C  |
| Availability Zone | ap-northeast-2c  |
| IPv4 CIDR block   | 10.0.20.0/24     |

3. Clique em **Create subnet**.

## Resultado

Após a criação, você deve ver as duas subnets:

- `public subnet A`  
- `public subnet C`  

A arquitetura da sua VPC até o momento estará assim:
```
VPC-Lab
├─ public subnet A (ap-northeast-2a)
└─ public subnet C (ap-northeast-2c)
```

<img width="656" height="360" alt="image" src="https://github.com/user-attachments/assets/8af4f813-d294-4c48-a3d9-ab3e6659ce7b" />

## Edite a tabela de rotas

## Entendendo a tabela de rotas de uma VPC

Uma **tabela de rotas** contém um conjunto de regras, chamadas **rotas**, que determinam para onde o tráfego da sua subnet ou gateway é direcionado.

- **Tabela de rotas principal**: criada automaticamente com sua VPC. Controla o roteamento para todas as subnets que **não** estão explicitamente associadas a outra tabela de rotas.  
- **Tabela de rotas personalizada**: uma tabela de rotas criada manualmente para a sua VPC.

## Como editar a conexão de uma tabela de rotas

1. Clique no botão **Actions** no menu **Subnet** e selecione **Edit routing table association**.  
2. Selecione uma **tabela de rotas** que **não seja a principal**.  
3. Verifique se há uma **rota para a internet** na tabela de rotas selecionada.  
4. Depois de selecionar **public subnet C**, clique no hyperlink da tabela de rotas na guia **Details**.  
5. Na guia **Routes**, você verá algo semelhante a:

| Destination | Target    |
|------------|-----------|
| 10.0.0.0/16 | local    |
| 0.0.0.0/0   | igw-OOO  |

Isso confirma que uma rota para a **internet** foi criada para a **subnet pública C**.

## Arquitetura atual

A arquitetura construída até agora está ilustrada abaixo:

<img width="794" height="572" alt="image" src="https://github.com/user-attachments/assets/23851fc2-8655-4079-8f81-1bf12cdabbde" />

## Crie um Security Group

Um **security group** atua como um **firewall virtual** para suas instâncias, permitindo controlar o tráfego de entrada e saída.

## Criando o Security Group

1. No console da AWS, clique no menu **Security Groups** no lado esquerdo.  
2. Clique no botão **Create security group**.  
3. Preencha os campos conforme abaixo e selecione a VPC criada neste laboratório:

| Key                  | Value                       |
|---------------------|-----------------------------|
| Security group name  | webserver-sg                |
| Description          | security group for web servers |
| VPC                  | VPC-Lab                     |

4. Adicione **Inbound Rules**:

| Type | Source |
|------|--------|
| HTTP | Custom: [Seu endereço IP privado]/32 (Você pode encontrar seu IP local pesquisando “What is my IP”) |
| SSH  | Custom: [Seu endereço IP privado]/32 (Você pode encontrar seu IP local pesquisando “What is my IP”) |

5. Clique em **Create security group** no canto inferior direito.

## Verificando as regras

Revise se a **Inbound Rule** foi criada corretamente, garantindo que o tráfego HTTP e SSH será permitido apenas do seu IP.

## Testar Conectividade

## A. Criar 2 instâncias EC2 nas sub-redes públicas existentes

### A1 - Crie um novo Par de Chaves

Neste laboratório, vamor criar um **Par de Chaves SSH** para se conectar às instâncias EC2.

1. Faça login no **Console de Gerenciamento da AWS** e abra o **Console Amazon EC2**.
2. No canto superior direito, confirme se você está na região correta.
3. No menu à esquerda, clique em **Pares de Chaves** (Key Pairs) em **Rede e Segurança**.
4. Clique no botão **Criar Par de Chaves**.
5. Digite `[Seu-nome]-ID` na caixa **Nome do Par de Chaves**.
6. Selecione **.pem** para o formato do arquivo e clique em **Criar Par de Chaves**.
7. O arquivo `[Seu-nome]-ID.pem` será baixado para seu computador. Lembre-se do caminho completo do arquivo, pois será usado para conectar às instâncias EC2.

---

### A2 - Inicie uma nova instância EC2 na Sub-rede Pública - 1

1. No **Painel EC2**, clique em **Iniciar Instâncias**.
2. Em **Nome**, insira `EC2-1`.
3. Verifique a **Imagem de Máquina da Amazon** (Amazon Linux 2).
4. Selecione **t2.micro** em Tipo de Instância.
5. Escolha o **Par de Chaves** criado no início do laboratório.
6. Clique em **Editar Configurações de Rede**:
   - VPC: `VPC-Lab`
   - Sub-rede: AZ-A
   - Habilite **Atribuição Automática de IP Público**
7. Crie um **Grupo de Segurança**:
   - Nome: `ID-EC2-SG`
   - Regra HTTP: Tipo: HTTP, Origem: Qualquer lugar (0.0.0.0/0)
8. Aceite os demais valores padrão e clique em **Iniciar Instância**.
9. Clique em **Exibir Instâncias** para ver `EC2-1`, sua zona de disponibilidade e o IP público.

---

### A3 - Inicie uma nova instância EC2 na Sub-rede Pública - 2

1. Repita os passos do item A2.
2. Configurações únicas:
   - Nome da Instância: `EC2-2`
   - Sub-rede: Zona de Disponibilidade B
   - Grupo de Segurança: Selecione o existente `ID-EC2-SG`
3. Conclua o lançamento da instância.

---

## B. Validar Conectividade

### Pré-requisito

Para validar a conectividade com **ping**, adicione uma regra **ICMP** no Grupo de Segurança existente (`ID-EC2-SG`).

> Nota: Em ambientes de produção, seja restritivo ao definir os blocos CIDR aplicáveis.

---

### B1. Entre nas instâncias EC2

1. Clique em `EC2-2` e anote o **endereço IPv4 público**.
2. Clique em `EC2-1` e depois em **Conectar** > **Conectar Instância EC2**.
3. No terminal, digite: ` ping <Endereço IPv4 Público da instância EC2-2> `
4. Você deverá ver respostas de ping bem-sucedidas, confirmando que as duas instâncias EC2 estão conectadas.

## Desprovisionamento dos recursos

## Desprovisionando os recursos

1. No **Console da VPC**, selecione a **VPC** que você criou neste laboratório no menu **VPC**.
2. Clique em **Actions** e selecione **Delete VPC**.
3. Confirme a exclusão da VPC quando solicitado.

> Nota: Excluir a VPC também excluirá todos os recursos associados, como subredes, tabelas de rotas, gateways e security groups que foram criados dentro dessa VPC.

## Amazon API Gateway

O **Amazon API Gateway** é um serviço totalmente gerenciado que facilita a criação, publicação, manutenção, monitoramento e proteção de APIs em qualquer escala. As APIs atuam como a porta de entrada para que os aplicativos acessem dados, lógica de negócios ou funcionalidade de seus serviços de backend. Usando o Amazon API Gateway, você pode criar APIs **RESTful**, **HTTP** ou **WebSocket**, que permitem aplicativos de comunicação bidirecional em tempo real.

O Amazon API Gateway lida com todas as tarefas envolvidas na aceitação e processamento de até centenas de milhares de chamadas de API simultâneas, incluindo:

- Gerenciamento de tráfego
- Suporte a CORS
- Autorização e controle de acesso
- Throttling
- Monitoramento
- Gerenciamento de versão das APIs

O Amazon API Gateway não possui taxas mínimas ou custos de inicialização. Você paga pelas chamadas de API que recebe e pela quantidade de dados transferidos. Com o modelo de preços em camadas do Amazon API Gateway, é possível reduzir seu custo à medida que o uso da API aumenta.

> **Nota:** Este laboratório foi movido para **Module 1 of The Amazon API Gateway Workshop**. Complete a etapa **System Setup** antes de iniciar o lab.

## Segurança - AWS IAM

Com a AWS, você tem o controle e a confiança necessários para executar suas aplicações no ambiente de nuvem pública mais flexível e seguro disponível hoje. Como cliente AWS, você se beneficia dos data centers da AWS e de uma rede arquitetada para proteger suas informações, identidades, aplicações e dispositivos.

A AWS ajuda você a:

- Atender requisitos de **compliance e segurança**, como guarda e proteção de dados, utilizando os serviços e funcionalidades da plataforma.
- **Automatizar tarefas de segurança** manuais, permitindo que você foque em escalar e inovar o seu negócio.
- **Pagar apenas pelo que utiliza**, garantindo eficiência de custos.
- Contar com uma nuvem **avaliada e aceita como segura** o suficiente para cargas de trabalho ultrassecretas.

## AWS IAM

O **AWS Identity and Access Management (IAM)** é um serviço que ajuda você a controlar de forma segura o acesso aos recursos da AWS. Com o IAM, você controla quem está **autenticado** e **autorizado** a usar os recursos.

Quando você cria uma conta AWS, você começa com uma identidade que possui **acesso total a todos os serviços e recursos** da conta. Essa identidade é chamada de **usuário root**, acessada com o endereço de email e senha usados para criar a conta.

> **Recomendação:** Não use o usuário root para atividades diárias, nem mesmo administrativas.
> Crie e utilize um **usuário IAM** seguindo as instruções [aqui](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started.html).

### Componentes do AWS IAM

- **Identidades do IAM**
  - Users
  - User Groups
  - Roles
- **Policy**

Para deixar seu ambiente AWS seguro, confira o documento de [melhores práticas com IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html).

<img width="2155" height="883" alt="image" src="https://github.com/user-attachments/assets/f3b0d7dd-5687-4f84-ad99-90c18e691ec4" />

## Estrutura do laboratório

Este laboratório está dividido nas seguintes partes:

1. Crie instâncias EC2 com tags  
2. Crie identidades com o AWS IAM  
3. Testando o acesso à instância EC2  
4. IAM Role para EC2  
5. Desprovisionamento dos recursos  

---

## Iniciar Instâncias EC2 com Tags

Neste laboratório, vamos aprender a iniciar **duas instâncias Amazon Linux 2** na AWS.  
Uma delas representará um **ambiente de desenvolvimento (dev)** e a outra um **ambiente de produção (prod)**.  
Usaremos **tags** para distinguir essas instâncias.

---

## 🧭 Etapas para iniciar a instância de produção

1. Acesse o **Console EC2** no **Console de Gerenciamento da AWS**.  
   - No menu lateral esquerdo, clique em **Painel EC2**.  
   - Em seguida, clique em **Iniciar instâncias**.

2. Em **Nome**, insira: `prod-instance`  

3. Clique em **Adicionar tags adicionais**.

4. Clique em **Adicionar tag** e insira o seguinte par **chave-valor**: `Chave: Env` / `Valor: prod`

5. Verifique as configurações padrão da **Imagem de Máquina da Amazon (AMI)** e selecione **Amazon Linux 2**.

6. Em **Tipo de Instância**, selecione: `t2.micro`  

7. Em **Par de chaves (login)**, selecione: Prosseguir sem um par de chaves  

8. Clique em **Iniciar instância**.

> ⚠️ **Atenção:**  
> Para este laboratório, utilizamos o **grupo de segurança padrão**.  
> As regras com origem `0.0.0.0/0` permitem acesso de qualquer IP.  
> Em ambientes reais, **recomenda-se restringir o acesso apenas a IPs conhecidos**.

---

## 🧩 Criar a instância de desenvolvimento

Agora você criará uma segunda instância EC2 para o **ambiente de desenvolvimento**.  
Repita os passos de **1 a 8**, alterando apenas o **nome** e a **tag** conforme abaixo:

| Chave | Valor |
|:------|:------|
| Nome  | dev-instance |
| Env   | dev |

---

## 🔍 Verificando as instâncias e tags

Após iniciar as duas instâncias:

1. No menu lateral, clique em **Instâncias**.  
2. Localize e selecione a instância **prod-instance**.  
3. Na parte inferior da página, clique na guia **Tags**.

Você verá os detalhes das **tags** associadas à instância EC2, como `Env=prod` e `Nome=prod-instance`.

## ✅ Conclusão

Parabéns! 🎉  
Você implantou **duas instâncias EC2** (servidores) com **diferentes tags de recursos** — uma para **produção** e outra para **desenvolvimento**.  
Essas tags ajudarão no **gerenciamento, automação e organização** dos recursos dentro da AWS.

## Crie identidades com o AWS IAM

Neste capítulo, trabalharemos na **criação de identidades do AWS IAM**.  
As identidades do IAM incluem **Users**, **Groups** e **Roles**.  
Além disso, criaremos **Policies**, que são objetos no IAM que — quando associados a uma identidade ou recurso — definem as permissões.

<img width="2155" height="883" alt="image" src="https://github.com/user-attachments/assets/db9d36ee-ba14-4c8c-8264-4b7034713d7b" />

## Etapas deste capítulo

1. Criação de uma **Policy** a ser anexada a um **Group** do IAM  
2. Criação de um **Group** chamado `dev-group`  
3. Criação de um **User** chamado `dev-user`, que fará parte do `dev-group`  

## Acesso ao Console IAM

1. Acesse o [AWS IAM Console](https://console.aws.amazon.com/iam/).  
2. Clique no botão **Customize** para gerar uma URL de login personalizada.  
3. Insira o alias da conta — para este laboratório, digite `aws-login-user_name` — e clique em **Create Alias**.

## Criando uma Policy

1. Clique em **Policies** no menu lateral esquerdo.  
2. Selecione **Create Policy** no canto superior direito.  
3. Vá até a aba **JSON** e cole o conteúdo abaixo:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "ec2:*",
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "ec2:ResourceTag/Env": "dev"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": "ec2:Describe*",
            "Resource": "*"
        },
        {
            "Effect": "Deny",
            "Action": [
                "ec2:DeleteTags",
                "ec2:CreateTags"
            ],
            "Resource": "*"
        }
    ]
}
```

4. Clique em Next: Tags e mantenha as configurações padrão.

5. Clique em Next: Review e insira os seguintes valores:

# Key / Value

| **Key** | **Value** |
|----------|------------|
| **Name** | DevPolicy |
| **Description** | IAM Policy for Dev Group |

## Estrutura de uma Policy em JSON

- **Version:** Use a versão `2012-10-17`  
- **Statement:** Pode incluir um ou mais blocos de instrução  
- **Sid:** Identificador opcional para o statement  
- **Effect:** Define se a policy `Allow` (permite) ou `Deny` (nega) acesso  
- **Principal:** Especifica a conta, usuário ou role (em policies baseadas em recursos)  
- **Action:** Lista de ações permitidas ou negadas  
- **Resource:** Lista de recursos aos quais as ações se aplicam  
- **Condition:** Bloco opcional que define condições específicas  

> 🔒 **Dica:** O `Deny` sempre tem precedência sobre qualquer `Allow`.

## Criando um Grupo (Group)

No console IAM, clique em **User groups → Create group**.

Preencha conforme abaixo:

| **Key** | **Value** |
|----------|------------|
| **User group name** | dev-group |

Selecione a policy **DevPolicy** criada anteriormente.  
Clique em **Create group**.

## Criando um Usuário (User)

No menu lateral, clique em **Users → Add users**.

Preencha as informações:

| **Key** | **Value** |
|----------|------------|
| **User name** | dev-user |
| **Access type** | Programmatic access + AWS Management Console access |
| **Password** | Custom password (defina uma senha de sua escolha) |

Desmarque **Require password reset**  
> 💡 Em um ambiente real, recomenda-se ativar essa opção.

Clique em **Next: Permissions**.  
Selecione o grupo **dev-group**.  
Clique em **Next: Tags → Next: Review → Create user**.

## Finalizando

- Baixe o arquivo `.csv` que contém a **Access Key** e a **Secret Access Key**.  
- Você também pode enviar instruções de login ao usuário criado.  
- Clique na **Sign-in URL** para acessar o console com o `dev-user`.  
- Faça login usando o **IAM user name** e **password**.

🎉 **Parabéns!**  
Você criou com sucesso identidades na sua conta AWS.  
No próximo capítulo, validaremos se as permissões configuradas funcionam corretamente.

## Testando o Acesso à Instância EC2

Faça login no console da **AWS** e verifique:
- O **usuário** e o **alias da conta**;  
- A **região da AWS** onde você criou as instâncias **EC2**.  

Acesse o console do serviço **EC2** e clique no menu **Instances**.  
Selecione a instância chamada **prod-instance** e clique em **Instance state → Stop instance**.  
Na janela que aparece, clique em **Stop**.  

⚠️ Um aviso aparecerá informando que **dev-user não tem permissões** para realizar a operação de parar a instância EC2.  
Isso acontece porque, no laboratório anterior, configuramos que esse usuário **só pode atuar em instâncias EC2** com o par chave-valor **Env=dev**.

Agora selecione a instância chamada **dev-instance**, clique em **Instance state → Stop instance**, e depois clique em **Stop**.  

Após alguns segundos, você verá que a instância **dev** foi parada com sucesso. ✅  

🎉 **Ótimo trabalho!**  
Você testou corretamente as permissões de acesso às instâncias EC2.  
No próximo capítulo, aprenderemos sobre **Roles** para recursos da AWS!

---

## 💡 Dica — IAM Policy Simulator (Opcional)

No mundo real, você pode não querer desligar sua instância EC2 apenas para testar sua **policy**.  
O **IAM Policy Simulator** é uma ferramenta que permite examinar e validar as permissões configuradas em suas políticas IAM.

Neste passo, testaremos a permissão do **dev-group** simulando as ações `DeleteTags` e `StopInstances` usando o **IAM Policy Simulator**.  
👉 Pular este passo **não afeta os próximos laboratórios**.

1. Acesse o **[IAM Policy Simulator](https://policysim.aws.amazon.com/)**.  
2. Selecione **dev-group**.  
3. Selecione as ações **DeleteTags** e **StopInstances** para testar.  

Você verá que ambas as ações foram **negadas**, mas por razões diferentes:  
- **DeleteTags** → negada por *1 matching statement*;  
- **StopInstances** → negada por *Implicitly denied (no matching statements)*.  

Expanda **DeleteTags** e clique em **Show statement** na policy **DevPolicy**.  
Isso destacará quais statements correspondem à ação simulada.

Expanda **StopInstances** — a ação foi negada porque o recurso da simulação é `"*"`.  
O **dev-group** só pode parar instâncias EC2 que possuem a tag **Env=dev**.  

Agora adicione a tag **Env=dev** à instância e teste novamente para validar a policy corretamente.  

---

🎯 **Conclusão:**
Você acabou de testar sua **IAM Policy personalizada** sem interferir no seu ambiente!  

Com o **IAM Policy Simulator**, você pode testar e realizar troubleshooting de:
- Políticas baseadas em identidade;  
- Fronteiras de permissões do IAM;  
- **Service Control Policies (SCPs)** do AWS Organizations;  
- Políticas baseadas em recursos.  

Saiba mais [neste link](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_testing-policies.html).

## IAM Role para EC2

Faça login na conta **AWS** com o usuário com **permissões de administrador** (não o `dev-user`) antes de iniciar este capítulo.

<img width="1423" height="797" alt="image" src="https://github.com/user-attachments/assets/e57d6c10-8e1d-42f8-975e-cc00369446fa" />

## 🪣 Criando Buckets no Amazon S3

1. Acesse o console do **Amazon S3**.  
2. Clique em **Create bucket**.  
3. Insira um **nome de bucket único** no campo **Bucket name**.  
   - Exemplo: `iam-test-user_name`  
   - ⚠️ *Os nomes de bucket devem ser únicos globalmente.*
4. Clique em **Create bucket**, deixando as demais configurações padrão.  
5. Faça **upload de qualquer arquivo** (exemplo: `aws-logo-picture`) neste bucket.

Repita o processo para criar outro bucket chamado `iam-test-other-user_name` e faça upload de outro arquivo nele.

## 👤 Criando uma IAM Role para EC2

Acesse o console do **IAM**.  

### 1. Criar uma Policy

1. No painel esquerdo, vá em **Access Management → Policies**.  
2. Clique em **Create policy** → selecione a aba **JSON**.  
3. Cole o seguinte conteúdo, **substituindo** `iam-test-user_name` pelo nome real do seu bucket:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": ["s3:ListAllMyBuckets", "s3:GetBucketLocation"],
            "Effect": "Allow",
            "Resource": ["arn:aws:s3:::*"]
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:Get*",
                "s3:List*"
            ],
            "Resource": [
                "arn:aws:s3:::iam-test-user_name/*",
                "arn:aws:s3:::iam-test-user_name"
            ]
        }
    ]
}
```

No arquivo **JSON** criado anteriormente, **substitua o valor de `Resource`** pelo **nome real do bucket do Amazon S3** que você criou.

Em seguida:

1. Pule a etapa de adicionar tags.  
2. Clique em **Next: Review**.  
3. No campo **Name**, digite `IAMBucketTestPolicy`.  
4. Clique em **Create policy** para criar a política que será anexada a uma IAM Role.

---

## 🎭 Criando a Role para EC2

1. No console do **IAM**, acesse **Roles → Create role**.  
2. Mantenha o tipo de *Trusted entity* como **AWS service**.  
3. Escolha **EC2** como *Service* ou *Use case*.  
4. Clique em **Next**.

### Associando a Policy

1. Pesquise a política chamada `IAMBucketTestPolicy`.  
2. Marque a caixa de seleção para adicioná-la à role.  
3. Clique em **Next**.  
4. No campo **Role name**, digite `IAMBucketTestRole`.  
5. Clique em **Create role**.

Agora a role está pronta para ser anexada à instância EC2.

---

## 🖥️ Anexando a Role à instância EC2

1. Acesse o **console EC2**.  
2. Caso não veja suas instâncias, **verifique se a região da AWS está correta** no canto superior direito.  
3. Selecione a instância **`prod-instance`**.  
4. Clique em **Connect → EC2 Instance Connect → Connect**.

### Testando antes de anexar a Role

No terminal da instância, digite o comando abaixo: ` aws s3 ls `  
Você não verá a lista de buckets S3 — isso acontece porque a instância ainda não tem permissões IAM configuradas.  

## Atribuindo a Role à instância

Volte à página de Instâncias no console EC2.
Selecione `prod-instance`.  
Clique em Actions → Security → Modify IAM role.
No campo de Role, selecione `IAMBucketTestRole`.  
Clique em Save para salvar as alterações.

## ✅ Testando novamente

Conecte-se novamente à instância e execute os comandos:
```
aws s3 ls
aws s3 ls iam-test-user_name
aws s3 ls iam-test-other-user_name
```

Resultados esperados:
O comando `aws s3 ls` agora mostra a lista de buckets S3.  
O comando `aws s3 ls iam-test-user_name` mostra os objetos do bucket com permissão.
O comando `aws s3 ls iam-test-other-user_name` retorna Access Denied, pois não há permissão para esse bucket.

🎉 Conclusão

Parabéns! 🎯
Você concluiu todo o laboratório prático sobre o IAM, aprendendo a:
Criar e ajustar policies personalizadas
Criar uma IAM Role para EC2
Anexar e testar permissões diretamente na instância
Você agora entende na prática como controlar o acesso de recursos EC2 ao S3 usando IAM Roles.

## 🧹 Desprovisionamento dos Recursos

Após concluir todos os testes e validações do laboratório, é importante **remover os recursos criados** para evitar custos desnecessários.

### 🖥️ EC2 — Instâncias
1. Acesse o console do **Amazon EC2**.  
2. Localize as instâncias criadas durante o laboratório:  
   - `dev-instance`  
   - `prod-instance`  
3. Selecione ambas e clique em **Instance State → Terminate instance**.  
4. Confirme a exclusão.

---

### 🧑‍💼 IAM — Identidades

Acesse o console do **IAM** e exclua os seguintes itens:

#### **User Groups**
- `dev-group`

#### **Users**
- `dev-user`

#### **Roles**
- `IAMBucketTestRole`

#### **Policies**
- `DevPolicy`  
- `IAMBucketTestPolicy`

---

### 🗂️ S3 — Buckets

1. Acesse o console do **Amazon S3**.  
2. Localize e exclua os buckets criados:
   - `iam-test-user_name`
   - `iam-test-other-user_name`

> ⚠️ **Atenção:** Antes de deletar o bucket `iam-test-user_name`, é necessário **esvaziá-lo**.  
> Use a opção **Empty** no console do S3 para remover todos os objetos antes de excluir o bucket.

---

## ✅ Conclusão

Você concluiu o **desprovisionamento completo dos recursos AWS** criados neste laboratório.  
Essa etapa é essencial para manter sua conta organizada e evitar cobranças indesejadas.  

Parabéns! 🎉  
Seu ambiente agora está limpo e pronto para novos experimentos.

---

## 🛠️ Laboratório Prático do AWS Config

O **AWS Config** é um serviço web que fornece uma visão detalhada da **configuração dos recursos AWS** na sua conta. Isso inclui:

- Como os recursos estão relacionados entre si;  
- Como eles foram configurados no passado;  
- Como as configurações e relacionamentos mudam ao longo do tempo.

---

### Por que usar o AWS Config?

Ao executar aplicações na AWS, você normalmente utiliza diversos recursos que precisam ser criados e gerenciados coletivamente.  

À medida que a demanda pela sua aplicação cresce, aumenta também a necessidade de **monitorar e controlar os recursos da AWS**.  

O **AWS Config** foi projetado para ajudá-lo a gerenciar os recursos da sua aplicação, oferecendo suporte em:

- **Administração de recursos**  
- **Auditoria e compliance**  
- **Resolução de problemas (troubleshooting)**  
- **Análise de segurança**

## ✅ Habilitando o AWS Config

Neste laboratório, vamos habilitar o **AWS Config**, que começará a registrar mudanças de configuração e histórico dos recursos AWS que você implantou.

<img width="721" height="292" alt="image" src="https://github.com/user-attachments/assets/190ddb1b-40fa-4a90-b040-f5913f89aff9" />

---

### Passo 1: Acessar o Console do AWS Config

1. Faça login no **Console de Gerenciamento da AWS**.  
2. Acesse o serviço **AWS Config** [clicando aqui](https://console.aws.amazon.com/config/).  

---

### Passo 2: Configuração com 1-Click Setup

1. Clique em **1-click setup** no console do AWS Config.  
2. Revise os detalhes da configuração do AWS Config.  
3. Aceite as configurações padrão e clique em **Confirm**.  

> Ao fazer isso, um **gravador do AWS Config** (AWS Config recorder) e um **canal de entrega** (delivery channel) serão criados.  
> As mudanças de configuração começarão a ser registradas e notificações serão enviadas para um **novo bucket do Amazon S3**.

---

### Passo 3: Visualizar o Dashboard

- Após a configuração ser concluída, você pode revisar o **Dashboard do AWS Config** para verificar o status e as mudanças registradas nos recursos.

---

### Conclusão

🎉 Ótimo trabalho!  
Você concluiu a configuração do **AWS Config**.  

No próximo passo, você criará um **Amazon SNS Topic** para publicar mensagens de notificação.

---

# 📬 Criar Tópico Amazon SNS

## Visão Geral

O **Amazon Simple Notification Service (Amazon SNS)** é um serviço totalmente gerenciado que permite o envio de mensagens de **publicadores** para **assinantes**.  
Os publicadores enviam mensagens para um **tópico**, que pode ser assinado por diferentes endpoints, como email.

Neste laboratório, vamos:

1. Criar um tópico SNS;  
2. Assinar o tópico usando um endpoint de email.

---

## Passos

1. Faça login no **Console Amazon SNS** na AWS.  

2. Crie um novo tópico:  
   - Nome do tópico: `config-lab`  
   - Clique em **Next step**.  

3. Tipo de tópico: **Standard**  
   - Mantenha os demais valores padrão e clique em **Create topic**.  

4. Na aba **Subscriptions**, clique em **Create subscription**.  

5. Configure a assinatura:  
   - **Protocol:** Email  
   - **Endpoint:** Digite um email válido  
   - Clique em **Create subscription**  

6. A assinatura será criada com status **Pending confirmation**.  

7. Verifique seu email, procure a mensagem de **AWS Notifications** e clique em **Confirm subscription**.  
   - Uma aba do navegador será aberta mostrando: **Subscription confirmed!**  

8. No console Amazon SNS, selecione o tópico e copie o **Topic ARN** para uso posterior.  

---

✅ Parabéns!  
Você criou um **Tópico Amazon SNS** e confirmou a assinatura por email.  
O próximo passo será criar uma **IAM Role**.

---

# 🔑 Criar IAM Role para Remediação Automatizada

## Visão Geral

O **AWS Identity and Access Management (IAM)** é um serviço que ajuda a controlar o acesso aos recursos da AWS de forma segura.  
Uma **IAM Role** é uma identidade IAM que você pode criar na sua conta com **permissões específicas**.  
As roles permitem delegar acesso a usuários, aplicações ou serviços que normalmente não têm acesso direto aos recursos da AWS.

Neste laboratório, você irá criar uma **IAM Role** que permitirá ao **AWS Config** realizar remediação automatizada e publicar mensagens no tópico SNS criado no laboratório anterior.

---

## Passos

1. Faça login no **AWS IAM Console**.  

2. Clique em **Roles** no menu lateral e depois em **Create role**.  

3. **Tipo de entidade confiável:** selecione **AWS service** (padrão)  
   - Em **Use case**, pesquise e selecione **Systems Manager**  
   - Clique em **Next**  

4. **Permissões:**  
   - Pesquise e selecione:  
     - **AmazonSNSFullAccess**  
     - **AmazonSSMAutomationRole**  
   - Clique em **Next**  

> ⚠️ Observação: Essas políticas são permissivas para demonstração. Em produção, utilize o princípio de **least privilege**.

5. **Nome da role:** `AutomationServiceRole`  
   - Role criada com sucesso, clique em **Create role**  

6. Para revisar a role criada:  
   - Clique em **Roles** no menu lateral  
   - Selecione `AutomationServiceRole`  
   - Copie o **Role ARN** e salve para uso posterior  

---

✅ Parabéns!  
Você criou uma **IAM Role** que será usada no próximo laboratório para configurar uma regra do AWS Config com **remediação automatizada**.

---
# ⚙️ Ativar Regra do AWS Config e Configurar Remediação Automatizada

## Visão Geral

As **AWS Config rules** representam as configurações ideais para os recursos da sua conta AWS.  
Enquanto o **AWS Config** monitora continuamente as alterações de configuração, ele verifica se essas mudanças estão em conformidade com as regras definidas. Recursos que **não cumprem a regra** são marcados como **noncompliant**.

O AWS Config oferece **AWS managed rules**, que são regras pré-definidas e personalizáveis para verificar as melhores práticas.

Além disso, é possível configurar **remediação automatizada** de recursos não conformes usando o **AWS Systems Manager Automation**.

Neste laboratório, vamos:

- Adicionar uma **AWS managed rule** (`desired-instance-type`) para EC2  
- Configurar **remediação automatizada** que publica mensagens no **SNS Topic** criado anteriormente

---

## Passos

### 1. Criar a regra AWS Config

1. Faça login no **AWS Config Console**.  
2. Selecione **Rules** e clique em **Add rule**.  
3. Tipo de regra: selecione **Add AWS managed rule**  
4. Pesquise por `desired-instance-type` e selecione a opção encontrada  
5. Clique em **Next**  

### 2. Configurar parâmetros da regra

- Deixe os campos padrão em **Details** e **Evaluation mode**  
- Em **Parameters**, no campo `instanceType`, insira: `t3.micro`  
- Clique em **Next**  
- Revise os detalhes da regra e clique em **Save**

---

### 3. Configurar remediação automatizada

1. Selecione a regra criada e clique em **Actions → Manage remediation**  
2. Em **Select remediation method**, escolha **Automatic remediation**  
3. Em **Remediation action details**, pesquise e selecione **AWS-PublishSNSNotification**  
4. Configure os parâmetros:  
   - `TopicArn`: ARN do SNS Topic criado anteriormente  
   - `Message`: insira uma mensagem de exemplo  
   - `AutomationAssumeRole`: ARN da IAM Role criada no passo anterior  
5. Clique em **Save changes**

---

✅ Parabéns!  
Você adicionou uma **AWS Managed Config rule** e configurou **remediação automatizada** para publicar em um **SNS topic**.  
No próximo passo, você testará a regra e a automação do AWS Config.

---

# 🚀 Iniciar Instância EC2 para Testar Remediação da Regra do AWS Config

## Visão Geral

O **Amazon Elastic Compute Cloud (Amazon EC2)** fornece capacidade de computação escalável sob demanda na nuvem da AWS.

Neste laboratório, você irá:

- Iniciar uma instância **Amazon Linux 2023** com um **tipo de instância diferente** do parâmetro definido na regra AWS Config criada anteriormente.  
- O **AWS Config** avaliará esse recurso como **noncompliant** e acionará a **remediação automatizada**, publicando no SNS Topic criado.

---

## Passos

### 1. Iniciar instância EC2

1. Faça login no **AWS EC2 Console**.  
2. No painel do EC2, clique em **Launch instance**.  
3. Para **Name**, insira: `config-lab-test`  
4. Em **Application and OS Images**, mantenha o padrão **Amazon Linux 2023**  
5. Em **Instance type**, selecione um tipo **diferente** do definido na regra AWS Config, por exemplo: `t2.micro`  
6. Em **Key pair (login)**, selecione **Proceed without a key pair** (não será necessário acessar esta instância)  
7. Em **Network settings**, desmarque a opção **Allow SSH traffic from**  
8. Clique em **Launch instance**

---

### 2. Conferir status da instância

1. Você verá a confirmação de que a instância foi iniciada  
2. Clique no **Instance ID** na mensagem para visualizar detalhes no console EC2  
3. Aguarde até o **status check** mostrar **2/2 checks passed** (pode ser necessário atualizar a página)

---

### 3. Verificar avaliação do AWS Config

1. Abra o **AWS Config Console**  
2. Selecione **Rules** e clique na regra criada anteriormente  
3. Em **Resources in scope**, você verá:  
   - **EC2 Instance ID**  
   - **Status:** Action executed successfully  
   - **Compliance:** Noncompliant

---

### 4. Conferir notificação por e-mail

- Verifique seu e-mail para uma mensagem do **AWS Notifications** confirmando que a ação automatizada publicou no SNS Topic

---

## ✅ Recapitulando

- Você iniciou uma instância EC2 que **não estava em conformidade** com a regra do AWS Config.  
- O **AWS Config** marcou a instância como **noncompliant** e acionou a remediação automatizada.  
- O **Systems Manager Automation** publicou uma notificação no **SNS Topic**.  
- Você recebeu a confirmação por e-mail da execução bem-sucedida.  

Este exercício mostra como o **AWS Config** pode monitorar a conformidade de recursos e acionar automaticamente ações de remediação.

---

# 🧹 Limpeza de Recursos

## Passos para desprovisionar recursos do laboratório

### 1. Encerrar instância EC2
- Faça login no **AWS EC2 Console**  
- Encerre a instância `config-lab-test` clicando em **Terminate**

---

### 2. Remover ação de remediação do AWS Config
- Faça login no **AWS Config Console**  
- Acesse a regra criada e **delete a Remediation action**

---

### 3. Excluir a regra do AWS Config
- No **AWS Config Console**, selecione a regra criada e clique em **Delete rule**

---

### 4. Excluir tópico do Amazon SNS
- Faça login no **Amazon SNS Console**  
- Selecione o tópico `config-lab` e clique em **Delete topic**

---

### 5. Excluir role do IAM
- Faça login no **AWS IAM Console**  
- Selecione a role `AutomationServiceRole` e clique em **Delete role**

🎉 Todos os recursos do laboratório foram removidos com sucesso.

---

## AWS CloudTrail Hands-on Lab

O **AWS CloudTrail** é um serviço da AWS que ajuda a habilitar auditoria operacional e de riscos, governança e conformidade da sua conta AWS. Ações realizadas por um usuário, role ou serviço da AWS são registradas como **eventos** no CloudTrail. Esses eventos incluem ações feitas no **AWS Management Console**, **AWS CLI** e **AWS SDKs e APIs**.

Você tem acesso automático ao **Histórico de Eventos** do CloudTrail ao criar sua conta AWS. Este histórico fornece um registro visualizável, pesquisável, baixável e imutável dos **últimos 90 dias de eventos de gerenciamento** em uma região AWS.

Para manter um registro contínuo dos eventos além de 90 dias, crie uma **trail** ou um **CloudTrail Lake event data store**.

### Componentes principais do CloudTrail
- **CloudTrail Events**  
- **Event History**  
- **Trails**

Para aprender a configurar o CloudTrail visando segurança e conformidade, confira o documento [CloudTrail security](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-security.html).

---

## Objetivo do Laboratório

Neste hands-on lab, criaremos uma **arquitetura orientada a eventos** que será acionada com base no estado de uma instância **Amazon EC2**, usando uma **regra do Amazon EventBridge** que envia notificações por **Amazon SNS** para informar o estado da instância.

### Arquitetura do laboratório
- **Fonte do evento:** Estado da instância EC2  
- **Mecanismo de eventos:** Amazon EventBridge  
- **Destino:** Tópico Amazon SNS → envio de e-mail  

<img width="1000" height="420" alt="image" src="https://github.com/user-attachments/assets/d5f0e64a-c6cf-4a69-999f-ed900ce2f715" />

## Partes do Hands-on Lab
1. Lançar instância EC2  
2. Criar tópico Amazon SNS  
3. Criar regra Amazon EventBridge  
4. Limpeza de recursos  

---

# Lançar Instância EC2

## Amazon Elastic Compute Cloud (Amazon EC2)

O **Amazon EC2** fornece capacidade de computação escalável e sob demanda na **nuvem da AWS**.

Neste laboratório, você lançará uma instância **Amazon Linux 2023**, que será usada em laboratórios posteriores para testar a arquitetura orientada a eventos.

---

## Passos para lançar a instância

1. Faça login no **AWS Amazon EC2** no console de gerenciamento da AWS.

2. No **dashboard do EC2**, clique em **Launch instance**.

3. No campo **Name**, insira um nome, por exemplo: `cloudtrail-lab-test`.

4. Para **Application and OS Images**, mantenha os padrões para **Amazon Linux 2023**.

5. Para **Instance type**, use o tipo padrão **t2.micro**.

6. Para **Key pair (login)**, selecione **Proceed without a key pair**, pois não será necessário conectar à instância.

7. Em **Network settings**, desmarque a opção **Allow SSH traffic from**.

8. Clique em **Launch instance**.

9. Você verá a confirmação de que o lançamento da instância foi iniciado. Clique no **Instance ID** no banner de mensagens para visualizar a instância no console do EC2.

---

🎉 **Ótimo trabalho!**  
Nas próximas etapas, você criará a arquitetura orientada a eventos utilizando esta instância EC2.

---

# Criar Tópico Amazon SNS

## Amazon Simple Notification Service (Amazon SNS)

O **Amazon SNS** é um serviço totalmente gerenciado que entrega mensagens de **publicadores** para **assinantes**.  
Os publicadores enviam mensagens para um tópico, que pode ser assinado por diferentes endpoints, como e-mail.

Neste laboratório, criaremos um tópico SNS e assinaremos este tópico usando um endpoint de e-mail.

---

## Passos para criar o tópico SNS

1. Faça login no **Amazon SNS Console** no console de gerenciamento da AWS.

2. No campo **Name**, insira um nome para o tópico, por exemplo: `cloudtrail-lab`.  
   Clique em **Next step**.

3. Para **Topic Type**, selecione **Standard**.  
   Mantenha as demais configurações padrão e clique em **Create topic**.

4. Certifique-se de estar na aba **Subscriptions** do console do tópico.  
   Clique em **Create subscription**.

5. Selecione **Email** como **Protocol**.

6. Insira um e-mail válido no campo **Endpoint**.  
   Clique em **Create subscription**.

7. A assinatura será criada e o **Status** aparecerá como **Pending confirmation**.

8. Após alguns minutos, verifique seu e-mail. Abra a mensagem da AWS Notifications e clique em **Confirm subscription**.  
   Uma aba do navegador será aberta mostrando a mensagem **Subscription confirmed!**.

---

🎉 **Ótimo trabalho!**  
Você criou um tópico SNS e confirmou uma assinatura de e-mail.  
Na próxima etapa, você criará uma **regra Amazon EventBridge** que terá como destino este tópico para testar a arquitetura.

---

# Criar Regra Amazon EventBridge e Testar com Instância EC2

## Amazon EventBridge

O **Amazon EventBridge** é um serviço serverless da AWS que conecta componentes de aplicação usando eventos, facilitando a criação de aplicações escaláveis baseadas em eventos.  
Você pode definir regras (**rules**) que especificam quais eventos devem ser enviados a quais **targets** para processamento.

Neste laboratório, criaremos uma regra EventBridge que monitora um padrão específico de evento e, ao receber um evento correspondente, envia para o tópico SNS criado anteriormente.  
Depois, testaremos a arquitetura alterando o estado da instância EC2 criada anteriormente.

---

## Passos para criar a regra EventBridge

1. Faça login no **Amazon EventBridge Console**.

2. No menu **Rules**, clique em **Create rule**.

3. Configure a regra:
   - **Name**: `IMD-Eventbridge-Rule`
   - **Event bus**: default
   - **Rule type**: Rule with an event pattern  
   Clique em **Next**.

4. Deixe **Event source** como padrão: **AWS events or EventBridge partner events**.

5. Em **Event pattern**:
   - **Creation method**: Use pattern form
   - **Event source**: AWS services
   - **AWS service**: EC2
   - **Event type**: EC2 Instance State-change Notification
   - **Specific state(s)**: marque **stopping** e **stopped**  
   Clique em **Next**.

6. Em **Select target(s)** para **Target 1**:
   - **Target type**: AWS service
   - **Select a target**: SNS topic
   - Escolha o tópico `cloudtrail-lab` criado anteriormente
   - **Permissions**: deixe marcado **Use execution role** e **Create a new role for this specific resource**  
   Clique em **Skip to Review and Create**.

7. Revise a configuração da regra e clique em **Create rule**.

---

## Testando a arquitetura com EC2

1. Acesse o **console EC2**.  
   Selecione **Instances**, escolha `cloudtrail-lab-test` e clique em **Stop instance** no dropdown **Instance state**. Confirme a ação.

2. Conforme a instância muda de estado (**Running → Stopping → Stopped**), um evento será enviado para o EventBridge.  
   A regra criada detectará o evento e acionará o tópico SNS, enviando um e-mail de notificação.

3. Verifique seu e-mail. Você receberá uma mensagem da AWS Notification Message, incluindo informações do estado da instância que disparou a arquitetura baseada em eventos.

---

🎉 **Ótimo trabalho!**

### Recapitulação

- Você criou um tópico SNS e assinou seu e-mail para notificações.  
- Criou uma regra EventBridge para monitorar mudanças de estado da instância EC2 (stopping e stopped).  
- Testou a arquitetura acionando um evento com a EC2 e recebeu a notificação por e-mail.  

Essa experiência prática demonstra como construir aplicações baseadas em eventos usando serviços AWS como **EC2**, **SNS** e **EventBridge**.

---

## Limpeza de Recursos

1. Faça login no **AWS Amazon Elastic Compute Cloud (EC2)** e **terminate** a instância `cloudtrail-lab-test`.

2. Faça login no **Amazon EventBridge Console**, localize a regra criada e clique em **Delete**.  
   Uma janela pop-up aparecerá solicitando etapas adicionais para confirmar a exclusão.

3. Faça login no **Amazon SNS Console** e exclua o tópico `cloudtrail-lab`.  
   Uma janela pop-up aparecerá solicitando etapas adicionais para confirmar a exclusão.

---

## Monitoramento - Amazon CloudWatch

<img width="2459" height="761" alt="image" src="https://github.com/user-attachments/assets/235b4105-e983-41d0-9459-351ae2cf64b0" />

O **Amazon CloudWatch** é um serviço de monitoramento e observabilidade criado para engenheiros de DevOps, desenvolvedores, engenheiros de confiabilidade de sites (SREs) e gerentes de TI. O CloudWatch fornece dados e insights acionáveis para:

- Monitorar seus aplicativos
- Responder a mudanças de desempenho em todo o sistema
- Otimizar a utilização de recursos
- Obter uma visão unificada da integridade operacional

O CloudWatch coleta dados operacionais e de monitoramento na forma de **registros**, **métricas** e **eventos**, fornecendo uma visão unificada dos recursos, aplicativos e serviços da AWS executados na nuvem e em servidores locais. Com ele, você pode:

- Detectar comportamentos anômalos em seus ambientes
- Definir alarmes
- Visualizar registros e métricas lado a lado
- Realizar ações automatizadas
- Solucionar problemas
- Descobrir insights para manter seus aplicativos funcionando sem problemas

## Casos de Uso

### Monitoramento e solução de problemas de infraestrutura
- Monitore métricas e registros críticos
- Visualize sua pilha de aplicativos e infraestrutura
- Crie alarmes e correlacione métricas e registros para identificar causas raiz de problemas de desempenho
- Inclui monitoramento de ecossistemas de contêineres no **Amazon ECS**, **AWS Fargate**, **Amazon EKS** e **Kubernetes**

### Monitoramento de aplicativos
- Monitore aplicativos executados na AWS (EC2, contêineres, serverless) ou no local
- Colete dados em todas as camadas da pilha de desempenho
- Visualize métricas e registros em **painéis automáticos**

### Análise de registros
- Explore, analise e visualize registros para resolver problemas operacionais e melhorar o desempenho de aplicativos
- Realize consultas para identificar rapidamente as causas de problemas
- Use uma linguagem de consulta específica para acelerar a investigação de falhas

### Melhore o desempenho operacional e a otimização de recursos
- Defina alarmes e automatize ações com base em limites predefinidos ou algoritmos de aprendizado de máquina que detectam anomalias
- Exemplos de ações automatizadas:
  - Iniciar **Amazon EC2 Auto Scaling**
  - Interromper instâncias para reduzir custos
- Use **CloudWatch Events** para serverless para acionar fluxos de trabalho com **AWS Lambda**, **Amazon SNS** e **AWS CloudFormation**

---

## Amazon CloudWatch

## Visão Geral do Amazon CloudWatch

<img width="792" height="137" alt="image" src="https://github.com/user-attachments/assets/cf8d92e2-8e21-44bb-a4b2-faa2ec56289f" />

Neste laboratório, vamos utilizar o **Amazon CloudWatch** para rastrear a utilização de CPU de uma instância **EC2** e configurar um alarme baseado em um limiar definido. Quando o limite for atingido, o alarme disparará uma notificação via **Amazon Simple Notification Service (Amazon SNS)**.

### Estrutura do Laboratório

Este laboratório é dividido nas seguintes partes:

1. **Criação de um tópico no Amazon SNS**
2. **Execução de uma instância do Amazon EC2**
3. **Configuração de um alarme do Amazon CloudWatch**

---

## Criar Tópico do SNS

Primeiro, criaremos um tópico para notificar nosso endereço de e-mail, que será posteriormente anexado ao alarme. Um tópico do **Amazon SNS** é um ponto de acesso lógico que atua como um canal de comunicação.

### Passos

1. No **Console da AWS**, clique em **Amazon SNS**.

2. No lado esquerdo da página, selecione **Tópicos** ou clique em **Próxima etapa**.

3. A tela **Criar tópico** será aberta:
   - Para **Tipo**, selecione **Padrão**.
   - No campo **Nome**, digite um nome para o tópico (inclua seu nome, se desejar).
   - Opcionalmente, defina um **Nome de exibição**.
   - Role até o final da página e clique em **Criar tópico**.

4. Após a criação, você será direcionado ao painel do tópico. Clique em **Criar assinatura** no lado direito.

5. No campo **Protocolo**, selecione **E-mail** e insira um endereço de e-mail ativo.  
   > Utilize um e-mail não comercial caso filtros de spam bloqueiem mensagens do SNS.

6. Clique em **Criar assinatura**.

7. Um e-mail de verificação será enviado com o assunto:  
   **“Notificação da AWS — Confirmação de assinatura”**.  
   Abra o e-mail e clique no link **Confirmar assinatura**.

8. Após confirmação, o status da assinatura mudará de **“Confirmação pendente”** para **“Confirmada”**.

---

# Execute uma instância EC2

## Inicie uma instância do Elastic Compute Cloud (EC2)

Nesta etapa, você iniciará uma instância do **EC2** e configurará os **dados do usuário** para instalar e iniciar uma ferramenta de estresse. A ferramenta começará a simular carga de CPU **5 minutos após a inicialização** da instância, permitindo que você configure o alarme do CloudWatch.

### Passos

1. Faça login na página do **Amazon EC2** e clique em **EC2 Dashboard** no menu à esquerda.

2. Clique em **Iniciar instância** e escolha **Iniciar instância**.

3. Na seção **Nome e tags**, digite um nome para sua instância.  
   - Este nome aparecerá no console e ajuda a identificar a instância em ambientes complexos.  
   - Formato sugerido: `[Seu nome] Servidor`.

4. Na seção **Imagens do aplicativo e do sistema operacional (Amazon Machine Image)**:
   - Selecione **Amazon Linux**.
   - Escolha **64 bits (x86)** na lista suspensa **Arquitetura**.

5. Selecione o tipo de instância **t2.micro**.

6. Na seção **Configuração de rede**:
   - Selecione **Criar grupo de segurança**.
   - Marque todas as caixas de seleção que permitem tráfego.

7. Configure o **script de inicialização** para instalar e iniciar a ferramenta de estresse:
   - Expanda **Detalhes avançados**.
   - No campo **Dados do usuário**, insira o seguinte script:

```sh
#!/bin/sh
yum -y update
amazon-linux-extras install epel -y
yum install stress -y
stress -c 1 --backoff 300000000 -t 30m
```

O script cria uma carga de CPU usando a ferramenta stress por 30 minutos, iniciando 5 minutos após a inicialização.

8. No painel de resumo, clique em Iniciar instância.

9. Na janela Criar par de chaves, selecione Prosseguir sem par de chaves e clique em Continuar sem par de chaves.

10. Clique novamente em Iniciar instância e, em seguida, em Exibir todas as instâncias para ver a lista de instâncias EC2.
    Após a inicialização, você verá seu servidor e a zona de disponibilidade em que a instância está localizada.

---

# Configurar um Alarme do CloudWatch

## Passos para Configuração

1. No console do **EC2**, selecione a caixa de verificação ao lado do nome da sua instância para ver detalhes.  
   Clique em: **Ações >> Monitorar e solucionar problemas >> Gerenciar monitoramento detalhado**.

2. Marque **Ativar** em **Monitoramento detalhado** para fornecer dados em intervalos de **1 minuto** (padrão é 5 minutos).  
   Clique em **Salvar**.

3. Clique na guia **Detalhes** e copie o **ID da instância** para uso posterior.

4. Clique em: **Ações >> Monitorar e solucionar problemas >> Gerenciar alarmes do CloudWatch**.  
   Selecione **Criar um alarme**.

5. Em **Notificação de alarme**, selecione o **tópico SNS** criado anteriormente.

6. Na seção **Limites de alarme**, configure os valores desejados e clique em **Criar**.

7. No console da AWS, vá em **Serviços > CloudWatch**.

8. Clique em **Alarmes** no painel esquerdo e selecione **Todos os alarmes**.  
   - O estado do alarme inicialmente mostrará **INSUFFICIENT_DATA** e depois mudará para **OK**.  
   - É possível filtrar os alarmes usando o **ID da instância**.

9. No console do CloudWatch, vá em **Métricas** no painel esquerdo:  
   - Selecione **Todas as métricas** e filtre pelo **ID da instância**.  
   - Clique em **Métricas por instância** e selecione **CPUUtilization** (utilização da CPU).

10. Clique em **Métricas gráficas** e configure:  
    - **Período:** 1 minuto  
    - **Intervalo do gráfico:** 30 minutos  
    - **Atualização automática:** 1 minuto

11. Após aproximadamente **5 minutos**, a ferramenta de estresse iniciará a simulação de carga da CPU.  
    - O alarme será acionado quando o limite configurado for atingido.  
    - Você poderá visualizar o estado do alarme no console do CloudWatch em **Alarmes > Todos os alarmes**.  
    - Se configurado, receberá uma **notificação por e-mail** do SNS.

🎉 **Parabéns!** Você configurou com sucesso um alarme do CloudWatch para monitorar a utilização de CPU da sua instância EC2.

---

# Desprovisionamento dos Recursos

## Desprovisionando os Recursos

Após finalizar o laboratório, certifique-se de excluir os seguintes recursos:

- **CloudWatch Alarm**  
- **Instância EC2**  
- **Tópico do Amazon SNS**

### Excluir CloudWatch Alarm

1. No console do **Amazon CloudWatch**, selecione o alarme criado neste laboratório.  
2. No menu **Actions**, selecione **Delete**.  
3. Uma janela de confirmação aparecerá. Clique em **Delete**.

### Excluir Instância EC2

1. No console do **Amazon EC2**, selecione a instância criada neste laboratório.  
2. No menu **Instance state**, selecione **Terminate instance**.  
3. Uma janela de confirmação aparecerá. Clique em **Terminate**.

### Excluir Tópico SNS

1. No console do **Amazon SNS**, selecione o tópico criado neste laboratório.  
2. Clique em **Delete**.  
3. Uma janela de confirmação aparecerá. Para confirmar, digite `delete me` e clique em **Delete**.

---

## Banco de Dados - Amazon RDS

<img width="1420" height="798" alt="image" src="https://github.com/user-attachments/assets/b16c0402-4cce-440f-8e1b-3d5c8c35b116" />

A AWS oferece a seleção mais ampla de **bancos de dados de propósito específico** para todas as suas aplicações. Com mais de **15 mecanismos de bancos de dados**, a AWS permite que você **economize, cresça e inove mais rápido**.

Centenas de milhares de clientes utilizam os bancos de dados da AWS, que possuem as seguintes características:

- Bancos de dados com propósito específico;
- Performance em escala;
- Completamente gerenciados;
- Seguros e altamente disponíveis.

Clientes de diversos setores recorrem aos bancos de dados criados especificamente pela AWS para potencializar seus aplicativos mais importantes, incluindo:

- Aplicações em escala de Internet;
- Aplicações em tempo real;
- Aplicativos de código aberto;
- Aplicações Enterprise.

---

## Amazon RDS MySQL


O Amazon RDS é um serviço que torna fácil **configurar, operar e escalar um banco de dados relacional na nuvem**. Ele oferece **capacidade redimensionável** enquanto a AWS realiza tarefas de gerenciamento de bancos de dados, permitindo que você foque em suas aplicações e no seu negócio.

Este laboratório requer que você tenha executado o **laboratório EC2 Linux** previamente. Utilizaremos o servidor web criado anteriormente para conectar a uma instância do **Amazon RDS MySQL**.

<img width="601" height="522" alt="image" src="https://github.com/user-attachments/assets/38448d77-cd10-4305-9424-6b88fa10f405" />

## Passos do Laboratório

1. Crie um security group.
2. Execute uma instância do Amazon RDS.
3. Salve as credenciais do Amazon RDS.
4. Configure a instância do Amazon EC2 para conectar ao Amazon RDS.
5. Crie um snapshot do Amazon RDS (Opcional).
6. Modifique o tamanho da instância do Amazon RDS (Opcional).
7. Desprovisionamento dos recursos.

---

## Crie um Security Group

## Pré-requisito
- EC2 Linux: Você deve ter executado instâncias EC2 com um security group chamado **Immersion Day - Web Server** que permite requisições TCP na porta 80 para o servidor web.

## Criando o Security Group para o Banco de Dados

1. No console da **VPC**, clique em **Security Groups** e depois no botão **Create Security Group**.
2. Digite o nome e a descrição do security group, mantendo a configuração da VPC para a mesma VPC onde sua instância EC2 foi executada.

| Chave                  | Valor                     |
|------------------------|---------------------------|
| Security group name    | Immersion Day DB Tier     |
| Description            | Immersion Day DB Tier     |
| VPC                    | VPC-xxxxxx (default)      |

3. Em **Inbound Rules**, clique em **Add rule**.
4. Adicione uma regra **Inbound** para instâncias EC2 na camada web:
   - **Type:** MySQL/Aurora (3306)  
   - **Protocol:** TCP  
   - **Source:** digite o nome do security group da sua instância EC2. Enquanto digita, uma lista de security groups correspondentes aparecerá. Selecione o correto.
5. Configure a **tag Name** como `Immersion Day DB Tier`.
6. Role para baixo e clique em **Create security group**.

Isso criará o security group para a sua instância RDS.

# Execute uma instância do Amazon RDS

Agora que nosso **security group** para a camada de banco de dados está preparado, vamos configurar e criar uma instância do **Amazon RDS MySQL**.

## Passo a passo

1. Faça login no console da AWS e abra a página do serviço **Amazon RDS**.
2. Clique em **Create database**.
3. Em **Choose a database creation method**, selecione **Standard** para ter configurações mais granulares.
   - **Easy Create** oferece configurações de melhores práticas recomendadas, mas não será usada neste laboratório.
4. Em **Engine Options**, selecione **MySQL**.
   - Para este laboratório, selecione a versão **MySQL 5.7.X**.
5. Em **Template**, selecione **Free Tier**.
   - Outras opções são **Production** e **Dev/Test**.
6. Na seção **Settings**, preencha os campos:

| Parâmetro           | Valor        |
|--------------------|-------------|
| DB Instance Identifier | awsdb      |
| Master Username        | awsuser    |
| Master Password        | awspassword|

7. Na seção **DB Instance size**, para **DB instance class**, selecione **db.t2.micro** (automático no Free Tier).
8. Em **Storage**, selecione **General Purpose SSD**.
   - Você pode optar por habilitar ou desabilitar **Auto Scaling**.
9. Configuração **Multi-AZ** não é necessária para Free Tier; para Production/Dev-Test, é recomendada.
10. Na seção **Connectivity**, configure:

| Parâmetro                  | Valor                                  |
|----------------------------|----------------------------------------|
| VPC                        | Default VPC                             |
| Subnet Group               | default                                 |
| Publicly accessible        | No                                      |
| VPC Security Group(s)      | Escolha **Immersion Day DB Tier**       |
| Availability Zone          | No preference                           |
| Database port              | 3306                                     |

11. Em **Database authentication**, selecione **Password Authentication**.
12. Expanda **Additional Configuration** e configure **Database options**:

| Parâmetro                  | Valor                         |
|----------------------------|-------------------------------|
| Initial database name       | immersionday                  |
| DB parameter group & Option group | default.mysql5.7        |

13. Em **Backup**:
   - Habilite **Enable automatic backups**
   - **Backup retention period:** 7 days
   - **Backup Window:** No preference
14. Em **Log exports**, **Maintenance** e **Deletion protection**, deixe as configurações padrão.
15. Revise suas configurações e clique em **Create database**.
16. No dashboard do Amazon RDS, monitore sua nova instância até o status mudar de **Creating** → **Backing Up** → **Available**.  
   - Esse processo demora aproximadamente 5 minutos.

---

## Salve as credenciais do Amazon RDS no AWS Secrets Manager

O servidor web que você criou contém um catálogo de endereços simples como código de exemplo. Para que ele consiga conectar ao banco de dados, armazenaremos as credenciais no **AWS Secrets Manager**.

### Passo a passo

1. Abra o [AWS Secrets Manager](https://console.aws.amazon.com/secretsmanager/) e clique em **Store a new secret**.
2. Em **Secret Type**, selecione **Credentials for Amazon RDS database**.
   - Insira o **Master Username** e **Master Password** que você definiu ao criar o banco de dados.  
3. Em **Database**, selecione a instância do banco de dados criada anteriormente.  
4. Clique em **Next**.
5. Nomeie o segredo como **mysecret**.  
   - O código de exemplo utilizado foi escrito para trabalhar com este nome específico de segredo.  
6. Clique em **Next**.
7. Mantenha **Secret rotation** com os valores padrão e clique em **Next**.
8. Revise as configurações e clique em **Store**.

---

## Configure a instância do Amazon EC2 para conectar ao Amazon RDS

### Permitir que o servidor web acesse o segredo

Agora que você criou um segredo no **AWS Secrets Manager**, é necessário dar permissão ao servidor web para utilizá-lo. Para isso, criaremos uma **Policy** que permite que a instância EC2 leia o segredo e adicionaremos essa policy à Role do servidor web.

### Passo a passo

1. **Crie um IAM Instance Profile** se ainda não tiver feito, conforme descrito em *Connect to your Linux instance using Session Manager*.

2. Abra o [console do AWS IAM](https://console.aws.amazon.com/iam/).  
   No painel de navegação, escolha **Policies** e clique em **Create Policy**.

3. Clique em **Choose a service**, pesquise por **Secrets Manager** e selecione.

4. Em **Access level**, expanda **Read** e marque a caixa **GetSecretValue**.

5. Expanda **Resources**. Para este laboratório, selecione **All resources**.  
   > Em cenários reais, você deve restringir o acesso apenas a segredos específicos.

6. Clique em **Next: Tags** e depois em **Next: Review**.

7. Nomeie a policy como **ReadSecrets** e clique em **Create policy**.

8. No painel de navegação, escolha **Roles** e pesquise pela role **SSMInstanceProfile** (criada anteriormente). Clique nela.

9. Em **Permissions policies**, clique em **Attach policies**.  
   Pesquise pela policy **ReadSecrets**, marque a caixa e clique em **Attach policy**.

10. Verifique que **AmazonSSMManagedInstanceCore** e **ReadSecrets** estão listados em **Permissions policies**.

---

## Acesse a aplicação

1. Acesse o [console do Amazon EC2](https://console.aws.amazon.com/ec2/) e encontre o servidor criado no laboratório de EC2 Linux.  
   Copie o **endereço IP público** do servidor web.

2. Abra uma nova guia no navegador e cole o endereço IP. Clique em **RDS**.

3. Você verá uma página exibindo as informações do banco de dados criado.

4. Explore o catálogo de endereços e utilize os links **Add Contact**, **Edit** e **Remove** para interagir com os dados do RDS.

---

## Conclusão

Parabéns! Você implementou e utilizou com sucesso um banco de dados MySQL gerenciado pela AWS. Este é um exemplo básico de um catálogo de endereços simples interagindo com o RDS. O Amazon RDS suporta cenários muito mais complexos de bancos de dados relacionais.

---

# Crie um snapshot do Amazon RDS (Opcional)

Agora é um bom momento para tirar um **snapshot** do seu banco de dados Amazon RDS. Um snapshot permite que você faça backup da instância em um estado conhecido e, posteriormente, restaure para esse estado a qualquer momento.

### Passo a passo

1. Acesse a [página do Amazon RDS](https://console.aws.amazon.com/rds/).

2. Selecione sua instância do RDS, clique em **Actions** e depois em **Take snapshot**.

3. Dê um nome para o snapshot, por exemplo, `Immersion-day-snapshot`, e clique em **Take Snapshot**.

> Observação: Para uma instância única do Amazon RDS, haverá um pequeno downtime enquanto o backup é realizado. Para instâncias de exemplo pequenas, o tempo de backup será curto.

4. Para visualizar snapshots, acesse o link **Snapshots** no painel esquerdo.  

5. Você pode restaurar rapidamente uma instância do RDS a partir de qualquer snapshot selecionando **Restore Snapshot** no menu **Actions**.

---

## Modifique o tamanho da instância do Amazon RDS (Opcional)

Você pode modificar o tamanho da sua instância do Amazon RDS facilmente pelo console da AWS.  
Siga os passos abaixo:  

1. Selecione a sua instância de banco de dados, clique em **Actions** e depois em **Modify**.

2. Edite o tipo da instância para `t2.small` e, se desejar, aumente o tamanho do banco de dados ao mesmo tempo. Clique em **Next**.

3. Na próxima tela, **marque a opção Apply Immediately** para que as mudanças sejam aplicadas imediatamente. Em seguida, clique em **Modify DB Instance**.

> Observação:  
> - Você pode modificar o tamanho das instâncias a qualquer momento, mas não é possível diminuir o tamanho de um banco de dados depois de aumentado.  
> - Durante a modificação, haverá uma breve interrupção do serviço. Normalmente, a alteração do tamanho da instância leva entre 4 e 12 minutos.

4. Lembre-se de excluir seus recursos quando não forem mais necessários para evitar cobranças adicionais, caso esteja usando uma conta AWS própria.

---

## Desprovisionando os recursos

Para deletar a instância do Amazon RDS utilizada no laboratório, siga os passos abaixo:

1. Selecione a instância do Amazon RDS que você criou.
2. No menu **Actions**, clique em **Delete**.
3. Na janela de confirmação:
   - **Desmarque** a opção **Create final snapshot**.
   - **Marque** a caixa de verificação:  
     *I acknowledge that upon instance deletion, automated backups, including system snapshots and point-in-time recovery, will no longer be available*.
   - Digite **delete me** para confirmar.
4. Clique em **Delete** para remover completamente a instância do banco de dados.

---

## Laboratório prático do Amazon RDS SQL Server

O Amazon RDS é um serviço web que facilita a configuração, a operação e o dimensionamento de um banco de dados relacional na nuvem. Ele fornece capacidade econômica e redimensionável enquanto gerencia tarefas demoradas de administração de banco de dados, liberando você para se concentrar em seus aplicativos e negócios.

Este laboratório exige que o **EC2 Windows Hands-On Lab** seja concluído com antecedência. Ele usará o servidor web criado anteriormente no laboratório do EC2 para acessar a instância de banco de dados usando o cliente **Microsoft SQL Server Management Studio (SSMS)** por meio do **Microsoft Remote Desktop Protocol (RDP)**.

<img width="751" height="652" alt="image" src="https://github.com/user-attachments/assets/5da0c2c2-e159-4178-825f-48686fab934e" />

## Objetivos do laboratório

- Iniciar uma instância do RDS
- Acessar o RDS a partir do EC2
- Criar um snapshot do RDS (opcional)
- Modificar o tamanho da instância do RDS (opcional)
- Limpeza de recursos

---

## Inicie uma instância do RDS

### Pré-requisito: EC2 Windows Hands-On Lab

No **EC2 Windows Hands-On Lab**, lançamos uma instância EC2 de servidor web com o grupo de segurança **Immersion Day - Web Server**, que permite tráfego TCP nas portas **80** e **3389** para o servidor web.

## Inicie uma instância do RDS

Vamos configurar e iniciar uma instância RDS do **Microsoft SQL Server**.

1. Faça login no **AWS Management Console** e abra o console do **Amazon RDS**.

2. Clique em **Criar banco de dados**.

3. Em **Escolha um método de criação de banco de dados**, selecione a opção **Criação fácil (Easy Create)**.  
   - Com **Standard Create**, você define as configurações do seu banco de dados manualmente.  
   - A opção **Easy Create** fornece configurações recomendadas de melhores práticas, incluindo acesso a tamanhos de instâncias do nível gratuito.

4. Selecione **Microsoft SQL Server** em **Opções do mecanismo**.  
   Para **Tamanho da instância do banco de dados**, selecione **Nível gratuito**.  
   O Amazon RDS fornecerá a **Amazon Machine Image (AMI)** e o software do banco de dados automaticamente.

5. Preencha as informações da instância do banco de dados:

| Parâmetro                  | Valor         |
|-----------------------------|---------------|
| Identificador de instância  | awsdb         |
| Nome de usuário principal   | awsuser       |
| Senha mestra                | awspassword   |

6. Para configurar a conexão com a instância EC2 criada anteriormente, abra **Configurar conexão do EC2 - opcional**.  
   - Selecione **Conectar-se a um recurso computacional do EC2**.  
   - Escolha a instância EC2 criada no **EC2 Windows Hands-On Lab**.

7. Expanda a seção **Exibir configurações padrão para criação fácil**.  
   - O **Easy Create** define várias configurações padrão, algumas das quais podem ser alteradas após a criação.  
   - Consulte a coluna **Editável após a criação do banco de dados** para saber quais parâmetros podem ser modificados depois.

8. Revise todas as configurações e clique em **Criar banco de dados**.

9. No painel do RDS, monitore sua nova instância até que o status mude de **Criando** → **Fazendo backup** → **Disponível**.  
   - Este processo pode levar até 5 minutos enquanto o banco de dados é provisionado e inicializado.

---

## Acesse o RDS a partir do EC2

### Configuração de conexão do Amazon RDS para SQL Server

Neste procedimento, vamos conectar à instância de banco de dados usando o **Microsoft SQL Server Management Studio (SSMS)**.

---

### 1. Localize o endpoint da instância RDS

1. Abra o console do **Amazon RDS**.  
2. No painel de navegação, escolha **Bancos de dados**.  
3. Selecione a instância de banco de dados SQL Server criada.  
4. Na guia **Conectividade**, copie o **endpoint** e anote o **número da porta** (padrão: 1433).  

> Você precisará do endpoint e da porta para conectar à instância de banco de dados.

---

### 2. Conecte-se à instância EC2

1. Conecte-se à instância EC2 criada no **EC2 Windows Hands-On Lab** usando **RDP**.  
2. Após carregar a área de trabalho remota, abra o **PowerShell**.

---

### 3. Instale o SQL Server Management Studio (SSMS)

1. Use o menu Iniciar para abrir o **PowerShell**.  
2. Copie e cole o script abaixo para baixar e instalar o SSMS, e pressione Enter.  
   ```powershell
   # Script de instalação do SSMS
   Invoke-WebRequest -Uri "https://aka.ms/ssmsfullsetup" -OutFile "$env:TEMP\SSMS-Setup-ENU.exe"
   Start-Process "$env:TEMP\SSMS-Setup-ENU.exe" -Wait
   ```
3. A instalação pode levar alguns minutos.  
4. O Microsoft SQL Server Management Studio será aberto automaticamente. Clique em Instalar.  
5. Após a instalação, feche o SSMS e o PowerShell.

---

### 4. Conecte-se à instância de banco de dados SQL Server

1. Abra o **SQL Server Management Studio (SSMS)** pelo menu Iniciar.  
2. Na caixa de diálogo **Conectar ao servidor**, forneça as seguintes informações:

| Campo               | Valor                                                                 |
|--------------------|-----------------------------------------------------------------------|
| Tipo de servidor     | Mecanismo de banco de dados                                           |
| Nome do servidor     | `<endpoint>,<porta>` (ex: `awsdb.0123456789012.us-west-2.rds.amazonaws.com,1433`) |
| Autenticação         | Autenticação do SQL Server                                           |
| Login                | awsuser                                                               |
| Senha                | awspassword                                                           |

3. Clique em **Conectar** para estabelecer a conexão.

> Para segurança, recomenda-se usar conexões criptografadas. Conexões não criptografadas devem ser usadas apenas em VPCs confiáveis.

---

### 5. Explore sua instância de banco de dados

1. No SSMS, vá em **Exibir → Explorador de objetos**.  
2. Expanda a instância de banco de dados, **Bancos de dados**, e depois **Bancos de dados do sistema** (master, model, msdb, tempdb).  
3. Observe também o banco de dados **rdsadmin**, usado pelo Amazon RDS para gerenciamento interno e procedimentos avançados.

---

### 6. Executar consultas de teste

1. No menu **Arquivo → Novo → Consulta com conexão atual**, abra uma nova query.  
2. Insira a consulta SQL de teste: ` SELECT @@VERSION; `
3. Execute a consulta. O SSMS retornará a versão do SQL Server da sua instância Amazon RDS.

---

## Criar um snapshot do RDS (opcional)

Agora é um bom momento para tirar um **instantâneo** do seu banco de dados do RDS. Um instantâneo permite que você faça backup da sua instância em um estado conhecido com a frequência que desejar e restaure esse estado a qualquer momento.

1. No **console de gerenciamento da AWS**, acesse a seção **RDS**.  
2. Selecione sua instância do RDS, clique em **Instance Actions** e selecione **Take a snapshot**.  
3. Dê um nome ao snapshot, por exemplo: **Immersion-day-snapshot**, e clique em **Take snapshot**.

> Observação: usando uma instância única do RDS, haverá um pequeno tempo de inatividade para que o backup seja concluído. Como nosso banco de dados de exemplo é pequeno, o tempo total será mínimo.

4. Os snapshots podem ser encontrados no link **Snapshots** no painel esquerdo do console.  
5. Para iniciar uma nova instância a partir de um snapshot, selecione o snapshot desejado e clique em **Restore Snapshot** no menu **Actions**.

---

## Modificar o tamanho da instância do RDS (opcional)

Você pode alterar o tamanho da sua instância do RDS diretamente pelo console da AWS. Siga os passos abaixo:

1. Selecione sua **instância de banco de dados RDS**, clique em **Actions** → **Modify**.  
2. Altere a instância para uma classe Burstable, por exemplo **t3.xlarge**. Se desejar, você também pode aumentar o tamanho do armazenamento ao mesmo tempo. Clique em **Next**.  
3. Na tela seguinte, marque a opção **Apply Immediately** para que as alterações sejam aplicadas imediatamente. Caso contrário, elas serão aplicadas na próxima janela de manutenção. Clique em **Modify DB Instance**.  

> Observações:
> - É possível aumentar ou diminuir o tamanho das instâncias a qualquer momento, mas o armazenamento não pode ser reduzido após o aumento.  
> - Assim como nos backups, haverá um breve tempo de interrupção durante essas operações. Em geral, o redimensionamento leva de 4 a 12 minutos.  
> - Lembre-se de **desprovisionar recursos** para evitar cobranças adicionais.

---

## Limpeza dos recursos

Para excluir a instância do RDS criada durante o laboratório, siga os passos abaixo:

1. No console do **Amazon RDS**, selecione a instância que você deseja deletar.  
2. Clique em **Actions** → **Delete**.  
3. Na janela de confirmação:  
   - **Desmarque** a opção **Create final snapshot**.  
   - Marque a opção **I acknowledge that upon instance deletion, automated backups, including system snapshots and point-in-time recovery, will no longer be available**.  
   - Digite `delete-me` para confirmar.  
4. Clique em **Delete** para excluir completamente a instância de banco de dados.

---

# Armazenamento - Amazon S3

<img width="939" height="471" alt="image" src="https://github.com/user-attachments/assets/e177867b-1703-4321-8b9e-27e3a1fb24cd" />

A AWS oferece um conjunto completo de serviços para que você possa **armazenar, acessar e analisar seus dados**, ajudando a reduzir custos, aumentar a agilidade e acelerar a inovação.  

Você pode escolher entre diferentes tipos de serviços de armazenamento:  

- **Armazenamento de objetos**  
- **Armazenamento de arquivos**  
- **Armazenamento em bloco**  
- **Backup**  
- **Migração de dados**  

Esses serviços permitem construir a base do seu **ambiente de TI na nuvem** de forma flexível e escalável.

---

## Laboratório prático do Amazon Elastic File System (EFS)

O **Amazon Elastic File System (EFS)** foi projetado para fornecer **armazenamento de arquivos sem servidor e totalmente elástico**, permitindo **compartilhar dados de arquivos** sem precisar provisionar ou gerenciar a capacidade e o desempenho do armazenamento.  

O EFS cresce e diminui automaticamente à medida que você adiciona e remove arquivos, sem a necessidade de gerenciamento manual ou provisionamento.

---

## Objetivos do laboratório

Neste laboratório, projetaremos uma **unidade do Elastic File System** e a conectaremos a **dois nós do Elastic Compute Cloud (EC2)**.  

Ao final do laboratório, você será capaz de:

- Criar uma **VPC (Virtual Private Cloud)** com duas sub-redes públicas  
- Criar **grupos de segurança** para EC2 e EFS  
- Criar um **sistema de arquivos elástico (EFS)**  
- Criar a **primeira instância EC2** e montar o drive EFS  
- Criar a **segunda instância EC2** e montar o drive EFS  
- Conectar-se às **duas instâncias EC2** usando o **Instance Connect**  
- Criar um arquivo na unidade **EFS**  
- Demonstrar a **montagem do EFS** a partir da segunda instância  
- **Limpar** os recursos criados ao final

---

## Criação de uma VPC e duas sub-redes públicas

Primeiro, precisamos criar uma **VPC (Virtual Private Cloud)** para que nossas instâncias do **EC2** e do **EFS** residam.

Abaixo está um **desenho arquitetônico** do que construiremos neste laboratório prático.

<img width="1636" height="1300" alt="image" src="https://github.com/user-attachments/assets/65c72e18-a6a7-4ab4-972a-651aa46a4231" />

---

## Passo a Passo

1. **Acesse o console do VPC**  
   Use a barra de pesquisa no console da AWS para localizar **VPC** ou encontre no menu **Serviços**.  

2. **Clique em Criar VPC**  
   No console do VPC, clique no botão laranja **Criar VPC** na parte superior da página.  

3. **Escolha "VPC e mais"**  
   Estamos criando não apenas a VPC, mas também as **duas sub-redes públicas** onde nossas instâncias EC2 residirão.  

4. **Nome da VPC**  
   Atribua um nome memorável à sua VPC. Exemplo: `LJW-EFS`. Você pode escolher qualquer nome que ajude a identificar a VPC.  

5. **Bloco CIDR IPv4**  
   Configure como `10.0.0.0/16`. Isso fornece cerca de 65 mil endereços IP para uso na VPC.  

6. **Bloco CIDR IPv6**  
   Selecione **Sem bloco CIDR IPv6**. Este exercício não utilizará IPv6.  

7. **Tenancy**  
   Deixe como padrão.  

8. **Número de zonas de disponibilidade**  
   Selecione **2**.  

9. **Número de sub-redes públicas**  
   Selecione **2 sub-redes públicas**.  

10. **Número de sub-redes privadas**  
    Selecione **0**, pois não utilizaremos sub-redes privadas neste exercício.  

11. **Gateway NAT**  
    Selecione **Nenhum**, já que não há sub-redes privadas.  

12. **VPC EndPoints**  
    Selecione **Nenhum**, pois não usaremos o S3 neste laboratório.  

13. **Opções de DNS**  
    Marque ambas as opções na parte inferior da tela.  

14. **Criar VPC**  
    Clique em **Criar VPC** para finalizar a configuração.  

Agora sua **VPC com duas sub-redes públicas** está pronta e você pode prosseguir para a próxima etapa: **Criação de grupos de segurança para EC2 e EFS**.

---

## Criação de grupos de segurança para EC2 e EFS

### O que é um grupo de segurança?

Um **grupo de segurança** atua como um firewall virtual para suas instâncias do EC2, controlando o tráfego de entrada e saída.  

- **Regras de entrada**: Controlam o tráfego que chega à instância.  
- **Regras de saída**: Controlam o tráfego que sai da instância.  

[Saiba mais sobre grupos de segurança](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html)

---

## Criar grupos de segurança para EC2 e EFS

1. **Acesse o console do EC2**  
   Pesquise na barra de pesquisa ou clique em **Serviços** → **EC2**.  

2. **Abra a seção Grupos de Segurança**  
   No menu lateral, role até **Rede e Segurança** e clique em **Grupos de Segurança**.  

3. **Criar Grupo de Segurança para EC2 (SSH)**  
   - Clique em **Criar grupo de segurança**.  
   - **Nome**: `EC2-SSH-SecurityGroup`  
   - **Descrição**: Grupo de segurança para permitir conexão SSH às instâncias EC2.  
   - **VPC**: Selecione a VPC criada anteriormente.  
   - **Regra de entrada**:  
     - Tipo: `SSH`  
     - Protocolo e porta preenchidos automaticamente  
     - Fonte: `IPv4 Anywhere`  
     - Descrição: opcional, descrevendo a regra  
   - **Regras de saída**: mantenha padrão (todo o tráfego permitido)  
   - Clique em **Criar grupo de segurança**.  

4. **Criar Grupo de Segurança para EFS (NFS)**  
   - Clique em **Criar grupo de segurança** novamente.  
   - **Nome**: `EC2-EFS-SecurityGroup`  
   - **Descrição**: Conexão do EC2 ao EFS  
   - **VPC**: selecione a VPC criada anteriormente  
   - **Regra de entrada**:  
     - Tipo: `NFS`  
     - Fonte: Personalizado → selecione o **Grupo de Segurança do EC2 (EC2-SSH-SecurityGroup)**  
   - Deixe as tags como padrão  
   - Clique em **Criar grupo de segurança**  

5. **Editar grupo de segurança do EFS para permitir autoacesso**  
   - Selecione o grupo de segurança do EFS recém-criado  
   - Clique em **Editar regras de entrada**  
   - Adicione uma regra:  
     - Tipo: `NFS`  
     - Fonte: selecione o próprio **Grupo de Segurança do EFS**  
     - Descrição: opcional  
   - Clique em **Salvar regras**  

Agora você está pronto para passar para a próxima etapa: **Criação do Elastic File System (EFS)**.

---

# Criando um sistema de arquivos elástico (EFS)

## Visão geral

Agora que configuramos os grupos de segurança, vamos criar um **Elastic File System (EFS)** que será montado em nossas instâncias do EC2 posteriormente.

---

## Passos para criar o EFS

1. **Acesse o console EFS**  
   Pesquise por **EFS** na barra de pesquisa ou clique em **Serviços** → **EFS**.

2. **Criar sistema de arquivos**  
   - Clique em **Criar sistema de arquivos**.  
   - Clique em **Personalizar** para acessar a configuração completa.

3. **Configurações do sistema de arquivos**  
   - **Nome**: escolha um nome memorável.  
   - **Classe de armazenamento**: `Regional` (dados redundantes em múltiplas zonas de disponibilidade).  
   - **Backups automáticos**: Ativado.  
   - **Gerenciamento do ciclo de vida**:
     - Transition into Infrequent Access (IA): 30 dias desde o último acesso  
     - Transition into archive: None  
     - Transition into standard: On first access  
   - **Criptografia**: Desmarque `Ativar criptografia de dados em repouso` (fora do escopo do laboratório).  
   - **Desempenho**: Throughput `Bursting`.  
   - **Tags**: Opcional.  
   - Clique em **Avançar**.

4. **Acesso à rede**  
   - **VPC**: selecione a VPC criada anteriormente.  
   - **Destinos de montagem**: use as sub-redes públicas criadas anteriormente.  
   - **Grupos de segurança**:
     - Remova os grupos padrão (clique no X).  
     - Adicione o **Grupo de Segurança EFS** em cada zona de disponibilidade.  
   - Clique em **Avançar**.

5. **Política do sistema de arquivos**  
   - Opcional, não faremos alterações.  
   - Clique em **Avançar**.

6. **Revisar e criar**  
   - Verifique todas as configurações.  
   - Clique em **Criar**.

7. **Verificação**  
   - Após a criação, o EFS aparecerá no console pronto para uso.

Agora você está pronto para a próxima etapa: **Criar duas instâncias do EC2 e montar o EFS**.

---

## Crie a primeira instância EC2 e monte nosso drive EFS

Agora que temos nossa VPC com duas sub-redes públicas, vamos criar a primeira instância EC2.

1. **Acesse o console do EC2**  
   Pesquise **EC2** na barra de pesquisa do console.

2. **Iniciar uma instância**  
   - Clique em **Launch Instance**.  
   - Selecione **Launch Instance** no menu suspenso.

3. **Configurações da instância**  
   - **Nome e tags**: `EFS-SERVER-1`  
   - **AMI (Amazon Machine Image)**: `Amazon Linux 2 AMI (HVM)`  
   - **Tipo de instância**: `t2.micro`  
   - **Par de chaves**: `Prosseguir sem um par de chaves` (usaremos Instance Connect)

4. **Configurações de rede**  
   - Clique em **Editar** ao lado de Configurações de rede  
   - **VPC**: selecione a VPC criada anteriormente  
   - **Sub-rede**: `Sub-rede pública 1`  
   - **Atribuir IP público automaticamente**: `ATIVAR`  
   - **Grupos de segurança**: selecione **EC2-SSH-SecurityGroup** e **EC2-EFS-SecurityGroup**

---

## Montagem do EFS no servidor

1. **Configurar armazenamento**  
   - Ao lado de **0 x Sistema de Arquivos**, clique em **EDITAR**  
   - **Sistemas de arquivos**: selecione o EFS criado anteriormente  
   - Clique em **Adicionar sistema de arquivos compartilhados**  
   - **Desmarque** `Criar e anexar grupos de segurança automaticamente`  
   - **Marque** `Monte automaticamente o sistema de arquivos compartilhado anexando o script de dados do usuário`

2. **Detalhes avançados**  
   - Deixe as opções padrão

3. **Finalizar**  
   - Clique em **Launch Instance**

Agora a primeira instância EC2 está criada e pronta para montar o drive EFS.  

Próxima etapa: **Criar a segunda instância EC2 e montar o drive EFS**.

---

## Crie a segunda instância EC2 e monte nosso drive EFS

Na lição anterior, criamos a primeira instância EC2 e montamos o drive EFS. Agora vamos criar a segunda instância.

A única diferença é que esta instância será colocada na **Sub-rede pública 2**.

---

## Criando a segunda instância EC2

1. **Acesse o console do EC2**  
   Pesquise **EC2** na barra de pesquisa do console.

2. **Iniciar instância**  
   - Clique em **Launch Instance**  
   - Selecione **Launch Instance** no menu suspenso.

3. **Configurações da instância**  
   - **Nome e tags**: `EFS-SERVER-2`  
   - **AMI**: `Amazon Linux 2 AMI (HVM)`  
   - **Tipo de instância**: `t2.micro`  
   - **Par de chaves**: `Prosseguir sem um par de chaves` (usaremos Instance Connect)

4. **Configurações de rede**  
   - Clique em **Editar** ao lado de Configurações de rede  
   - **VPC**: selecione a VPC criada anteriormente  
   - **Sub-rede**: `Sub-rede pública 2`  
   - **Atribuir IP público automaticamente**: `ATIVAR`  
   - **Grupos de segurança**: selecione **EC2-SSH-SecurityGroup** e **EC2-EFS-SecurityGroup**

---

## Montagem do EFS na instância

1. **Configurar armazenamento**  
   - Ao lado de **0 x Sistema de Arquivos**, clique em **EDITAR**  
   - **Sistemas de arquivos**: selecione o EFS criado anteriormente  
   - Clique em **Adicionar sistema de arquivos compartilhados**  
   - **Desmarque** `Criar e anexar grupos de segurança automaticamente`  
   - **Marque** `Monte automaticamente o sistema de arquivos compartilhado anexando o script de dados do usuário`

2. **Detalhes avançados**  
   - Deixe as opções padrão

3. **Finalizar**  
   - Clique em **Launch Instance**

---

## Verificando instâncias EC2

- Use o painel de navegação vertical à esquerda para acessar o **Painel do EC2**  
- Clique em **Instâncias (em execução)**  
- Confirme que **EFS-SERVER-1** e **EFS-SERVER-2** estão em execução

Agora você está pronto para a próxima etapa: **conectar-se ao EC2 usando a conexão de instância**.

---

## Conecte-se às nossas instâncias do EC2 usando o Instance Connect

Agora que nossas duas instâncias estão em execução e o drive EFS está montado, vamos verificar se o Elastic File System funciona corretamente.

---

## Objetivos

1. Usar a conexão de instância (Instance Connect) para acessar ambas as instâncias via SSH.
2. Verificar se o EFS está montado em ambas as instâncias.
3. Criar um arquivo em uma instância no EFS.
4. Confirmar a existência do arquivo na primeira instância.
5. Listar o arquivo na segunda instância para verificar compartilhamento.

---

## Conectando-se via Instance Connect

1. **Acesse o console do EC2**  
   Pesquise **EC2** na barra de pesquisa do console ou acesse via menu **Serviços**.

2. **Acesse as instâncias em execução**  
   Clique em **Instâncias (em execução)** no painel central.

3. **Conectar à primeira instância (EFS-SERVER-1)**  
   - Selecione a caixa ao lado da instância `EFS-SERVER-1`.  
   - Clique em **Conectar** no canto superior direito.  
   - Na página de conexão, escolha **EC2 Instance Connect** (padrão) e clique em **Conectar**.  
   - Uma nova guia do navegador abrirá o terminal SSH para a instância.

4. **Conectar à segunda instância (EFS-SERVER-2)**  
   - Mantenha a guia da primeira instância aberta.  
   - Volte à guia do console EC2 e clique em **Instances**.  
   - Selecione a segunda instância `EFS-SERVER-2`.  
   - Clique em **Conectar**, escolha **EC2 Instance Connect** e clique em **Conectar**.  
   - Uma terceira guia abrirá o terminal SSH para a segunda instância.

---

Agora você deve ter três guias do navegador:

1. Guia do console do EC2
2. SSH para **EFS-SERVER-1**
3. SSH para **EFS-SERVER-2**

Próximo passo: **Criar um arquivo na unidade EFS** e verificar se ele é acessível em ambas as instâncias.

---

# Crie um arquivo na unidade EFS

Agora que estamos conectados às duas instâncias, vamos testar o **Elastic File System (EFS)** em ação. Nossas duas instâncias estão em **zonas de disponibilidade diferentes**, e o EFS permite que múltiplas instâncias compartilhem arquivos simultaneamente.

O ponto de montagem do EFS é: `/mnt/efs/fs1`  

---

## Adicionando um arquivo ao EFS

1. **Abra uma das guias SSH** da primeira instância (`EFS-SERVER-1`).

2. **Mude para o diretório do EFS**: ` cd /mnt/efs/fs1 `  
Você estará agora no ponto de montagem do EFS.

3. Crie um novo arquivo chamado `newfile.txt`: `sudo touch newfile.txt`  
4. Verifique se o arquivo foi criado: ` ls `
5. Você deverá ver apenas o arquivo newfile.txt, que acabamos de criar.

Agora que o arquivo foi criado na primeira instância, estamos prontos para demonstrar a montagem do EFS na segunda instância e confirmar que o arquivo é visível lá também.  

---

## Demonstre a montagem do EFS a partir da segunda instância

Agora que criamos um arquivo na primeira instância do **Elastic File System (EFS)**, vamos confirmar se ele é visível na segunda instância.

### Passo 1: Acesse a segunda instância

Selecione a guia da segunda instância (`EFS-SERVER-2`) no navegador.

Mude para o diretório do ponto de montagem do EFS: ` cd /mnt/efs/fs1 `  
Execute o comando para listar os arquivos: ` ls `  
Você deverá ver o arquivo criado na primeira instância (newfile.txt).  

### Passo 2: Criar um segundo arquivo na segunda instância

Ainda na segunda instância (`EFS-SERVER-2`), crie um novo arquivo chamado `SecondNewFile.txt`:

```bash
sudo touch SecondNewFile.txt
```

Verifique os arquivos no diretório: `ls`  
Agora você verá os dois arquivos listados:
- newfile.txt
- SecondNewFile.txt

### Passo 3: Verificar os arquivos na primeira instância

Vá para a guia da primeira instância (`EFS-SERVER-1`).
Confirme se você está no ponto de montagem do EFS:

```bash
cd /mnt/efs/fs1
```

Liste os arquivos: `ls`  
Você deverá ver ambos os arquivos:
- newfile.txt
- SecondNewFile.txt

## Recapitulação

- Criamos uma VPC com duas zonas de disponibilidade.  
- Criamos instâncias EC2 em cada zona.  
- Montamos um **Elastic File System (EFS)** para compartilhar arquivos entre as instâncias.  
- Criamos arquivos a partir de uma instância e confirmamos que estavam visíveis na outra.  

Isso demonstra a capacidade do EFS de fornecer **armazenamento compartilhado** entre múltiplas instâncias do EC2.

---

## Limpeza de recursos

Agora que concluímos nosso laboratório prático do **Elastic File System (EFS)**, precisamos encerrar os serviços que usamos hoje.

## Resumo do que criamos

- VPC com duas zonas de disponibilidade  
- Duas instâncias EC2  
- Sistema de arquivos EFS montado nas instâncias  

---

## Encerrando os serviços

### 1. Excluir o Elastic File System (EFS)

1. Acesse o console do **EFS** pesquisando na barra de pesquisa do console.  
2. Selecione a unidade EFS clicando no balão à esquerda do sistema de arquivos.  
3. Clique em **Excluir** no canto superior direito.  
4. Confirme a exclusão seguindo as instruções.  
5. Aguarde a barra de status verde indicando que a exclusão foi concluída.

---

### 2. Excluir as instâncias EC2

1. Acesse o console do **EC2**.  
2. Clique em **Instâncias (em execução)** no meio da página.  
3. Selecione ambas as instâncias clicando na caixa à esquerda de cada uma.  
4. Clique em **Estado da instância → Encerrar instância** no canto superior direito.  
5. Aguarde a barra de status verde indicando que a exclusão foi concluída.

---

### 3. Excluir sub-redes e VPC

1. Acesse o console do **VPC**.  
2. Na barra de navegação esquerda, clique em **Sub-redes**.  
3. Classifique as sub-redes pelo nome e selecione as que você criou.  
4. Clique em **Ações → Excluir sub-rede** e confirme a exclusão.  
5. Em seguida, vá para **Suas VPCs** na barra de navegação esquerda.  
6. Selecione a VPC que você criou (não a padrão) e clique em **Ações → Excluir VPC**.  
7. Confirme a exclusão.  
   > Observação: a exclusão da VPC também remove os grupos de segurança associados.

Parabéns! 🎉  
Você excluiu com sucesso todos os recursos criados durante este laboratório prático e concluiu a limpeza do ambiente.

---
