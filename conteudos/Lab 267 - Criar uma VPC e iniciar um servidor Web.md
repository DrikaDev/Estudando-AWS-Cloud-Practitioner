## 🧪 Lab 267 - Criação de uma VPC e iniciar um servidor Web

Neste laboratório, vamos usar a **Amazon Virtual Private Cloud (VPC)** para:  

1. Criar sua própria **VPC**  
2. Adicionar componentes adicionais a ela para produzir uma rede personalizada para um cliente **Fortune 100**  
3. Criar **grupos de segurança** para sua instância do **EC2**  
4. Configurar e personalizar uma instância do **EC2** para executar um **servidor web**  
5. Iniciá-la na **VPC** criada, que segue o modelo solicitado pelo cliente.

---

### Diagrama do cliente

O cliente está solicitando a construção da seguinte arquitetura para iniciar seu servidor web com êxito:  

- Uma **VPC**  
- **Sub-redes** (públicas e privadas)  
- Um **grupo de segurança**  
- Uma **instância Amazon EC2** configurada para rodar um **servidor web**  

<img width="1114" height="511" alt="image" src="https://github.com/user-attachments/assets/ac797c63-780b-44ef-9720-9915caed3e39" />

---

### Tarefa 1: Criar a VPC

Nesta tarefa, vamos utilizar o **Assistente de VPC** para criar:  

- Uma **VPC**
- Um **gateway da Internet**
- Duas **sub-redes** (pública e privada) em uma única Zona de Disponibilidade

📌 O **gateway da Internet** permite a comunicação entre instâncias da VPC e a **Internet**.  
📌 Uma sub-rede é considerada:  
- **Pública** → quando possui rota para o gateway da Internet.  
- **Privada** → quando **não** possui rota para o gateway da Internet.  

> Além disso, o assistente criará automaticamente um **NAT Gateway**, usado para fornecer conectividade à Internet para instâncias do EC2 em sub-redes privadas.

1. Escolha **Criar VPC** e configure as seguintes opções:

- **Recursos a serem criados:** escolha **VPC e muito mais**  
![image](https://github.com/user-attachments/assets/97b9eb34-0e26-4b1d-93d5-360352d28ee5)

- **Geração automática da etiqueta de nome:** desmarque a caixa **Gerar automaticamente**  
![image](https://github.com/user-attachments/assets/4b749bb3-3bba-465e-bba3-8fc7f9538f50)

- **IPv4 CIDR:** insira `10.0.0.0/16`  
![image](https://github.com/user-attachments/assets/9f378b62-d7c0-401b-87b5-3dfc7dcd8d40)

- **IPv6 CIDR block:** selecione **No IPv6 CIDR Block (Nenhum bloco CIDR IPv6)**  
![image](https://github.com/user-attachments/assets/0e0058e2-af1c-479f-a906-dc7a548810b4)

- **Tenancy:** escolha **Padrão**  
![image](https://github.com/user-attachments/assets/50dc2c81-078a-4f14-914d-971506a072e0)

- **Número de Zonas de Disponibilidade (AZs):** `1`  
![image](https://github.com/user-attachments/assets/eefb0d69-6fd4-453e-ad38-ea97688ba122)

- **Número de sub-redes públicas:** `1`  
![image](https://github.com/user-attachments/assets/97cb1d65-0961-46d5-8300-d4c8d4445268)

- **Número de sub-redes privadas:** `1`  
![image](https://github.com/user-attachments/assets/93b7cc5a-ac9e-4a81-b698-8d31c833645e)

- Expanda **Personalizar blocos CIDR de sub-redes** e configure:
   - Public subnet CIDR: `10.0.0.0/24`
   - Private subnet CIDR: `10.0.1.0/24`

![image](https://github.com/user-attachments/assets/700be8c8-a388-4246-8e42-b75a8d5626af)

- **Gateways NAT:** selecione **In 1 AZ (Em 1 AZ)**  
![image](https://github.com/user-attachments/assets/7769397a-62d6-49b1-b8a1-f12d613eed4c)

- **Endpoints da VPC:** escolha **Nenhum**  
![image](https://github.com/user-attachments/assets/1861bb61-1b8a-4bb5-9835-11f28508723f)

- No painel **Visualização**, nomeie os recursos:
   - **VPC:** `Lab VPC`  
   - **Subnets (2):**  
      - `Public Subnet 1`  
      - `Private Subnet 1`  

![image](https://github.com/user-attachments/assets/48eec9e1-f6e2-4b02-9d42-5770a3ca1fcd)

   - **Tabelas de rota (2):**  
      - `Public Route Table`  
      - `Private Route Table`

![image](https://github.com/user-attachments/assets/fa57ca00-1ba9-4153-92b7-541cd1b73832)

- Clique em **Criar VPC**

![image](https://github.com/user-attachments/assets/af103a91-0f76-4000-8b29-5c3c58bdab2e)  

![image](https://github.com/user-attachments/assets/48c4bc2a-a392-45ee-8fd3-b0900e96133a)

> Ao final desta tarefa, teremos criado a **Lab VPC** com:  
>  - Uma sub-rede pública  
>  - Uma sub-rede privada  
>  - Gateway da Internet  
>  - NAT Gateway  
>  - Tabelas de rota configuradas

---

### Tarefa 2: Criar sub-redes adicionais

Nesta tarefa, vamos criar **duas sub-redes adicionais** em uma segunda **Zona de Disponibilidade (AZ)**.  
Isso garante **alta disponibilidade**, permitindo que recursos sejam distribuídos em múltiplas zonas.

#### 1. Criar a segunda sub-rede **pública**:  

- No menu lateral, vá em **Sub-redes** → **Criar sub-rede**  
![image](https://github.com/user-attachments/assets/d32b3161-f857-45ab-9fe8-9c202cbe97cc)

- **VPC ID:** escolha `Lab VPC`  
![image](https://github.com/user-attachments/assets/9ebe24f1-3f55-47a9-a4da-6f60a1181d60)

- **Nome da sub-rede:** `Public Subnet 2`  
![image](https://github.com/user-attachments/assets/46e57b0b-2455-4284-982c-6a4c14073643)

- **Zona de disponibilidade:** `Sem preferências`  
![image](https://github.com/user-attachments/assets/07932a02-2621-4b8b-aed4-9a20a8598b00)

- **IPv4 CIDR block:** `10.0.2.0/24`  
![image](https://github.com/user-attachments/assets/50dbf879-cec1-4571-8b10-026ba42aecae)

- Clique em **Criar sub-rede**  
![image](https://github.com/user-attachments/assets/bca749c1-ce36-44eb-9561-fa85722837de)

📌 Essa sub-rede terá endereços IP que começam com **10.0.2.x**.  

#### 2. Criar a segunda sub-rede **privada**:  

- Clique em **Criar sub-rede** novamente e configure:  
   - **VPC ID:** `Lab VPC`  
   - **Nome:** `Private Subnet 2`  
   - **Zona de disponibilidade:** `Sem preferências`  
   - **IPv4 CIDR block:** `10.0.3.0/24`  

![image](https://github.com/user-attachments/assets/5dafac08-9470-49f3-8538-28482161ea76)  

![image](https://github.com/user-attachments/assets/45b22829-e023-45ba-86a8-54fddb5c6b87)

- Clique em **Criar sub-rede**  

📌 Essa sub-rede terá endereços IP que começam com **10.0.3.x**.  

### Resultado Esperado

Ao final desta tarefa, você teremos:  

- **Public Subnet 2** (`10.0.2.0/24`)  
- **Private Subnet 2** (`10.0.3.0/24`)  

> Essas sub-redes complementam as criadas anteriormente, garantindo **redundância e disponibilidade** em múltiplas Zonas de Disponibilidade.

---

### Tarefa 3: Associar as sub-redes e adicionar rotas

Nesta tarefa, vamos associar as **sub-redes adicionais** às tabelas de rotas correspondentes (pública e privada).  
Isso garante que as sub-redes possam se comunicar corretamente e que a VPC funcione como uma **rede totalmente configurada**.  

- No painel de navegação à esquerda, escolha **Tabelas de rotas**.
- Selecione **Tabela de rotas públicas**

![image](https://github.com/user-attachments/assets/6b001e17-bed0-4026-970e-598981887fd2)

- No painel inferior, escolha a guia **Associações de sub-rede**  

![image](https://github.com/user-attachments/assets/b40bbca7-44ad-40ee-8986-f4fe82948b59)

- Em **Sub-redes sem associações explícitas**, escolha **Editar associações de sub-rede**  

![image](https://github.com/user-attachments/assets/8bb197d3-9da6-484b-8827-eab6542db197)

- Marque a caixa de seleção **Public Subnet 2** e clique em **Salvar associações**  

![image](https://github.com/user-attachments/assets/4ef26da4-7162-400f-9cbe-80e5df8809aa)

---

### Associar a Private Subnet 2 à Tabela de Rota Privadas

- Selecione **Tabela de rotas privadas**  

![image](https://github.com/user-attachments/assets/6d8b3ada-798a-42b6-b5df-bce22276e165)

- No painel inferior, escolha a guia **Associações de sub-rede**  

- Em **Sub-redes sem associações explícitas**, clique em **Editar associações de sub-rede**  

- Marque a caixa de seleção **Private Subnet 2** e clique em **Salvar associações**  

![image](https://github.com/user-attachments/assets/6d31037a-069a-4811-ac6e-b8c5d8f87f9c)

**✅ Resultado Esperado**
Agora a **VPC** possui sub-redes **públicas e privadas** configuradas em **duas Zonas de Disponibilidade**.  

> 📌 Estrutura Final:  
> - Sub-redes públicas associadas à **Tabela de Rotas Públicas**.  
> - Sub-redes privadas associadas à **Tabela de Rotas Privadas**.  

![image](https://github.com/user-attachments/assets/f3db56ca-f635-4563-ba17-c77bc9331760)

---

### Tarefa 4: Criar um grupo de segurança da VPC

Nesta tarefa, vamos criar um **grupo de segurança** da VPC, que funcionará como um **firewall virtual** para sua instância.  
Um grupo de segurança controla o tráfego de entrada e saída das instâncias do **Amazon EC2** associadas a ele.  

- No painel de navegação à esquerda, escolha **Grupos de segurança**  

- Clique em **Criar grupo de segurança**  

![image](https://github.com/user-attachments/assets/8fe2d50d-2b7f-48f9-bea9-f2ed0e97ebe5)

- Configure:  
   - **Nome:** `Web Security Group`  
   - **Descrição:** `Enable HTTP access`  
   - **VPC:** `Lab VPC`  

![image](https://github.com/user-attachments/assets/bf926d0d-c3a6-46e6-8a23-036cec9f3b4b)

- Em **Regras de entrada**, clique em **Adicionar regra** e configure:  
   - **Tipo:** `HTTP`  
   - **Origem:** `Anywhere-IPv4`  
   - **Descrição:** `Permit web requests`  

![image](https://github.com/user-attachments/assets/015493b7-83fa-4030-b84f-849f6be2b43a)

- Clique em **Criar grupo de segurança**  

![image](https://github.com/user-attachments/assets/6c0d3f91-c0ba-4d62-8aab-816e5980e330)

**✅ Resultado Esperado**  

Um grupo de segurança chamado **Web Security Group** foi criado e configurado para permitir tráfego HTTP (porta 80) de qualquer origem IPv4.  

📌 Esse grupo de segurança será usado na **próxima tarefa** para associar a uma instância do Amazon EC2.  

---

### Tarefa 5: Iniciar uma instância de servidor web

Nesta tarefa, vamos iniciará uma instância do **Amazon EC2** dentro da **Lab VPC**.  
A instância será configurada como um **servidor web** que hospeda uma aplicação simples.  

- No **Console da AWS**, procure por **EC2**.  
- Clique em **Instâncias** → **Executar instâncias**  

- Configure:  
- **Nome e tags**: `Web Server 1`  

- **Application and OS Images (AMI)**  
   - Início rápido: **Amazon Linux**  
   - AMI: **Amazon Linux 2 AMI (HVM)**  

![image](https://github.com/user-attachments/assets/f45614a2-1971-466a-bd2b-19c3e33a685f)

- **Tipo de instância**: `t3.micro`  

![image](https://github.com/user-attachments/assets/7174145f-a2e1-4e05-b129-669c7957c7ac)

- **Par de chaves (login)**: `vockey`  
![image](https://github.com/user-attachments/assets/05e2ad3f-a70f-435e-accc-cb618b8d3bd0)

- **Configurações de rede**  
   - **VPC:** `Lab VPC`  
   - **Sub-rede:** `Public Subnet 2`  
   - **Atribuir IP público automaticamente:** `Habilitar`  
   - **Grupo de segurança:** `Web Security Group`  

![image](https://github.com/user-attachments/assets/9b60ac0c-3c9b-4f71-a70c-e571e0a92b96)

- Expanda **Detalhes avançados** e, em **Dados do usuário**, insira o seguinte script:

```
#!/bin/bash
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd

cat <<EOF > /var/www/html/index.html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>BRSAO215 - AWS re/Start</title>
  <style>
    body {
      background-color: #1A237E; /* Azul escuro elegante */
      font-family: Verdana, sans-serif;
      color: #FFEB3B; /* Amarelo claro */
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 100vh;
      margin: 0;
    }
    h1 {
      font-size: 28px;
      margin-top: 20px;
      text-align: center;
    }
    img {
      width: 220px;
    }
  </style>
</head>
<body>
  <a href="https://aws.amazon.com/what-is-cloud-computing" target="_blank">
    <img src="https://d0.awsstatic.com/logos/powered-by-aws-white.png" alt="Powered by AWS Cloud Computing">
  </a>
  <h1>BRSAO215 é a melhor turma de AWS re/Start 2025</h1>
</body>
</html>
EOF
```

- Escolha **Executar instância**.  

- Para exibir a instância em execução, clique em **Visualizar todas as instâncias**.  
  Aguarde até que o **Web Server 1** mostre **3/3 checks passed (2/2 verificações aprovadas)** na coluna *Verificação de status*.  

  ![instance-status](https://github.com/user-attachments/assets/dda4f2f6-dcc6-4e97-ae72-9c1cbc09ed2b)

- Conectar ao servidor web:
   - Marque a caixa de seleção da instância **Web Server 1** e abra a guia **Detalhes**.
   - Copie o valor **Public IPv4 DNS** (DNS IPv4 público).
   - Abra uma nova aba no navegador, cole o **Public IPv4 DNS** e pressione **Enter**.  
   
   ![public-dns](https://github.com/user-attachments/assets/caac392e-0f50-46b0-ae53-cb48b380f878)

- Resultado esperado  
  Se tudo deu certo, a página exibida no navegador deve corresponder à página do servidor web do laboratório.  

![Imagem do WhatsApp de 2025-08-07 à(s) 11 29 28_97015cbf](https://github.com/user-attachments/assets/9d50ec86-c80c-4131-992a-d043d57978c0)

- Veja a seguir a arquitetura completa que você implantou:

<img width="1114" height="511" alt="image" src="https://github.com/user-attachments/assets/d5e62731-a1fd-49d9-bde2-c8c43e88d359" />

> Uma VPC totalmente funcional com seus recursos (rede e segurança) e um servidor web.  

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
