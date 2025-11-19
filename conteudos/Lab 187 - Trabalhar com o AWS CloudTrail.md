## 🧪 Lab 187 - Trabalhar com o AWS CloudTrail

Este laboratório demonstra como:
- **monitorar ações na conta AWS**, 
- **investigar atividades suspeitas** e 
- **proteger recursos** utilizando o **AWS CloudTrail**, Amazon EC2, AWS CLI, grep e Amazon Athena.

Nesta atividade vamos criar uma trilha do **AWS CloudTrail** para auditar ações executadas na sua conta.  
Em seguida vamos investigar **quem modificou o site** da cafeteria **Café**, que está hospedado em uma instância **Amazon EC2** chamada **Café Web Server**.

As etapas do laboratório seguem um fluxo investigativo:

- **Tarefa 1:** Observar o site funcionando normalmente.  
- **Tarefa 2:** Criar uma trilha do CloudTrail e perceber que o site foi violado — alguém alterou o **Security Group**.  
- **Tarefa 3:** Analisar logs do CloudTrail usando vários métodos (Linux `grep`, AWS CLI).  
- **Tarefa 4:** Consultar logs no **Amazon Athena**.  
- **Desafio:** Identificar o hacker responsável.  
- **Tarefa 5:** Remover o acesso do usuário malicioso e reforçar a segurança da conta AWS.

> *O lab utiliza uma instância EC2 que hospeda o site da cafeteria Café, monitorada por uma trilha do AWS CloudTrail, armazenada em um bucket S3 e consultada via Athena.*
<img width="814" height="437" alt="image" src="https://github.com/user-attachments/assets/af57de0e-48bd-404c-b531-0d42937ccdf1" />

## 🎯 Objetivos do Lab

- Criar e configurar uma trilha do **AWS CloudTrail**
- Analisar logs do CloudTrail utilizando ferramentas diversas
- Importar logs do CloudTrail para o **Amazon Athena**
- Executar consultas SQL para investigar eventos
- Detectar ações suspeitas na conta AWS
- Resolver problemas de segurança na instância EC2 Linux e no ambiente da AWS

## 🧭 Cenário

A equipe de liderança da cafeteria **Café** — Martha e Frank — está preocupada porque o site foi **invadido recentemente**.  
Eles contam com você, no papel de **Sofia**, para:  
- Identificar **quem alterou** o sistema
- Descobrir **como** isso aconteceu
- Evitar que futuras invasões ocorram
Faythe, Frank, Martha e outros realizam alterações constantes no site — e às vezes sem rastreamento adequado. Agora, diante da invasão, eles questionam Sofia:

> *“Existe alguma forma de sabermos exatamente quem alterou o quê? E quando?”*

Sua missão é responder **sim** — utilizando o **AWS CloudTrail** para reconstruir a história e apontar o culpado.

## 🧭 Resumo da Missão

Você deve:
1. Ativar o CloudTrail  
2. Detectar alterações indevidas  
3. Analisar logs com múltiplas ferramentas  
4. Investigar a identidade do invasor  
5. Remover seu acesso  
6. Proteger o ambiente contra novos ataques

## Tarefa 1: Modificar um Grupo de Segurança e Observar o Site

### 1. Acessar o EC2
1. No console da AWS, abra **Services** (Serviços)  
2. Selecione **EC2**

### 2. Selecionar a Instância
1. Clique em **Instâncias**
2. Encontre e selecione a instância **Café Web Server (WebSecurityGroup)**

### 3. Modificar o Grupo de Segurança
1. Na guia **Segurança**, selecione o **Security Group** `sg-xxxxxxxxxx`
2. Vá até **Regras de entrada**
3. Observe que existe apenas uma regra permitindo **HTTP (porta 80)**

### 4. Adicionar Regra de SSH  
1. Clique em **Editar regras de entrada**
2. Escolha **Adicionar regra**
3. Configure conforme abaixo:
   - **Tipo:** SSH  
   - **Intervalo de portas:** 22  
   - **Origem:** *My IP*  
   
   ⚠️ **Importante:**  
   Verifique que a origem da regra mostra um **CIDR com /32**, permitindo acesso *somente* ao seu endereço IP.  
   Não deve aparecer **0.0.0.0/0**, que deixaria o SSH aberto para qualquer pessoa.

4. Clique em **Salvar regras**

### 5. Observar o Site da Cafeteria
1. Volte em **Instâncias**
2. Selecione **Café Web Server**
3. Na guia **Detalhes**, copie o valor de **Public IPv4 address**
4. Abra uma nova aba no navegador e acesse: `http://<WebServerIP>/cafe/` 
➡️ *Substitua `<WebServerIP>` pelo endereço copiado.*

## ☕ Resultado Esperado
O site deve carregar normalmente, exibindo imagens apropriadas para uma cafeteria e funcionando como esperado.

## Tarefa 2: Criar um Log do CloudTrail e Observar o Site Invadido
Nesta tarefa vamos **criar uma trilha do AWS CloudTrail** e, logo em seguida, perceber que o site da cafeteria **Café** foi comprometido.  
Esse comportamento faz parte do laboratório e permitirá investigar o que ocorreu usando os logs gerados.

### Tarefa 2.1: Criar um Log do CloudTrail

1. No **Console de Gerenciamento da AWS**, abra **Services (Serviços)**.

2. Acesse **Gerenciamento e Governança → CloudTrail**.  
   > Ignore a mensagem de acesso negado do AWS Organizations.

3. No painel esquerdo, selecione **Trails (Trilhas)**.  
   > Se o painel não aparecer, clique no ícone de **três linhas** no canto superior esquerdo.

4. Se aparecer o aviso _“The option to create an organization trail is not available…”_, apenas ignore.

5. Clique em **Create trail (Criar trilha)**.

### 📝 Configurando a trilha

Preencha os seguintes campos:
- **Nome da trilha:** `monitor`  
  ⚠️ *Obrigatório — o laboratório depende desse nome.*
- **S3 bucket:** escolha **Create a new S3 bucket**
- **Bucket e pasta de log:** insira `monitoring####`. Sendo `####` quatro dígitos aleatórios.  
- **AWS KMS alias:** insira suas iniciais seguidas de `-KMS`. Ex.: `ad-KMS`  
- Clique em **Próximo**.

### 📁 Configurar eventos de log
- Na página **Choose log events**, clique em **Próximo**.

### ✔️ Criar trilha
- Na página **Analisar e criar**, clique em **Create trail (Criar trilha)**.
- Confirme que a trilha aparece listada em **Trails (Trilhas)**.

### Tarefa 2.2: Observar o Site Violado
1. Retorne à aba do navegador onde o site da cafeteria está aberto.
2. Atualize a página.

⚠️ **Atenção:**  
- Pode levar até **1 minuto** para que o site seja invadido.  
- O navegador pode estar mantendo imagens em cache.  
Use: **Shift + Atualizar** para forçar o carregamento completo.

### 😱 O que você verá?
O site agora mostra uma **imagem indevida**, claramente colocada por um invasor.
Agora é sua missão descobrir **quem fez isso**.
E ainda bem que você ativou o CloudTrail — ele registrou tudo!

### 🕵️‍♂️ Investigando a Violação
1. Volte ao **Console da AWS** e acesse **EC2**.
2. Selecione a instância **Café Web Server**.
3. Analise os **Detalhes** e depois a guia **Segurança**.

### 🔎 Algo suspeito?
Ao abrir o **Security Group `sg-xxxxxxxxxx`**, na guia **Regras de entrada**, você verá:
- Sua regra SSH (porta 22) permitindo acesso somente ao seu IP (**/32**)  
- **UMA REGRA NOVA** liberando SSH para o mundo (**0.0.0.0/0**) ❗
Essa regra abriu a porta SSH publicamente — uma falha grave de segurança.

A pergunta agora é: 👉 Quem adicionou essa regra? 🤔
A resposta está nos **logs do CloudTrail**, que você acabou de configurar.  
O CloudTrail registrou quem fez a alteração e quando.

## Tarefa 3: Analisar os Logs do CloudTrail Usando `grep`
Nesta tarefa vamos utilizar o utilitário **grep** do Linux para analisar logs do AWS CloudTrail diretamente na instância EC2.  
O objetivo é identificar quem modificou o site da cafeteria Café.  

### Tarefa 3.1: Conectar-se à Instância EC2 Café Web Server via SSH
Você irá acessar a instância **Café Web Server** usando SSH.  
As instruções variam de acordo com o sistema operacional:  
- Usuários **Windows** → siga a **Tarefa 3.2 para Windows**  

### Tarefa 3.2 (Windows): Conectar via SSH  

1. No painel do laboratório, abra o menu suspenso **Detalhes**.
2. Clique em **Mostrar** para exibir a janela de **Credenciais**.
3. Clique em **Download PPK (labsuser.ppk)** e salve o arquivo.  
   *Normalmente ele será salvo na pasta Downloads.*
4. **Anote o endereço IP público (PublicIP)** da instância.
5. Feche o painel clicando no **X**.

💡 **Após conectar à instância**, você estará pronto para utilizar o comando `grep` para analisar os logs do CloudTrail, que foram enviados para o bucket S3 configurado na 
Tarefa 2.

### Tarefa 3.3: Baixar e Extrair os Logs do CloudTrail
Nesta etapa vamos **baixar os logs do CloudTrail** que foram enviados para o bucket S3 criado anteriormente e **extrair** esses arquivos para que possam ser analisados 
com `grep`.

#### 1. Verificar conexão com a instância
Certifique-se de que você está conectado via **SSH** à instância **Café Web Server** antes de continuar.

#### 2. Criar diretório para armazenar os logs
Crie um diretório local para salvar os logs do CloudTrail: `mkdir ctraillogs`  
Mude para o novo diretório: `cd ctraillogs`  

#### 3. Listar buckets S3 e localizar o bucket de logs
Liste os buckets S3 na conta com o comando: `aws s3 ls`  
Procure pelo bucket cujo nome começa com: `monitoring####`  

#### 4. Baixar os logs do CloudTrail
Execute o comando abaixo, substituindo `<monitoring####>` pelo nome real do bucket encontrado:
`aws s3 cp s3://<monitoring####>/ . --recursive`  
Se o comando estiver correto, você verá vários arquivos sendo baixados.

⚠️ Importante:
Se nenhum arquivo aparecer, significa que o CloudTrail ainda não enviou logs para o S3.  
O CloudTrail publica logs a **cada 5 minutos**, então aguarde um pouco e execute o comando novamente.  

📌 Não avance até que pelo menos um arquivo de log tenha sido baixado.  

#### 5. Navegar pelos diretórios até encontrar os logs
Os arquivos baixados estarão dentro de um caminho semelhante a: `AWSLogs/<account-number>/CloudTrail/<Region>/<yyyy>/<mm>/<dd>/`  
Use os comandos: `cd` / `ls`  
Ou pressione `Tab` para autocompletar diretórios até chegar aos arquivos `.json.gz.`  

#### 6. Extrair os arquivos de log
Os arquivos estarão compactados no formato GNU zip `(.json.gz)`.  
Extraia todos eles: gunzip *.gz`  
Liste novamente para verificar: `ls`  
Agora os arquivos devem aparecer somente como `.json`, totalmente descompactados e prontos para análise.  

Pronto! Agora podemos partir para a etapa de análise com `grep` para investigar quem invadiu o site da Cafeteria Café.  

### Tarefa 3.4: Analisar os Logs Usando `grep`
Nesta etapa vamos usar o utilitário **grep** do Linux para filtrar e entender os eventos registrados pelo **AWS CloudTrail**.  
O objetivo é identificar ações suspeitas relacionadas à invasão do site da cafeteria Café.

#### 📄 1. Visualizar a estrutura dos logs
Comece analisando um dos arquivos de log baixados.
Liste os arquivos e copie um dos nomes exibidos: `ls`  
Agora exiba o conteúdo bruto do arquivo: `cat <filename.json>`  
Os logs estão no formato JSON, mas sem formatação — difíceis de ler.  

#### 🧹 2. Deixar o JSON legível
Use o módulo de formatação do Python: `cat <filename.json> | python -m json.tool`  
Agora podemos ver a estrutura dos registros de log.  
Observe que cada registro contém os mesmos campos padrão, incluindo awsRegion, eventName, eventSource, eventTime, requestParameters, sourceIPAddress, userIdentity etc.  
O gráfico abaixo mostra um registro de log de exemplo:  

<img width="442" height="458" alt="image" src="https://github.com/user-attachments/assets/758f884f-d4ae-4e04-90d2-efa9ded3bc3f" />

Dessa forma veremos os campos estruturados, como:  
- awsRegion  
- eventName
- eventSource
- eventTime
- requestParameters
- sourceIPAddress
- userIdentity
- entre outros  
📌 Isso permite identificar rapidamente o tipo de evento registrado em cada log.  

