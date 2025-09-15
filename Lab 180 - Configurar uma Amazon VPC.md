## 🧪 Lab - Configurar uma VPC

### 🔎 Visão Geral do Laboratório

O **Amazon Virtual Private Cloud (Amazon VPC)** possibilita que você provisione uma seção da nuvem **Amazon Web Services (AWS)** 
isolada logicamente, em que é possível executar recursos da AWS em uma rede virtual que você define por conta própria.  

Você tem **controle total** sobre o ambiente de rede virtual, incluindo:  
- Seleção dos intervalos de endereços IP  
- Criação de sub-redes  
- Configuração de tabelas de rota  
- Configuração de gateways de rede  

Neste laboratório, vamos criar:  
- Uma **VPC (nuvem privada virtual)**  
- Os **componentes de rede necessários** para implantar recursos, como uma instância do **Amazon Elastic Compute Cloud (Amazon EC2)**.

### Arquitetura:
<img width="1460" height="878" alt="image" src="https://github.com/user-attachments/assets/9f163efb-9cee-4dac-a697-70f7aea1dc15" />

---

## Tarefa 1: Criar uma VPC

1. No **Console de Gerenciamento da AWS**, na barra de pesquisa, insira e escolha **VPC**.  
2. No painel de navegação à esquerda, em **Nuvem privada virtual**, selecione **Suas VPCs**.  

⚠️ Observação: uma **VPC padrão** com um bloco de roteamento entre domínios sem classe (**CIDR**) `172.31.0.0/16` já foi criada 
para você em cada Região. Mesmo que ainda não tenha criado nada em sua conta, alguns recursos de VPC pré-existentes estarão disponíveis.  

3. Clique em **Criar VPC** e configure as seguintes opções:  

   - **Recursos a serem criados**: selecione `Somente VPC`  
   - **Tag de nome**: insira `Lab VPC`  
   - **IPv4 CIDR block (Bloco CIDR IPv4)**: selecione `IPv4 CIDR manual input (Entrada manual de CIDR IPv4)`  
   - **IPv4 CIDR (CIDR IPv4)**: insira `10.0.0.0/16`  
   - **IPv6 CIDR block (Bloco CIDR IPv6)**: selecione `No IPv6 CIDR block (Nenhum bloco CIDR IPv6)`  
   - **Locação**: escolha `Padrão`  
   - **Tags**: mantenha as sugeridas  

<img width="1420" height="646" alt="image" src="https://github.com/user-attachments/assets/25280130-6a16-4959-99ba-0dd9610d2833" />

4. Clique em **Criar VPC**.  

Na parte superior da página, será exibida a mensagem:  
> “Você criou com sucesso o vpc-xxxxxxxxxxxx / Lab VPC.”

<img width="1206" height="41" alt="image" src="https://github.com/user-attachments/assets/5b796789-be13-4c34-a374-87fe8440e7e2" />

5. Selecione **Ações** e clique em **Editar configurações da VPC**.  

6. Na seção **Configurações de DNS**, marque a opção **Habilitar nomes de host DNS**.  

<img width="1423" height="114" alt="image" src="https://github.com/user-attachments/assets/fd33373f-0641-4217-8f3f-18115d20eacd" />

7. Clique em **Salvar**.  

✅ Agora, as instâncias do **Amazon EC2** iniciadas na VPC receberão automaticamente um **nome de host DNS IPv4 público**.

---

## Tarefa 2: Criar Sub-redes

Nesta tarefa, vamos criar uma **sub-rede pública** e uma **sub-rede privada**.

---

### Tarefa 2.1: Criar uma Sub-rede Pública

1. No painel de navegação à esquerda, em **Nuvem privada virtual**, selecione **Sub-redes**.  

2. Clique em **Criar sub-rede** e configure as seguintes opções:  
   - **ID da VPC**: escolha `Lab VPC`  
   - **Nome da sub-rede**: insira `Public Subnet`  
   - **Zona de disponibilidade**: escolha a **primeira Zona de Disponibilidade** na lista (não selecione *Sem preferência*)  
   - **IPv4 CIDR block (Bloco CIDR IPv4)**: insira `10.0.0.0/24`  

3. Clique em **Criar sub-rede**.  

#### ⚙️ Configurar atribuição automática de IP público

Agora, configure a **sub-rede pública** para atribuir automaticamente um endereço IPv4 público a todas as instâncias do EC2 iniciadas nela:

1. Selecione **Sub-rede pública**.  

2. Clique em **Ações** e selecione **Editar configurações da sub-rede**.  

<img width="1425" height="252" alt="image" src="https://github.com/user-attachments/assets/de09b87d-37f9-42e7-8957-faba1968ed21" />

3. Na seção **Configurações de atribuição automática de IP**, marque a opção:  
   - `Enable auto-assign public IPv4 address (Habilitar endereço IPv4 público de atribuição automática)`  

<img width="1408" height="166" alt="image" src="https://github.com/user-attachments/assets/4c19caa7-abe3-4358-8a04-e259d1ba9170" />

4. Clique em **Salvar**.  

> ℹ️ Mesmo que essa sub-rede tenha o nome **Sub-rede pública**, ela ainda **não é pública**!  
> Para ser considerada pública, será necessário anexar um **gateway de internet**, o que será feito em outra tarefa do laboratório.

---

### Tarefa 2.2: Criar uma Sub-rede Privada

Nesta tarefa, vamos criar a **sub-rede privada**, que será usada para recursos que devem permanecer **isolados da internet**.  

1. No painel de navegação à esquerda, em **Nuvem privada virtual**, selecione **Sub-redes**.  

2. Clique em **Criar sub-rede** e configure as seguintes opções:  
   - **ID da VPC**: escolha `Lab VPC`  
   - **Nome da sub-rede**: insira `Private Subnet`  
   - **Zona de disponibilidade**: escolha a **primeira Zona de Disponibilidade** na lista (não selecione *Sem preferência*)  
   - **IPv4 CIDR block (Bloco CIDR IPv4)**: insira `10.0.2.0/23`  

2. Clique em **Criar sub-rede**.  

---

#### ℹ️ Observações

- O bloco CIDR `10.0.2.0/23` inclui todos os endereços IP que começam com `10.0.2.x` e `10.0.3.x`.  
- Essa faixa é **duas vezes maior que a sub-rede pública**, pois a maioria dos recursos devem ser mantidos em **sub-redes privadas**,
  a menos que precisem estar acessíveis pela internet.  
- Sua **VPC** agora possui **duas sub-redes**.  

<img width="1430" height="198" alt="image" src="https://github.com/user-attachments/assets/2bce61ae-291e-48f9-91a1-108d47cc0f4d" />

⚠️ No entanto, a VPC ainda está totalmente **isolada** e não pode se comunicar com recursos fora dela.  
➡️ Na próxima tarefa, você configurará a **sub-rede pública** para se conectar à internet por meio de um **gateway de internet**.

---

## Tarefa 3: Criar um Gateway de Internet

Nesta tarefa, vamos criar um **gateway de internet** para a VPC.  
O gateway de internet é necessário para estabelecer **conectividade externa** com instâncias do EC2 nas VPCs.  

1. No painel de navegação à esquerda, em **Nuvem privada virtual**, selecione **Gateways da internet**.  

2. Clique em **Criar gateway de Internet** e configure:  
   - **Tag de nome**: insira `Lab IGW`  

<img width="1421" height="455" alt="image" src="https://github.com/user-attachments/assets/9b9c3489-92d3-4779-933d-00d2c4e96661" />

3. Clique em **Criar gateway da Internet**.  

4. Selecione o gateway recém-criado, clique em **Ações** e escolha **Associar a uma VPC**.  
   - Escolha a **Lab VPC**.  

<img width="1210" height="96" alt="image" src="https://github.com/user-attachments/assets/05c9ad75-d150-4164-839f-d75a840a3ac1" />

✅ Agora, sua **sub-rede pública** tem uma conexão com a internet.  
⚠️ Entretanto, para que o tráfego seja roteado corretamente, será necessário **configurar a tabela de rota da sub-rede pública** 
para utilizar o **gateway de internet** — o que será feito na próxima tarefa.  

---

## Tarefa 4: Configurar Tabelas de Rota

Nesta tarefa, vamos fazer os seguintes passos:  
- Criar uma **tabela de rota pública** para tráfego vinculado à internet.  
- Adicionar uma rota à tabela de rota para direcionar o tráfego para o **gateway de internet**.  
- Associar a **sub-rede pública** à nova tabela de rota.  

1. No painel de navegação à esquerda, em **Nuvem privada virtual**, selecione **Tabelas de rotas**.  
   - Várias tabelas de rota estarão listadas.  

2. Escolha a tabela de rota que inclui **Lab VPC** na coluna **VPC**.  

<img width="1209" height="176" alt="image" src="https://github.com/user-attachments/assets/d4f49288-424f-404e-b64f-4718d4d4151f" />

3. Na coluna **Nome**, clique no ícone de edição, insira `Private Route Table` e clique em **Salvar**.  

4. Selecione a guia **Rotas**.  
   - No momento, existe apenas **uma rota**: ela mostra que todo o tráfego destinado a `10.0.0/16` (o intervalo da **Lab VPC**)
     será roteado **localmente**.  
   - Isso permite que todas as sub-redes dentro da VPC se comuniquem.  

### 🔹 Criar a Tabela de Rota Pública

1. Clique em **Criar tabela de rotas** e configure:  
   - **Nome (opcional)**: insira `Public Route Table`  
   - **VPC**: selecione `Lab VPC`  

2. Clique em **Criar tabela de rotas**.  

---

### 🔹 Adicionar Rota para a Internet

1. Após criar a tabela, vá até a guia **Rotas**.  
2. Clique em **Editar rotas**.  

<img width="1209" height="176" alt="image" src="https://github.com/user-attachments/assets/9be9aad8-e03f-4cb5-a6d4-e5446f0e2232" />

3. Selecione **Adicionar rota** e configure:  
   - **Destino**: insira `0.0.0.0/0`  
   - **Alvo**: selecione **Gateway da Internet** e escolha `Lab IGW` na lista  

4. Clique em **Salvar alterações**.  

---

### 🔹 Associar a Tabela de Rota Pública à Sub-rede Pública

1. Na tabela de rota pública, selecione a guia **Associações de sub-rede**.  
2. Clique em **Editar associações de sub-rede**.  

<img width="1191" height="226" alt="image" src="https://github.com/user-attachments/assets/d51c8abf-f63b-427b-b42c-fcf60eaab5ba" />

3. Selecione **Sub-rede pública**.  
4. Clique em **Salvar associações**.  

✅ Agora, a **sub-rede pública** é realmente **pública**, pois possui uma entrada na tabela de rota que envia tráfego para a 
internet por meio do **gateway de internet**.  

---

### 📌 Recapitulando
- Criamos uma **VPC**.  
- Anexamos um **gateway de internet**.  
- Criamos **sub-redes** (pública e privada).  
- Configuramos uma **tabela de rota pública** e a associou à sub-rede pública.  

➡️ Agora já é possível **iniciar recursos nas sub-redes conforme necessário**.

---

## Tarefa 5: Iniciar um Servidor Bastion na Sub-rede Pública

Um **servidor bastion** (também conhecido como *jump box*) é uma instância do **EC2** em uma sub-rede pública, configurada com 
segurança para fornecer acesso a recursos em uma **sub-rede privada**.  

👉🏻 Os operadores de sistemas podem se conectar ao **servidor bastion** e, a partir dele, acessar os recursos da sub-rede privada.  

Nesta tarefa, vamos iniciar um **servidor bastion** (instância EC2) na **sub-rede pública** criada anteriormente.

1. No **Console de Gerenciamento da AWS**, na barra de pesquisa, insira e escolha **EC2**.  

2. No painel de navegação à esquerda, selecione **Instâncias**.  

3. Clique em **Executar instâncias** e configure:  

#### 📌 Nome e Tags
- **Nome**: `Bastion Server`  

#### 📌 Imagem (AMI)
- **Início rápido**: escolha `Amazon Linux`  
- **AMI**: selecione `Amazon Linux 2023 AMI`  

#### 📌 Tipo de Instância
- Selecione `t3.micro`  

#### 📌 Par de Chaves (login)
- Escolha `Prosseguir sem um par de chaves (não recomendado)`  
- ℹ️ Neste laboratório, o acesso será feito via **EC2 Instance Connect**, portanto o par de chaves não será necessário.  

#### 📌 Configurações de Rede - cliquem em Editar
- **VPC**: escolha `Lab VPC`  
- **Sub-rede**: escolha `Sub-rede pública`  
- **Atribuir IP público automaticamente**: selecione **Habilitar**  

#### 📌 Firewall (Grupo de Segurança)
- Selecione **Criar grupo de segurança**  
- **Nome do grupo de segurança**: `Bastion Security Group`  
- **Descrição**: `Allow SSH`  
- **Regras de entrada**:  
  - **Tipo**: `SSH`  
  - **Origem**: `Qualquer lugar (0.0.0.0/0)`  

4. Clique em **Executar instância**.  

5. Para visualizar a instância iniciada, escolha **Visualizar todas as instâncias**.  
   - Inicialmente, a instância `Bastion Server` estará no estado **Pendente**.  
   - Após alguns instantes, o estado mudará para **Em execução**, indicando que a instância foi inicializada.  

✅ O **servidor bastion** foi iniciado na **sub-rede pública**.  
➡️ Prossiga para a próxima tarefa. (*Não é necessário aguardar até a instância estar em execução.*)

---

## Tarefa 6: Criar um Gateway NAT

Nesta tarefa, vamos iniciar um **gateway NAT** na **sub-rede pública** e configurar a **tabela de rota privada** para permitir que 
os recursos da **sub-rede privada** se comuniquem com a internet.  

### 🔹 Criar o Gateway NAT

1. No **Console de Gerenciamento da AWS**, na barra de pesquisa, insira **NAT gateways**.  

2. Selecione a lista **Recursos** e clique em **Gateways NAT**.  

3. Clique em **Criar gateway NAT** e configure:  
   - **Nome**: `Lab NAT gateway`  
   - **Sub-rede**: selecione `Sub-rede pública`  
   - **IP elástico**: clique em **Alocar IP elástico** em azul  

<img width="1417" height="557" alt="image" src="https://github.com/user-attachments/assets/7248bd8d-a9f0-440e-87d6-e253b7ad528f" />

4. Clique em **Criar um gateway NAT**.  

---

### 🔹 Configurar a Rota da Sub-rede Privada

1. No painel de navegação à esquerda, selecione **Tabelas de rotas**.  

2. Escolha a **Tabela de rotas privadas**.  

3. Vá até a guia **Rotas**.  
   - No momento, a tabela mostra apenas um registro, que roteia o tráfego **localmente dentro da VPC**.  

4. Clique em **Editar rotas**.  

5. Selecione **Adicionar rota** e configure:  
   - **Destino**: `0.0.0.0/0`  
   - **Alvo**: selecione **Gateway NAT** e escolha o `nat-xxxxxxxx` da lista  

6. Clique em **Salvar alterações**.  

✅ Agora, os **recursos na sub-rede privada** que desejarem se comunicar com a internet terão seu tráfego roteado pelo **gateway NAT**.  
➡️ O gateway NAT encaminha a solicitação para a internet, e as respostas retornam por ele de volta à sub-rede privada.

---

## Desafio Opcional: Testar a Sub-rede Privada  

Neste desafio, vamos iniciar uma instância do **Amazon EC2** na **sub-rede privada** e confirmar que ela pode se comunicar com a 
internet.  

1. No **Console de Gerenciamento da AWS**, na barra de pesquisa, insira e escolha **EC2**.  

2. No painel de navegação à esquerda, selecione **Instâncias**.  

3. Clique em **Executar instâncias** e configure:

- **Nome e tags**  
  - Nome: `Private Instance`  

- **Application and OS Images (AMI)**  
  - Início rápido: **Amazon Linux**  
  - AMI: **Amazon Linux 2023 AMI**  

- **Tipo de instância**  
  - Selecione: `t3.micro`  

- **Par de chaves (login)**  
  - Escolha: **Prosseguir sem um par de chaves (não recomendado)**  

- **Configurações de rede**  / Editar
  - **VPC (obrigatório):** `Lab VPC`  
  - **Sub-rede:** `Private Subnet` (não a pública)  
  - **Firewall (grupos de segurança):** Criar novo grupo  
    - Nome do grupo: `Private Instance SG`  
    - Descrição: `Allow SSH from Bastion`  

    **Regras de entrada (Inbound rules):**  
    - Tipo: `SSH`  
    - Origem: `Personalizado` → `10.0.0.0/16`  

#### 🔹 Dados do usuário (User Data)  

Expanda a seção **Detalhes avançados** e insira o seguinte script em **Dados do usuário (opcional):**  

```
#!/bin/bash
# Turn on password authentication for lab challenge
echo 'lab-password' | passwd ec2-user --stdin
sed -i 's|[#]*PasswordAuthentication no|PasswordAuthentication yes|g' /etc/ssh/sshd_config
systemctl restart sshd.service
```

> ⚠️ Esse script permite o login usando uma senha.  
> Ele foi incluído para ajudar a encurtar as etapas do laboratório, mas **não é recomendado** para implementações normais de instâncias.

4. Clique em **Executar instância**.  

5. Para exibir a instância iniciada, selecione **Visualizar todas as instâncias**.

<img width="1424" height="214" alt="image" src="https://github.com/user-attachments/assets/f1f615c7-9e82-4e31-9c78-abef39c2b4f7" />

---

## Fazer Login no Servidor Bastion

A instância que acabamos de iniciar está na **sub-rede privada**, portanto não é possível acessá-la diretamente.  
Em vez disso, faremos login **primeiro no servidor Bastion** (sub-rede pública) e depois acessaremos a instância privada a partir dele.

### 🔹 Passos para acessar o Bastion

1. No **Console de Gerenciamento da AWS**, na barra de pesquisa, insira e escolha **EC2** para abrir o Console de Gerenciamento do EC2.  
2. No painel de navegação, selecione **Instâncias**.  
3. Na lista de instâncias, clique na instância **Bastion Server**.  
4. Clique em **Conectar-se**.  
5. Na guia **EC2 Instance Connect**, clique em **Conectar-se**.  

> ℹ️ Observação: se preferir usar um **cliente SSH** para se conectar à instância do EC2, consulte as orientações em *Conecte-se à sua instância do Linux*.

