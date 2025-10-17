## 🧪 Lab - AWS General Immersion Day.md

Neste lab vamos aprender várias funcionalidades dos **serviços mais básicos da AWS**.

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

- No menu à esquerda, em **Load Balancing → Load Balancers**, verifique o provisionamento do seu **Application Load Balancer**.

## 🚦 Próximo Passo

Agora que seu **Auto Scaling Group** está ativo, prossiga para a próxima etapa:  
👉 **Configurar Grupos de Segurança (Security Groups)** para permitir o tráfego entre o **ALB** e os **web hosts**.

