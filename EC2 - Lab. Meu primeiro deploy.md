## 🧪 Lab EC2: Meu Primeiro Deploy!

Neste laboratório, realizei meu primeiro deploy de um projeto utilizando o **Amazon EC2**.  
Abaixo está o passo a passo com imagens ilustrativas.

---

### 1. Criar uma instância no EC2
<img src="https://github.com/user-attachments/assets/d0c0ef85-ea32-4502-bc74-ee21e96b9ae9" width="928" />

---

### 2. Selecionar a AMI Amazon Linux
<img src="https://github.com/user-attachments/assets/64efd141-a8a0-434f-a444-bb316558be06" width="920" />

---

### 3. Selecionar o tipo da instância `t2.micro`
<img src="https://github.com/user-attachments/assets/c3efaf00-e5ae-4fe7-bf34-8ea495a2d6f2" width="917" />

---

### 4. Selecionar o Key Pair - **vockey**
<img src="https://github.com/user-attachments/assets/fb1e810b-c9df-46db-b0b5-54423d8282bb" width="919" />

---

### 5. Configurar Network Setting  
Criar um grupo de segurança e liberar as portas:
- **SSH (22)** → para acesso remoto  
- **HTTP (80)** → para visualizar o site

<img src="https://github.com/user-attachments/assets/8920d69c-b892-4a09-8254-b2764cec7164" width="918" />

---

### 6. Manter as demais configurações e clicar em **Launch Instance**
<img src="https://github.com/user-attachments/assets/85478d7f-40dd-42d5-b3c5-9741db82d561" width="163" />

---

### 7. Acessar a instância via SSH  
Ir em **Connect**:
<img src="https://github.com/user-attachments/assets/fdd93403-c512-4d32-b2ee-790830d0ccdd" width="1177" />

---

### 8. Copiar o comando SSH
<img src="https://github.com/user-attachments/assets/5e3b7610-7308-4988-b5a5-580085045be3" width="1107" />

---

### 9. Conectar via PowerShell
1. Acessar a pasta onde está o arquivo `labsuser.pem`  
2. Clicar com o botão direito e abrir **PowerShell**  
3. Colar o comando SSH copiado  
4. Alterar o arquivo em azul para `labuser.pem`  
5. Pressionar **Enter** e digitar `yes`

Agora já podemos trabalhar no Linux 🎉  
<img src="https://github.com/user-attachments/assets/b7807791-7e21-490b-8460-0f3f307cab4a" width="1195" />

---

### 10. Testando comandos básicos no Linux:

| Comando | Descrição | Exemplo de Uso |
|---------|-----------|----------------|
| `pwd` | Mostra em qual diretório você está | `/home/ec2-user` |
| `ls` | Lista os arquivos e pastas do diretório atual | `ls -l` → lista detalhada |
| `mkdir Campinho` | Cria uma pasta chamada **Campinho** | Após executar: `ls` → mostra a nova pasta |
| `touch aula01.txt` | Cria um arquivo chamado **aula01.txt** | Arquivo vazio criado no diretório atual |
| `nano aula01.txt` | Abre o arquivo **aula01.txt** no editor Nano | **Atalhos no Nano**:<br> `CTRL + O` → salvar<br> `CTRL + X` → sair |
| `cat aula01.txt` | Exibe o conteúdo do arquivo no terminal | Mostra o texto digitado no arquivo |

---
<img width="730" height="312" alt="image" src="https://github.com/user-attachments/assets/a16bbdbf-7b09-466c-8d2f-93ed70d404f7" />
<img width="739" height="97" alt="image" src="https://github.com/user-attachments/assets/32e0b7c8-fbb5-4e3c-8610-6498806fc969" />

---

### 15. Atualizar pacotes com o comando: 
```
sudo yum update -y
```
<img src="https://github.com/user-attachments/assets/de02b135-b44e-47b5-8d97-14a2b9801711" width="461" />

---

### 16. Instalar o Apache com o comando: 
```
sudo yum install httpd -y  
```
<img src="https://github.com/user-attachments/assets/94ac3aa7-15a5-4ac6-966d-799c2ec22ff8" width="1138" />

---

### 17. Iniciar e habilitar o Apache para iniciar automaticamente:
```
sudo systemctl start httpd
sudo systemctl enable httpd
```
<img src="https://github.com/user-attachments/assets/bee0ae42-366e-4270-8f58-d5cf7c8c7951" width="1042" />

---

### 18. Instalar o Git:
```
sudo yum install git -y
```
<img src="https://github.com/user-attachments/assets/c58661e7-56f1-4002-8208-d26fe8eeca8d" width="1157" />

---

### 19. Clonar o repositório
```
git clone <url-do-repositorio>
```
<img src="https://github.com/user-attachments/assets/8954b810-ccca-420a-918c-60f291c72c83" width="873" />

---

20. Copiar arquivos do projeto para o diretório do Apache
```
sudo cp -r <pasta-do-projeto>/* /var/www/html/
```
<img src="https://github.com/user-attachments/assets/d372ec0f-bc89-4f36-a014-c0acaa4ea4fa" width="714" />

---

21. Ajustar permissões
```
sudo chown -R apache:apache /var/www/html/
```
<img src="https://github.com/user-attachments/assets/90f2de18-47c8-4211-898e-af7075df3eaa" width="571" />

---

22. Acessar o site no navegador com o IPv4 público da instância EC2:  
http://<IPv4-público>  

🎉 Deploy concluído com sucesso no Amazon EC2! 🎉
<img width="1421" height="808" alt="image" src="https://github.com/user-attachments/assets/1ecdd34b-e1d9-45f2-b130-de5bf7d1cd14" />

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
