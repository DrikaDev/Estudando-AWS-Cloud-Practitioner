## 🧪 Lab 169 - Usar o AWS Systems Manager

O **AWS Systems Manager** é um conjunto de recursos que permite centralizar dados operacionais e automatizar tarefas em recursos da **Amazon Web Services (AWS)**.  
Ele pode gerenciar:  

- Instâncias do **Amazon EC2**
- Servidores **on-premises**
- **Máquinas virtuais**
- Outros recursos da AWS, em escala

Com o Systems Manager, você pode configurar, monitorar, atualizar e acessar seus sistemas de forma segura e automatizada — **sem depender de SSH ou portas abertas**.

## Objetivos

- ✅ Verificar configurações e permissões  
- ✅ Executar tarefas em vários servidores  
- ✅ Atualizar as configurações da aplicação  
- ✅ Acessar a linha de comando em uma instância  

## Tarefa 1: Gerar listas de inventário para instâncias gerenciadas

O **Fleet Manager** (parte do Systems Manager) coleta automaticamente metadados e informações de software de instâncias EC2, servidores locais e VMs.

### Passos

1. No console AWS, na barra de pesquisa, digite **Systems Manager** e acesse o serviço.
2. No menu esquerdo, em **Node Tools / Gerenciamento de nós**, selecione **Fleet Manager**.
3. Clique em **Configurar inventário**.

<img width="1338" height="393" alt="image" src="https://github.com/user-attachments/assets/93431db2-8efc-4d25-a902-90f0f782e5d7" />

4. Preencha:
   - **Nome**: `Inventory-Association`
   
   <img width="870" height="294" alt="image" src="https://github.com/user-attachments/assets/32255941-b1f1-4196-ab24-ae916d10bbce" />

   - **Destinos** → **Especificar destinos por**: *Selecionar instâncias manualmente*
   - Marque a **Instância gerenciada** listada
   
   <img width="856" height="365" alt="image" src="https://github.com/user-attachments/assets/bb111ac9-21ba-4b73-9caa-c29ad3548103" />

5. Clique em **Configurar inventário**.

> ✅ Um banner confirma: *"Solicitação para configurar inventário bem-sucedida"*.

6. Clique no **ID do nó** para abrir a **Visão geral do nó**.
7. Acesse a guia **Inventário**.

<img width="1355" height="686" alt="image" src="https://github.com/user-attachments/assets/3dc18bab-f7e1-4a52-a0a2-22af1701a8d2" />

> 🔍 Aqui você pode ver todas as aplicações instaladas, pacotes do sistema, configurações de rede e muito mais — **sem precisar de SSH**.

✅ **Conclusão da tarefa**: Você configurou o inventário automático do Systems Manager para monitorar o estado da instância.

## Tarefa 2: Instalar uma aplicação personalizada usando **Executar comando**

Vamos usar o recurso **Executar comando** para implantar uma aplicação web chamada **Widget Manufacturing Dashboard** — sem acessar a instância diretamente.

### Arquitetura

<img width="2391" height="1309" alt="image" src="https://github.com/user-attachments/assets/9cf24491-fbd5-4389-8668-97a5aae10860" />

> Uma instância EC2 em uma **VPC** recebe comandos do Systems Manager.  
> O script instala: **Apache**, **PHP**, **AWS SDK** e a aplicação web, e inicia o servidor.

### Passos

1. No Systems Manager, em **Node Management / Gerenciamento de nós**, clique em **Run Command / Executar comando**.
2. Clique em **Executar comando** (botão laranja).
3. Na caixa de pesquisa, **não digite texto**. Em vez disso:
   - Clique na lupa 🔍
   - Selecione **Owner** → **Owner by me**
4. Um documento chamado **“InstallDashboardApp / Instalar a aplicação de painel”** aparecerá. 
   Selecione ele.

<img width="1350" height="569" alt="image" src="https://github.com/user-attachments/assets/4134759a-c20b-4ae7-b852-65af7391e008" />

6. Em **Target selection / Seleção do destino**:
   - Escolha **Selecionar instâncias manualmente**
   - Marque a **Managed Instance / Instância gerenciada**

<img width="1065" height="461" alt="image" src="https://github.com/user-attachments/assets/89b6998d-369c-4f9d-9fd1-fe33e32e7d40" />

7. Em **Output options / Opções de saída**, **desmarque** *Enable an S3 bucket - Habilitar um bucket do S3*.

<img width="1070" height="229" alt="image" src="https://github.com/user-attachments/assets/1260ebb5-d3c7-4a83-98b5-5b909b04aac0" />


8. (Opcional) Expanda **AWS command line interface command / Comando de interface de linha de comando da AWS** para ver o comando CLI equivalente.

<img width="1063" height="356" alt="image" src="https://github.com/user-attachments/assets/83976b9e-f58f-4e79-8ebf-45296a6ac321" />

9. Clique em **Run / Executar**.

> ⏳ Aguarde 1–2 minutos. O status deve mudar para **Êxito**. 

<img width="1069" height="176" alt="image" src="https://github.com/user-attachments/assets/56b69004-cc4f-44b6-bf73-38a5b04f03ed" />

Atualize a página se necessário.

<img width="1069" height="190" alt="image" src="https://github.com/user-attachments/assets/8d8d2cd2-7a30-4594-874f-eb55526a1f48" />

### Verificar a aplicação

1. No painel do laboratório (ex: Vocareum), clique em **Detalhes** > **Mostrar**.
2. Copie o valor **ServerIP** (IP público).
3. Abra uma nova guia no navegador e cole o IP.

> 🎉 O **Widget Manufacturing Dashboard** será exibido!

<img width="1202" height="425" alt="image" src="https://github.com/user-attachments/assets/7499dc62-b30a-4946-8cd9-5331573d512a" />

✅ **Conclusão da tarefa**: Você implantou uma aplicação web remotamente usando o Systems Manager — **sem SSH**.

> Mantenha essa aba do painel "Widget Manufacturing Dashboard" aberta!!! 

## Tarefa 3: Usar o **Parameter Store / Armazenamento de parâmetros** para gerenciar configurações

O **Parameter Store** permite armazenar configurações e segredos de forma hierárquica e segura — como senhas, flags de feature, strings de conexão, etc.

### Objetivo

Ativar um recurso **beta** na aplicação web criando um parâmetro.

### Passos

1. No Systems Manager, em **Application Tools**, selecione **Parameter Store**.
2. Clique em **Criar parâmetro**.
3. Preencha:
   - **Nome**: `/dashboard/show-beta-features`
   - **Descrição**: `Display beta features`
   - **Tier**: `leave the default option` 
   - **Tipo**: `String` (padrão)
   - **Valor**: `True`

<img width="982" height="681" alt="image" src="https://github.com/user-attachments/assets/10e3a983-cae8-41cf-a517-2734757264fc" />

4. Clique em **Criar parâmetro**.

> ✅ Confirmação: *"Solicitação de criar parâmetro bem-sucedida"*.

5. Volte à guia do **Widget Manufacturing Dashboard** e **atualize a página**.

> 📊 Agora são exibidos **três gráficos** — o terceiro é um recurso beta ativado pelo parâmetro.

<img width="1198" height="420" alt="image" src="https://github.com/user-attachments/assets/0de44be6-f18c-4876-ad0a-4565cd105bb8" />

> 💡 **Dica opcional**: Exclua o parâmetro e atualize a página — o terceiro gráfico desaparece!

✅ **Conclusão da tarefa**: Você usou o **Parameter Store** para **ativar dinamicamente funcionalidades** em uma aplicação em execução.

## Tarefa 4: Usar o **Session Manager** para acessar instâncias

O **Session Manager** oferece acesso seguro à linha de comando de instâncias EC2 **sem abrir portas**, **sem chaves SSH** e com **auditoria total via AWS CloudTrail**.

<img width="2391" height="1062" alt="image" src="https://github.com/user-attachments/assets/50a32af2-7304-4999-9b94-c7d428fb8a87" />

### Passos

1. No Systems Manager, em **Node Tools**, selecione **Session Manager**.
2. Clique em **Iniciar sessão**.
3. Selecione a **Instância gerenciada**.
4. Clique em **Iniciar sessão**.

<img width="1339" height="608" alt="image" src="https://github.com/user-attachments/assets/d1b73a46-451b-482d-8fe0-90858a6137af" />

> 🖥️ Uma janela de terminal baseada em navegador será aberta.

### Comandos de teste

Execute os seguintes comandos na sessão:
`ls /var/www/html`
<img width="1009" height="72" alt="image" src="https://github.com/user-attachments/assets/de7b0768-eceb-49c7-9cd9-a4757dc1e5f2" />

### Obter região da instância 
```
AZ=curl -s http://169.254.169.254/latest/meta-data/placement/availability-zone
export AWS_DEFAULT_REGION=${AZ::-1}
```

### Listar detalhes da EC2 instances via AWS CLI  
`aws ec2 describe-instances`

<img width="822" height="706" alt="image" src="https://github.com/user-attachments/assets/053bafe9-3bd8-485b-9516-bbda0cd2d080" />

### 📋 A saída mostra os detalhes da instância em formato JSON — confirmando que as credenciais de instância estão funcionando.  
✅ Conclusão da tarefa: Você acessou a instância com segurança via Session Manager — sem SSH, sem chaves, sem riscos.  

## 🔍 Por que isso acontece no Session Manager?

A instância tem uma IAM Role (então credenciais OK ✅)  
Mas não há configuração prévia de região no ambiente (~/.aws/config não existe)  
Portanto, a CLI não sabe em qual região procurar as instâncias  
EC2 é um serviço regional, então você sempre precisa especificar a região.  


Qwen3-Max
9:56 PM
Claro! Aqui está o conteúdo formatado em Markdown, pronto para ser incluído na sua documentação no GitHub:

markdown
123456789101112131415161718192021222324252627282930313233343536373839404142434445464748495051525354555657585960616263646566676869707172737475767778798081828384858687888990919293949596979899100101102103104105106107108109110111112113114115116117118119120121122123124125126127128129130131132133134135136137138139140141142143144145146147148149150151152153154155156157158159160161
# Usar o AWS Systems Manager

## Visão geral do laboratório

O **AWS Systems Manager** é um conjunto de recursos que permite centralizar dados operacionais e automatizar tarefas em recursos da **Amazon Web Services (AWS)**. Ele pode gerenciar:

- Instâncias do **Amazon EC2**
- Servidores **on-premises**
- **Máquinas virtuais**
- Outros recursos da AWS, em escala

bash
1234
# Obter região da instância e listar detalhes via AWS CLI
AZ=`curl -s http://169.254.169.254/latest/meta-data/placement/availability-zone`
export AWS_DEFAULT_REGION=${AZ::-1}
aws ec2 describe-instances
📋 A saída mostra os detalhes da instância em formato JSON — confirmando que as credenciais de instância estão funcionando.

✅ Conclusão da tarefa: Você acessou a instância com segurança via Session Manager — sem SSH, sem chaves, sem riscos.

## Conclusão - 🎉 Parabéns! Você concluiu com sucesso:

- Verificar configurações e permissões com Inventário
- Executar tarefas remotas com Executar comando
- Atualizar configurações da aplicação com Parameter Store
- Acessar a linha de comando com Session Manager

O AWS Systems Manager demonstra como é possível gerenciar infraestrutura de forma segura, escalável e automatizada — seguindo as melhores práticas da AWS.  

🔐 Dica final: O Session Manager e o Parameter Store são amplamente usados em arquiteturas zero-trust e compliance rigoroso (ex: PCI, HIPAA).  

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
