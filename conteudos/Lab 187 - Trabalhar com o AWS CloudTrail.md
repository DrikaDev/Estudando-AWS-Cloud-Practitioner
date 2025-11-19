## 🧪 Lab 187 - Trabalhar com o AWS CloudTrail

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

#### 🎯 3. Encontrar eventos relacionados ao servidor violado
Como o foco é descobrir ações feitas na instância Café Web Server, é útil filtrar registros onde o **sourceIPAddress** corresponde ao IP desse servidor.  
Defina o IP como variável: `ip=<WebServerIP>`  

Execute o seguinte comando: `for i in $(ls); do echo $i && cat $i | python -m json.tool | grep sourceIPAddress ; done`  

🤔 O que esse comando faz?  
- Percorre todos os arquivos do diretório atual (for i in $(ls)).  
- Exibe o nome de cada arquivo (echo $i).  
- Formata o conteúdo JSON (python -m json.tool).  
- Filtra somente linhas contendo sourceIPAddress.  
✔️ Você verá vários registros onde o IP do Café Web Server aparece.

#### 📝 4. Filtrar eventos por **eventName**
Agora filtre para descobrir quais ações foram realizadas:  
`for i in $(ls); do echo $i && cat $i | python -m json.tool | grep eventName ; done`  
Você verá muitos eventos:  
- Describe*  
- List*  
Esses são geralmente inofensivos.  
Mas também aparecerão eventos mais sensíveis, como:  
- Update*  
- AuthorizeSecurityGroupIngress  
- ModifyInstanceAttribute  
- etc.  
Estes podem indicar alterações reais na infraestrutura.  

#### 🔍 5. Analisar um log específico (opcional)
Se quiser investigar um evento suspeito, abra o arquivo no editor vi (ou outro): `vi <filename.json>`  
Procure pelo nome do evento: `/eventName`  
Analise os detalhes do registro.

💡 Próximo passo
Embora grep seja útil, existem ferramentas mais poderosas para investigar logs — a seguir você usará Amazon Athena, que permite consultas SQL diretamente nos logs do CloudTrail.

### Tarefa 3.5: Analisar os Logs Usando Comandos CloudTrail da AWS CLI
Nesta tarefa vamos utilizar diretamente a **AWS CLI** para consultar eventos registrados pelo **AWS CloudTrail**.  
Essa abordagem permite filtrar logs por atributos específicos, como tipo de evento, usuário, recursos modificados e muito mais.  

#### 📘 1. Consultar a documentação da AWS CLI
Acesse a página de referência da AWS CLI para CloudTrail e procure pelo comando: **`lookup-events`**  
Esse comando permite filtrar eventos usando até **8 atributos**, como:  

- Nome do evento (`EventName`)
- Tipo de recurso (`ResourceType`)
- Nome do usuário (`Username`)
- Chave de acesso (`AccessKeyId`)
- Entre outros

#### 🔐 2. Filtrar eventos de login no console
Role até a seção **Examples** da documentação e execute o comando para procurar eventos de login no console:  
```
aws cloudtrail lookup-events --lookup-attributes  
AttributeKey=EventName,AttributeValue=ConsoleLogin
```  
Resultado esperado:  
- Pode não haver nenhum login diferente do seu.
- Ou o único login registrado é o do usuário que você usou para acessar o console.
📌 Isso sugere que o invasor **não** usou o Console da AWS.

#### 🛡️ 3. Buscar alterações em grupos de segurança
Como o site foi violado e houve modificação no Security Group, procure por eventos relacionados a AWS::EC2::SecurityGroup:  
```
aws cloudtrail lookup-events --lookup-attributes  
AttributeKey=ResourceType,AttributeValue=AWS::EC2::SecurityGroup --output text
```  
Esse comando retorna todas as ações realizadas em grupos de segurança da conta.  
Contudo, os resultados podem ser muito extensos.  

#### 🎯 4. Encontrar o Security Group da instância Café Web Server
Para filtrar somente o Security Group que realmente importa, primeiro obtenha:  

🔹 Região onde a instância está rodando:  
`region=$(curl http://169.254.169.254/latest/dynamic/instance-identity/document | grep region | cut -d '"' -f4)`  

🔹 ID do Security Group associado ao Café Web Server:  
```
sgId=$(aws ec2 describe-instances \
  --filters "Name=tag:Name,Values='Cafe Web Server'" \
  --query 'Reservations[*].Instances[*].SecurityGroups[*].[GroupId]' \
  --region $region \
  --output text)
```

Exiba o valor encontrado: `echo $sgId`  
Agora você tem o ID exato do Security Group modificado pelo invasor.

#### 🕵️ 5. Filtrar os eventos usando o Security Group específico
Agora refine a consulta anterior procurando somente eventos que envolvem esse Security Group:  
```
aws cloudtrail lookup-events \
  --lookup-attributes AttributeKey=ResourceType,AttributeValue=AWS::EC2::SecurityGroup \
  --region $region --output text | grep $sgId
```

Esse comando revela:  
- Quem alterou o Security Group  
- Quando a alteração ocorreu  
- Qual ação foi executada  
- E outros detalhes importantes do evento registrado

#### 💭 6. Próximos passos
Você poderia continuar refinando consultas com a AWS CLI, mas a análise se tornaria trabalhosa.  
Por isso, a AWS e parceiros da APN oferecem ferramentas especializadas em análise de logs:  
🔗 https://aws.amazon.com/cloudtrail/partners/  
Contudo, no contexto deste laboratório, existe uma alternativa poderosa:  
👉 Usar o Amazon Athena para consultar os logs do CloudTrail usando SQL.  

## Tarefa 4 — Analisar os logs do CloudTrail usando Athena
Como visto anteriormente, encontrar informações específicas em grandes volumes de logs pode ser difícil.  
O **Amazon Athena** permite consultar arquivos armazenados no S3 usando SQL padrão, facilitando a análise dos logs do CloudTrail.

### 📌 Tarefa 4.1 — Criar a tabela do Athena

1. No **AWS Management Console** vá para **Services → CloudTrail**.  

2. No painel de navegação, selecione **Event history (Histórico de eventos)**.  
   > A interface de histórico permite filtros rápidos, mas neste exercício usaremos o Athena.

3. Clique em **Create Athena table (Criar tabela do Athena)**.

4. Em **Storage location (Local de armazenamento)**, selecione o bucket S3 que você criou para os logs do CloudTrail, por exemplo: `s3://monitoring####/`  
(Substitua `####` pelos quatro dígitos do seu bucket.)  

5. Analise a declaração **CREATE TABLE** gerada pelo console:
- Ela cria colunas correspondentes aos pares nome/valor do JSON do CloudTrail.
- Observe a cláusula `LOCATION` no final — ela aponta para os dados já presentes no S3.

6. Consulte a documentação (opcional):
- CloudTrail event reference: https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference.html  
- Athena + CloudTrail: https://docs.aws.amazon.com/athena/latest/ug/cloudtrail-logs.html

7. Após revisar, selecione **Create table**.  
A tabela será criada com um nome padrão que incorpora o nome do bucket (por exemplo `cloudtrail_logs_monitoring####`).

8. No Console, abra **Services → Analytics → Athena**.

### 🧠 Tarefa 4.2 — Analisar logs usando o Athena

1. No **Athena Query Editor**, feche o tutorial se aparecer e localize a tabela criada no painel esquerdo (ex.: `cloudtrail_logs_monitoring####`).

2. Expanda a tabela para ver as colunas. Observações:
- `useridentity` é um **struct** (contém subcampos como `userName`).
- `resources` é uma **array**.
- Cada campo do JSON virou uma coluna no esquema.

3. Configure o local de resultados das consultas:
- No canto superior direito, clique em **Settings → Manage**.
- Em **Location of query result**, defina:
  ```
  s3://monitoring####/results/
  ```
  (substitua `####` pelo seu bucket)
- Clique em **Save**.

### ▶ Consultas úteis

#### Consulta básica — ver primeiras linhas
```sql
SELECT *
FROM cloudtrail_logs_monitoring####
LIMIT 5;
```
Retorna 5 registros completos, útil para entender o esquema e as colunas disponíveis.  

Consulta focada — colunas mais relevantes  
```
SELECT useridentity.userName, eventtime, eventsource, eventname, requestparameters
FROM cloudtrail_logs_monitoring####
LIMIT 30;
```
- Retorna `userName`, `eventtime`, `eventsource`, `eventname` e `requestparameters`.  
- Use essa consulta para identificar ações suspeitas (ex.: alterações em security groups).  

### ✅ Dicas para investigação
- Remova o LIMIT para consultar todo o conjunto de logs quando necessário.  
- Filtre por eventos do EC2: `WHERE eventsource = 'ec2.amazonaws.com'`  
- Procure por eventnames relacionados a security groups, por exemplo `AuthorizeSecurityGroupIngress` ou `RevokeSecurityGroupIngress`:
`WHERE eventname LIKE '%SecurityGroup%' OR eventname LIKE '%AuthorizeSecurityGroupIngress%'`
- Use `from_iso8601_timestamp(eventtime)` para comparar datas/horários em consultas (por exemplo, últimos 1 dia).  

### 🎯 Objetivo desta tarefa  
Com o Athena você deve conseguir identificar:  
- Quem (useridentity.userName) modificou o Security Group da instância Café Web Server;  
- Quando (eventtime) a alteração ocorreu;  
- Quais parâmetros foram enviados (requestparameters) — por exemplo, o IP adicionado;  
- Se a ação foi feita via Console ou programaticamente (analise userIdentity e o tipo de evento).  
