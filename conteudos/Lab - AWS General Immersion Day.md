## 🧪 Lab - AWS General Immersion Day.md

Neste lab, vamos aprender várias funcionalidades dos serviços mais básicos da AWS.

Os módulos básicos são constituídos dos tópicos a seguir:

Computação - Amazon EC2  
Redes - Amazon VPC  
Segurança - AWS IAM  
Monitoramento - Amazon CloudWatch  
Banco de dados - Amazon RDS  
Armazenamento - Amazon S3  
Provisionamento - AWS CloudFormation  

## Computação - Amazon EC2 

<img width="1912" height="1082" alt="image" src="https://github.com/user-attachments/assets/f6444e57-9d2d-48a6-82dc-c6b730ceedba" />

**Computação** é a base para a construção e execução de aplicações, esteja você construindo aplicações corporativas, aplicativos móveis ou executando clusters para projetos de análise de dados. A AWS oferece um portfólio vasto de serviços de computação , permitindo que você desenvolva, faça deploy, execute, e escale suas aplicações e projetos na nuvem pública mais segura e propícia a inovação do mundo.

Os serviços de computação da AWS têm as seguintes características:

A instância adequada para os seus projetos;  
Time-to-market mais rápido;  
Segurança embutida em diversas camadas;  
Flexibilidade para otimização de custos;  
Provisionamento de capacidade computacional onde você necessita.  

## Visão Geral do Amazon EC2

O serviço Amazon EC2 oferece capacidade computacional escalável na nuvem da AWS.  
Esse serviço elimina sua necessidade de investir de antemão em hardware, permitindo que você desenvolva e faça deploy de aplicações 
mais rápido.  
Você pode usar instâncias EC2 para executar a quantidade de máquinas virtuais que você precisar, configurar aspectos de segurança e 
redes, e gerenciar armazenamento.  
O serviço permite que você aumente ou diminua a capacidade computacional para lidar com a sua demanda, reduzindo sua necessidade de 
ter que prever seu tráfego de antemão.  

<img width="657" height="473" alt="image" src="https://github.com/user-attachments/assets/dd1a86ce-f0b6-4108-aacb-3495f77de55e" />

Crie seu próprio servidor web executando os laboratórios abaixo em ordem:  

Criando um novo key pair  
Executando um servidor web  
Conecte-se a sua instância Linux  
Conecte-se a sua instância Linux usando o AWS Systems Manager Session Manager (Opcional)  
Conecte-se a sua instância usando PuTTy (Opcional)  
Desprovisionamento dos recursos  

## Criar um novo par de chaves

Neste laboratório, vamos precisar criar uma instância do EC2 usando um SSH keypair.  
As etapas a seguir descrevem a criação de um SSH keypair exclusivo para você usar neste laboratório.  

1. Faça login no AWS Management Console e abra o console do Amazon EC2.  
No canto superior direito do AWS Management Console, confirme se você está na região desejada da AWS.

2. Clique em Key Pairs na seção Network & Security na parte inferior do menu à esquerda.  
Isso exibirá uma página para gerenciar seus SSH keypair.

3. Para criar um novo SSH keypair, clique no botão Create key pair na parte superior da janela do navegador.

4. Digite [Seu nome]-ImmersionDay na caixa de texto Nome do Key Pair: e clique no botão Create key pair.

5. A página baixará o arquivo [Seu nome]-ImmersionDay.pem para a unidade local.
   Siga as instruções do navegador para salvar o arquivo no local de download padrão.  
   Lembre-se do caminho completo para o arquivo do par de chaves que você acabou de baixar.

<img width="1912" height="344" alt="image" src="https://github.com/user-attachments/assets/c7f5eaf5-ba18-423e-aca9-f9afc5b6b3fb" />

## Inicie uma instância de servidor Web  

Lançaremos uma instância do Amazon Linux 2, inicializaremos o Apache/PHP, e instalaremos uma página da web básica que exibirá 
informações sobre nossa instância.  

1. Clique em EC2 Dashboard na parte superior do menu à esquerda. E clique em Launch instance.

2. Em Name, coloque o valor Servidor Web para IMD. E verifique a configuração padrão da Amazon Machine Image abaixo.

3. Selecione t2.micro em Instance Type.

4. Selecione o key pair que você criou no início deste laboratório no menu suspenso.

5. Clique no botão Edit nas configurações de rede para definir o espaço em que o EC2 estará localizado.

6. Verifique a default VPC e a subnet.

7. Auto-assign public IP está definida como Enable.

8. Logo abaixo, crie Security Groups para atuar como um firewall de rede.
   Os Security Groups especificarão os protocolos e endereços que você deseja permitir em sua política de firewall.
   Para o Security Groups que você está criando atualmente, essa é a regra que se aplica ao EC2 que será criado.
   Depois de inserir `Immersion Day - Web Server` no **nome e descrição do grupo de segurança**, selecione **Adicionar regra de grupo de
   segurança** e defina HTTP como Tipo. 
   Também permita o TCP/80 para Web Service especificando-o. Selecione My IP na fonte.  

9. Todos os outros valores aceitam os valores padrão. Expanda clicando na guia Advanced Details na parte inferior da tela.
   Clique em Meta Data version no dropdown e selecione V2 only (token required)
   <img width="1223" height="95" alt="image" src="https://github.com/user-attachments/assets/1ac55504-88ad-459d-9d2c-a2780d4adcc9" />

10. Insira os seguintes valores no campo User data e selecione Launch instance.
    ```
    #!/bin/sh  
    #Install a LAMP stack
        
    dnf install -y httpd wget php-fpm php-mysqli php-json php php-devel
    dnf install -y mariadb105-server
    dnf install -y httpd php-mbstring
        
    #Start the web server
    
    chkconfig httpd on
    systemctl start httpd
        
    #Install the web pages for our lab
       
    if [ ! -f /var/www/html/immersion-day-app-php7.zip ]; then
       cd /var/www/html
       wget -O 'immersion-day-app-php7.zip' 'https://static.us-east-1.prod.workshops.aws/1d96a398-5c4e-4ba0-9d5a-3856f0f0759e/assets/immersion-day-app-php7.zip?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy8xZDk2YTM5OC01YzRlLTRiYTAtOWQ1YS0zODU2ZjBmMDc1OWUvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTc2MTE3NjA4M319fV19&Signature=lqzMJVmGXTVWfwmNcYNPnvGa1UWuKryQEamK9cLXNCEaqQsSNgqAm8gPeof3Oeniuvc-LZSG32TiB7ai6onAiMbhCtCdJ0-T5gpuBjjXsTnc4b9TmQWK-EcXxgNs9uedUer2hGR09569jsiL0xwb~QOEYCofLQADBBdiH0Bv8FWdCtztHacv9KYeWSKgRt9KWwAwpdk0YEhOaIJrno2HWY4gnYZlEwRXBKfFzEdlBB-Ildc6LD6Uwb2dLwW~yfHnpWoDQRo30-sScWe63rUD0ngGvC5O~weRyTgcCQ8uGueXLs5hLIY3sAanYSYH2r0Ra5OKOWsiJxowHUF7oXQ57A__'
       unzip immersion-day-app-php7.zip
    fi
    ​
    #Install the AWS SDK for PHP
        
    if [ ! -f /var/www/html/aws.zip ]; then
       cd /var/www/html
       mkdir vendor
       cd vendor
       wget https://docs.aws.amazon.com/aws-sdk-php/v3/download/aws.zip
       unzip aws.zip
    fi
    ​
    # Update existing packages
    dnf update -y
    ```

11. Clique no botão **Launch Instances** na parte inferior direita da tela.  
    Depois que sua instância for iniciada, você verá seu web server, a zona de disponibilidade em que a instância está e o nome DNS
    publicamente roteável.
    Clique na caixa de seleção ao lado do seu web server para ver detalhes sobre essa instância do EC2.
