## 🧪 Lab 172 - Criar instância do Amazon EC2 / Servidor Web com Amazon Linux

## Objetivos

Após concluir este desafio, você será capaz de:

- Configurar uma rede virtual (VPC).  
- Criar uma instância do Amazon Linux EC2 nessa rede.  
- Instalar e iniciar um servidor web (Apache).  
- Implantar e executar uma aplicação web simples.

---

## Desafio 1 - Criar uma instância do Amazon Linux EC2 para executar um aplicativo web

1. **Criar a instância via Console da AWS**
   - Dar um nome para a instância.  
   - Usar uma **Amazon Machine Image (AMI) Amazon Linux 2**.  
   - Selecionar um tipo de instância **T3** (menor que médio, ex.: t3.micro ou t3.nano).  

2. **Par de chaves**
   Criar um novo para acesso via SSH.
   
4. **Configurar rede**
   - Criar uma **nova VPC**.  
   - Criar uma **nova sub-rede**.  
   - Habilitar o **Auto-assign Public IPv4**.  

5. **Volume**
   - No Armazenamento, colocar tipo gp2 (General Purpose SSD).  
   - Tamanho padrão (8GB ou 10GB).

6. **Security Group**
   - Crie um novo Security Group.
   - Regras de entrada: SSH(22) seu IP / HTTP (80) Anywhere (0.0.0.0/0).  

7. **Configurar o User Data**
   > O **User Data** é aquele script automático de configuração no primeiro boot da máquina.
   
   - Instalar e iniciar o serviço **httpd (Apache)**.  
   - Dar permissão de gravação para usuários no diretório `/var/www/html`.  
  
   Exemplo de script **User Data**:  
   ```bash
   #!/bin/bash
   # Atualiza os pacotes
   yum update -y

   # Instala o Apache (httpd)
   yum install -y httpd

   # Inicia o serviço Apache
   systemctl start httpd
   systemctl enable httpd

   # Dá permissão de escrita para /var/www/html
   chmod -R 777 /var/www/html

   # (opcional) Cria uma página inicial simples
   echo "<h1>Servidor Apache no EC2 rodando!</h1>" > /var/www/html/index.html
   ```

8. Conclua a criação da instância normalmente.  
   Quando ela iniciar, o Apache já vai estar instalado, rodando e acessível pelo navegador (desde que você tenha aberto a porta 80 (HTTP) 
   no Security Group da instância).

9. Testar no navegador
   Copie o IPv4 público da instância e abra uma outra guia no navegador e cole **htt://<seu-ip-publico>**
   Veja a página: Servidor Apache rodando no Amazon Linux! 🎉  
   <img width="822" height="98" alt="image" src="https://github.com/user-attachments/assets/d6a7d454-d6e0-430c-9e91-4dc3cb8a8142" />

---

## Desafio 2 - Validação final
Vamos provar que o servidor Apache está rodando de verdade e que o site (mesmo que simples) está hospedado dentro da instância.  


echo "<h1>Adriana's re/Start Project Work</h1>" > /var/www/html/index.html
   
