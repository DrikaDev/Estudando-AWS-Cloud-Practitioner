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

Faythe, Frank, Martha e outros realizam alterações constantes no site — e às vezes sem rastreamento adequado.  
Agora, diante da invasão, eles questionam Sofia:  

> *“Existe alguma forma de sabermos exatamente quem alterou o quê? E quando?”*

Sua missão é responder **"sim"**, utilizando o **AWS CloudTrail** para reconstruir a história e apontar o culpado.  

## 🧭 Resumo da Missão

Você deve:
1. Ativar o CloudTrail  
2. Detectar alterações indevidas  
3. Analisar logs com múltiplas ferramentas  
4. Investigar a identidade do invasor  
5. Remover seu acesso  
6. Proteger o ambiente contra novos ataques

## Tarefa 1: Modificar um Grupo de Segurança e Observar o Site

1. No console da AWS, abra **Services** (Serviços)  

2. Selecione **EC2**

3. Clique **Instâncias**

4. Encontre e selecione a instância **Café Web Server (WebSecurityGroup)**

<img width="1421" height="223" alt="image" src="https://github.com/user-attachments/assets/5719f026-6bca-4613-b993-04e581deb5a7" />

5. Na guia **Security / Segurança**, selecione o **Security Group** `sg-xxxxxxxxxx`

<img width="1196" height="97" alt="image" src="https://github.com/user-attachments/assets/0f4c7b87-3306-488e-9a2c-04faea39f563" />

6. Vá até **Inbound rules / Regras de entrada**

7. Observe que existe apenas uma regra permitindo **HTTP (porta 80)**

<img width="1198" height="133" alt="image" src="https://github.com/user-attachments/assets/115541bd-96d2-4115-ad07-d414acfa5870" />

8. Adicione Regra de SSH  

9. Clique no link azul **WebSecurityGroup** do **Inbound rules**

10. Vai abrir outra guia, selecione o **Web Server Security Group** 

11. Na guia **Inbound rules**, clique em **Edit inbound rules / Editar regras de entrada**

<img width="1204" height="262" alt="image" src="https://github.com/user-attachments/assets/31f4a1b6-04a3-4067-b9ed-69155ae78dfc" />

12. Escolha **Add rule / Adicionar regra**

13. Configure conforme abaixo:

   - **Type / Tipo:** SSH  
   - **Port range / Intervalo de portas:** 22  
   - **Source / Origem:** *My IP*  

<img width="1425" height="387" alt="image" src="https://github.com/user-attachments/assets/c9315235-a2c1-4714-9dbc-4125e30746fb" />

⚠️ **Importante:**  
Verifique que a origem da regra mostra um **CIDR com /32**, permitindo acesso *somente* ao seu endereço IP.  
Não deve aparecer **0.0.0.0/0**, que deixaria o SSH aberto para qualquer pessoa.

14. Clique em **Save rules / Salvar regras**

15. Observe o Site da Cafeteria

16. Volte em **Instâncias**

17. Selecione **Café Web Server**

18. Na guia **Detalhes**, copie o valor de **Public IPv4 address**

<img width="1214" height="399" alt="image" src="https://github.com/user-attachments/assets/db06ed1c-d310-4c71-a61e-ece09c576217" />

19. Abra uma nova aba no navegador e acesse: `http://<WebServerIP>/cafe/` 
➡️ *Substitua `<WebServerIP>` pelo endereço copiado.*

<img width="417" height="48" alt="image" src="https://github.com/user-attachments/assets/a7a04ba4-f776-4ef4-91d0-52a25f4d6a4f" />

20. Resultado Esperado:
O site deve carregar normalmente, exibindo imagens apropriadas para uma cafeteria e funcionando como esperado.  

<img width="1430" height="649" alt="image" src="https://github.com/user-attachments/assets/3198b619-0aa9-4620-b624-6c4201cf3f45" />

## Tarefa 2: Criar um Log do CloudTrail e Observar o Site Invadido

Nesta tarefa vamos **criar uma trilha do AWS CloudTrail** e, logo em seguida, perceber que o site da cafeteria **Café** foi comprometido.  
Esse comportamento faz parte do laboratório e permitirá investigar o que ocorreu usando os logs gerados.

### Tarefa 2.1: Criar um Log do CloudTrail

1. No **Console de Gerenciamento da AWS**, acesse CloudTrail**.  
   
2. No painel esquerdo, selecione **Trails (Trilhas)**.  
   > Se o painel não aparecer, clique no ícone de **três linhas** no canto superior esquerdo.

<img width="1432" height="334" alt="image" src="https://github.com/user-attachments/assets/633481b0-3ea3-4c2d-a0cf-d7d2ae153eb9" />

3. Clique em **Create trail (Criar trilha)**.

4. Preencha os seguintes campos:
- **Nome da trilha:** `monitor`  
  ⚠️ *Obrigatório — o laboratório depende desse nome.*

<img width="1421" height="252" alt="image" src="https://github.com/user-attachments/assets/a6afe27c-274c-4afa-8b4f-447ed581ef78" />

- **S3 bucket:** escolha **Create a new S3 bucket**
- **Bucket e pasta de log:** insira `monitoring####`. Sendo `####` quatro dígitos aleatórios.  
- **AWS KMS alias:** insira suas iniciais seguidas de `-KMS`. Ex.: `ad-KMS`  

<img width="1136" height="417" alt="image" src="https://github.com/user-attachments/assets/b7e5d40a-edeb-4fde-92e8-a3f26d40bc96" />

- Clique em **Próximo**.  

5. Configurar eventos de log
Na página **Choose log events / Escolher eventos de log**, clique em **Próximo**.

6. Criar trilha
Na página **Analisar e criar**, clique em **Create trail / Criar trilha**.  
Confirme que a trilha aparece listada em **Trails / Trilhas**.  

<img width="1414" height="359" alt="image" src="https://github.com/user-attachments/assets/56794795-57ff-4cde-93db-c12b79a4e264" />

### Tarefa 2.2: Observar o Site Violado

1. Retorne à aba do navegador onde o site da cafeteria está aberto.

2. Atualize a página.

⚠️ **Atenção:**  
- Pode levar até **1 minuto** para que o site seja invadido.  
- O navegador pode estar mantendo imagens em cache.  
Use: **Shift + Atualizar** para forçar o carregamento completo.

<img width="1409" height="800" alt="image" src="https://github.com/user-attachments/assets/12b23315-f3c5-41a7-b85a-2472886ca771" />

### 😱 O que você viu?
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

<img width="1190" height="197" alt="image" src="https://github.com/user-attachments/assets/b7b1fd64-5774-4655-b44c-aa0cf1de341a" />

Essa regra abriu a porta SSH publicamente — uma falha grave de segurança.

A pergunta agora é: 👉 Quem adicionou essa regra? 🤔
A resposta está nos **logs do CloudTrail**, que você acabou de configurar.  
O CloudTrail registrou quem fez a alteração e quando.

## Tarefa 3: Analisar os Logs do CloudTrail Usando `grep`

Nesta tarefa vamos utilizar o utilitário **grep** do Linux para analisar logs do AWS CloudTrail diretamente na instância EC2.  
O objetivo é identificar quem modificou o site da cafeteria Café.  

### Tarefa 3.1: Conectar-se à Instância EC2 Café Web Server via SSH

Vamos acessar a instância **Café Web Server** usando SSH.  

1. No painel do laboratório, abra o menu suspenso **Detalhes**.
2. Clique em **Mostrar** para exibir a janela de **Credenciais**.
3. Clique em **Download PEM (labsuser.pem)** e salve o arquivo.  
   *Normalmente ele será salvo na pasta Downloads.*
4. **Anote o endereço IP público (PublicIP)** da instância.
5. Feche o painel clicando no **X**.

💡 **Após conectar à instância**, você estará pronto para utilizar o comando `grep` para analisar os logs do CloudTrail, que foram enviados para o bucket S3 configurado na Tarefa 2.  

<img width="1162" height="434" alt="image" src="https://github.com/user-attachments/assets/ef2555fd-60fa-4e6e-82ab-faaf5dc67bef" />

### Tarefa 3.2: Baixar e Extrair os Logs do CloudTrail

Nesta etapa vamos **baixar os logs do CloudTrail** que foram enviados para o bucket S3 criado anteriormente e **extrair** esses arquivos para que possam ser analisados 
com `grep`.

#### 1. Verificar conexão com a instância
Certifique-se de que você está conectado via **SSH** à instância **Café Web Server** com o comando `pwd` antes de continuar.  

<img width="267" height="70" alt="image" src="https://github.com/user-attachments/assets/4d886714-4a0a-4332-b165-42c352c1e67d" />

#### 2. Criar diretório para armazenar os logs
Crie um diretório local para salvar os logs do CloudTrail: `mkdir ctraillogs`  
Mude para o novo diretório: `cd ctraillogs`  

#### 3. Listar buckets S3 e localizar o bucket de logs
Liste os buckets S3 na conta com o comando: `aws s3 ls`  
Procure pelo bucket cujo nome começa com: `monitoring####`  

<img width="406" height="121" alt="image" src="https://github.com/user-attachments/assets/94dcf299-03fc-42ae-b86b-9b5a838cf221" />

#### 4. Baixar os logs do CloudTrail
Execute o comando abaixo, substituindo `monitoring####` pelo nome real do bucket encontrado:
`aws s3 cp s3://monitoring####/ . --recursive`  
Se o comando estiver correto, você verá vários arquivos sendo baixados.

⚠️ Importante:
Se nenhum arquivo aparecer, significa que o CloudTrail ainda não enviou logs para o S3.  
O CloudTrail publica logs a **cada 5 minutos**, então aguarde um pouco e execute o comando novamente.  

📌 Não avance até que pelo menos um arquivo de log tenha sido baixado.  

#### 5. Navegar pelos diretórios até encontrar os logs
Os arquivos baixados estarão dentro de um caminho semelhante a: `AWSLogs/<account-number>/CloudTrail/<Region>/<yyyy>/<mm>/<dd>/`  
Use os comandos: `cd` / `ls`  
Ou pressione `Tab` para autocompletar diretórios até chegar aos arquivos `.json.gz.`  

<img width="1350" height="534" alt="image" src="https://github.com/user-attachments/assets/c133d8e4-34a8-4e0a-a334-0af5bc6d883a" />

> Lembrando que a região da Cafeteria, nesse caso, é a Oregon us-west-2  
> Então us-east-1 é a região do invasor!

<img width="303" height="185" alt="image" src="https://github.com/user-attachments/assets/3ea0bb1d-c54d-4444-9bfe-9b061f4345ab" />

#### 6. Extrair os arquivos de log
Os arquivos estarão compactados no formato GNU zip `(.json.gz)`.  

> Arquivos no formato GNU zip (.gz) são arquivos comprimidos usando o utilitário gzip, muito comum em sistemas Linux/Unix.

Extraia todos eles: `gunzip *.gz`  
Liste novamente para verificar: `ls`  
Agora os arquivos devem aparecer somente como `.json`, totalmente descompactados e prontos para análise.  

<img width="1292" height="121" alt="image" src="https://github.com/user-attachments/assets/0e443c01-69c4-431a-8cb2-9d65bee5da10" />

Pronto! Agora podemos partir para a etapa de análise com `grep` para investigar quem invadiu o site da Cafeteria Café.  

### Tarefa 3.4: Analisar os Logs Usando `grep`

Nesta etapa vamos usar o utilitário **grep** do Linux para filtrar e entender os eventos registrados pelo **AWS CloudTrail**.  
O objetivo é identificar ações suspeitas relacionadas à invasão do site da cafeteria Café.  

#### 📄 1. Visualizar a estrutura dos logs
Comece analisando um dos arquivos de log baixados.  
Liste os arquivos e copie um dos nomes exibidos: `ls`  
Agora exiba o conteúdo bruto do arquivo: `cat <filename.json>` - cole o nome do arquivo no lugar do "filename"  
Os logs estão no formato JSON, mas sem formatação — difíceis de ler.  

<img width="1414" height="386" alt="image" src="https://github.com/user-attachments/assets/bbcec400-51e7-43d3-867d-f8b68507cce2" />

#### 🧹 2. Deixar o JSON legível
Use o módulo de formatação do Python: `cat <filename.json> | python -m json.tool`  
Agora podemos ver a estrutura dos registros de log:  

<img width="1423" height="809" alt="image" src="https://github.com/user-attachments/assets/078a3646-7214-47c3-b1ca-88d7c55fa95c" />

Observe que cada registro contém os mesmos campos padrão, incluindo *awsRegion*, *eventCategory*, *eventID*, *eventName*, *eventSource*, *eventTime* etc.  

📌 Isso permite identificar rapidamente o "tipo de evento" registrado em cada log.  

#### 🎯 3. Encontrar eventos relacionados ao servidor violado
Como o foco é descobrir ações feitas na instância Café Web Server, é útil filtrar registros onde o **sourceIPAddress** corresponde ao IP desse servidor.  
Defina o IP como variável: `ip=<WebServerIP>`  

<img width="382" height="48" alt="image" src="https://github.com/user-attachments/assets/cad5858a-0ceb-4da7-99df-b8aee2b8da4a" />

Execute o seguinte comando: `for i in $(ls); do echo $i && cat $i | python -m json.tool | grep sourceIPAddress ; done`  

<img width="1034" height="308" alt="image" src="https://github.com/user-attachments/assets/99cb2c55-b132-4aa0-896c-f84908b90dc5" />

🤔 O que esse comando faz?  
- Percorre todos os arquivos do diretório atual (for i in $(ls)).  
- Exibe o nome de cada arquivo (echo $i).  
- Formata o conteúdo JSON (python -m json.tool).  
- Filtra somente linhas contendo sourceIPAddress.  
✔️ Você verá vários registros onde o IP do Café Web Server aparece.  

#### 📝 4. Filtrar eventos por **eventName**
Agora filtre para descobrir quais ações foram realizadas:  
`for i in $(ls); do echo $i && cat $i | python -m json.tool | grep eventName ; done`  

<img width="981" height="312" alt="image" src="https://github.com/user-attachments/assets/91e70a6b-d465-4926-949c-da8eaa834ac8" />

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
Acesse a página de referência da AWS CLI para CloudTrail `https://docs.aws.amazon.com/cli/latest/reference/cloudtrail/` e procure pelo comando: **`lookup-events`**  

<img width="1192" height="141" alt="image" src="https://github.com/user-attachments/assets/c5133c22-b8a4-441d-92e0-6dcbc590ee0f" />

Esse comando permite filtrar eventos usando até **8 atributos**, como:  

<img width="142" height="218" alt="image" src="https://github.com/user-attachments/assets/e8feada0-1c77-4414-a540-7b386d826e89" />

#### 🔐 2. Filtrar eventos de login no console

Role até a seção **Examples** da documentação e execute o comando para procurar eventos de login no console:  
`aws cloudtrail lookup-events --lookup-attributes AttributeKey=EventName,AttributeValue=ConsoleLogin`  

<img width="720" height="154" alt="image" src="https://github.com/user-attachments/assets/7a620e8b-2d34-4094-9b0a-5358b7dfee00" />

Resultado esperado:  

<img width="1126" height="102" alt="image" src="https://github.com/user-attachments/assets/6431947a-5b72-4bec-a279-3dc0d69a8173" />

- Pode não haver nenhum login diferente do seu.  
- Ou o único login registrado é o do usuário que você usou para acessar o console.  
📌 Isso sugere que o invasor **não** usou o Console da AWS.  

#### 🛡️ 3. Buscar alterações em grupos de segurança
Como o site foi violado e houve modificação no Security Group, procure por eventos relacionados a AWS::EC2::SecurityGroup:  
`aws cloudtrail lookup-events --lookup-attributes AttributeKey=ResourceType,AttributeValue=AWS::EC2::SecurityGroup --output text`  

Esse comando retorna todas as ações realizadas em grupos de segurança da conta.  
Contudo, os resultados podem ser muito extensos.  

<img width="1413" height="459" alt="image" src="https://github.com/user-attachments/assets/46c1dc2b-a758-4481-89d8-221c78525454" />

#### 🎯 4. Encontrar o Security Group da instância Café Web Server
Para filtrar somente o Security Group que realmente importa, primeiro obtenha:  

🔹 Região onde a instância está rodando:  
`region=$(curl http://169.254.169.254/latest/dynamic/instance-identity/document | grep region | cut -d '"' -f4)`  

<img width="1228" height="101" alt="image" src="https://github.com/user-attachments/assets/9348b77f-dfb5-4b89-95db-5525db49e3b3" />

O que esse comando faz?
- **curl** acessa o *Instance Metadata Service (IMDS)* da instância EC2.
- O serviço retorna um documento **JSON** com várias informações sobre a instância, incluindo a **região**.
- **grep** localiza a linha onde aparece o campo `"region"`.
- **cut** extrai apenas o valor da região, como `us-east-1`.
- O resultado final é armazenado na variável **`region`**.

Sobre a saída do curl
O bloco que aparece com: `% Total % Received % Xferd ...` é apenas a **barra de progresso do curl**, totalmente normal e esperada.  

🔹 ID do Security Group associado ao Café Web Server:  
`sgId=$(aws ec2 describe-instances \ --filters "Name=tag:Name,Values='Cafe Web Server'" \ --query 'Reservations[*].Instances[*].SecurityGroups[*].[GroupId]' \ --region $region \ --output text)`

Exiba o valor encontrado: `echo $sgId`  
Agora você tem o ID exato do Security Group modificado pelo invasor.

<img width="1417" height="101" alt="image" src="https://github.com/user-attachments/assets/dd572e71-c395-4433-862e-0c2f4a3e4dff" />

#### 🕵️ 5. Filtrar os eventos usando o Security Group específico
Agora refine a consulta anterior procurando somente eventos que envolvem esse Security Group:  
`aws cloudtrail lookup-events \ --lookup-attributes AttributeKey=ResourceType,AttributeValue=AWS::EC2::SecurityGroup \ --region $region \ --output text | grep $sgId`

<img width="1413" height="303" alt="image" src="https://github.com/user-attachments/assets/0a2a5ebe-cbe9-4956-a217-5bd599b5789a" />

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

1. No **AWS Management Console** vá para ***CloudTrail**.  

2. No painel de navegação, selecione **Event history / Histórico de eventos**.  
   > A interface de histórico permite filtros rápidos, mas neste exercício usaremos o Athena.

3. Clique em **Create Athena table / Criar tabela do Athena**.

<img width="1415" height="165" alt="image" src="https://github.com/user-attachments/assets/a7e2ad77-4f17-464f-bcc7-b21ca85313dd" />

4. Em **Storage location (Local de armazenamento)**, selecione o bucket S3 que você criou para os logs do CloudTrail, por exemplo: `s3://monitoring####/`  

<img width="747" height="191" alt="image" src="https://github.com/user-attachments/assets/2a00cc28-5173-4096-bde4-792c6d30f65d" />

5. Analise a declaração **CREATE EXTERNAL TABLE** gerada pelo console:
- Ela cria colunas correspondentes aos pares nome/valor do JSON do CloudTrail.  
<img width="712" height="259" alt="image" src="https://github.com/user-attachments/assets/3f1aaa9c-73df-46a7-83d6-c3dc9edbbbb4" />

- Observe a cláusula `LOCATION` no final — ela aponta para os dados já presentes no S3.  
<img width="713" height="18" alt="image" src="https://github.com/user-attachments/assets/048e98e6-feb6-4c49-b85c-d96842a0fae5" />

6. Consulte a documentação (opcional):
- CloudTrail event reference: https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference.html  
- Athena + CloudTrail: https://docs.aws.amazon.com/athena/latest/ug/cloudtrail-logs.html

7. Após revisar, selecione **Create table / Criar tabela**.  
A tabela será criada com um nome padrão que incorpora o nome do bucket (por exemplo `cloudtrail_logs_monitoring####`).
<img width="1193" height="68" alt="image" src="https://github.com/user-attachments/assets/d033e5fc-0903-44ba-820a-ab6f935c984c" />

### 🧠 Tarefa 4.2 — Analisar logs usando o Athena

1. No Console, abra **Athena**.

2. No **Query Editor** - menu a esquerda, feche o tutorial se aparecer e localize a tabela criada no painel esquerdo (ex.: `cloudtrail_logs_monitoring####`).

<img width="319" height="103" alt="image" src="https://github.com/user-attachments/assets/78baccc8-9212-4512-b3f8-181d883046e2" />

3. Expanda a tabela para ver as colunas.
   Observações:
- `useridentity` é um **struct** (contém subcampos como `userName`).
<img width="319" height="259" alt="image" src="https://github.com/user-attachments/assets/25d401b7-d365-4b3a-8224-42408b3c83be" />

- `resources` é uma **array**.
<img width="324" height="45" alt="image" src="https://github.com/user-attachments/assets/60bbe6da-f7fd-4390-8366-0691b886b1ed" />

- Cada campo do JSON virou uma coluna no esquema.

4. Configure o local de resultados das consultas:
- No canto superior direito, clique em **Settings → Manage settings**.
- Em **Location of query result**, defina: `s3://monitoring####/results/`  
  (substitua `####` pelo seu bucket)
- Clique em **Save**.
<img width="1416" height="206" alt="image" src="https://github.com/user-attachments/assets/76acacd8-c2f1-44e0-aca9-6435350822f7" />

- Selecione a guia **Editor** e cole a consulta SQL a seguir no painel Query 1 (Consulta 1).
  Substitua #### pelos números em sua tabela real e escolha Run (Executar).

```sql
SELECT *
FROM cloudtrail_logs_monitoring####
LIMIT 5;
```
<img width="1407" height="510" alt="image" src="https://github.com/user-attachments/assets/a7188be6-32a5-46a7-88c2-7ec1a2b480e3" />

Essa consulta retorna 5 registros completos, útil para entender o esquema e as colunas disponíveis.  

<img width="1068" height="398" alt="image" src="https://github.com/user-attachments/assets/4309a0f3-3b2a-402d-80c6-c317f90d47d9" />

Observe o conjunto de resultados (role para a direita no painel **Results / Resultados** para ver dados adicionais da coluna).  
Concentre-se nas colunas **useridentity**, **eventtime**, **eventsource**, **eventname** e **requestparameters**, que contêm as informações mais valiosas para ajudar a encontrar a origem do invasor.  

A coluna **useridentity** tem muitos detalhes que dificultam a leitura.  
Agora você retornará apenas o nome de usuário dessa coluna.  
  
Consulta focada — colunas mais relevantes  
```
SELECT useridentity.userName, eventtime, eventsource, eventname, requestparameters
FROM cloudtrail_logs_monitoring####
LIMIT 30;
```
<img width="1067" height="413" alt="image" src="https://github.com/user-attachments/assets/aff23c86-1ab6-42c7-8857-7e003d2a0774" />

- Retorna `userName`, `eventtime`, `eventsource`, `eventname` e `requestparameters`.  
- Use essa consulta para identificar ações suspeitas (ex.: alterações em security groups).  

<img width="1064" height="379" alt="image" src="https://github.com/user-attachments/assets/f44a1e2e-f604-4148-b491-2862c78d36d5" />

### Desafio: Identificar o Hacker

Nesta etapa da atividade, o objetivo é descobrir **qual registro de log** contém as informações essenciais sobre quem invadiu o site.  
Não há etapas prontas — você deverá **experimentar diferentes consultas SQL** até localizar a evidência correta.  

---

### **🧩 Dica 1 — Observe o que já foi retornado**
Analise os dados exibidos pelo último comando executado.  
Mesmo que não seja o log que você procura, eles mostram:

- Quais colunas existem  
- Que tipo de dados cada coluna armazena  
- Como você pode filtrar melhor

👉 Você pode criar novas abas de consulta selecionando o ícone **“+”** ao lado de *New query 1*.  
Assim, preserva consultas antigas sem apagá-las.

### **🧩 Dica 2 — Filtre por eventos do EC2**
Como o problema envolve modificação de **Security Group**, foque no Amazon EC2.

Use cláusulas `WHERE`, por exemplo: `WHERE eventsource = 'ec2.amazonaws.com'`  

### **🧩 Dica 3 — Remova o LIMIT**
Para garantir que está consultando todos os logs, remova a cláusula LIMIT da consulta.

### **🧩 Dica 4 — Filtre por eventname**
Examine os tipos de dados capturados em eventname.
Você pode buscar apenas eventos relacionados a Security:  
`WHERE eventsource = 'ec2.amazonaws.com'
  AND eventname LIKE '%Security%'`

SQL permite combinações com múltiplas condições AND.

### **🧩 Dica 5 — Refine ainda mais**
Depois de isolar os eventos de segurança, veja:
Algum eventname parece suspeito?
Algum indica mudança em regras de Security Group?
Ajuste sua consulta para procurar um eventname específico, conforme necessário.

### **🧩 Dica 6 — Consulta poderosa para descobrir quem estava ativo**
Se ainda não encontrou o log da abertura da porta 22 para o mundo, execute esta consulta geral:
```
SELECT DISTINCT useridentity.userName, eventName, eventSource 
FROM cloudtrail_logs_monitoring#### 
WHERE from_iso8601_timestamp(eventtime) > date_add('day', -1, now()) 
ORDER BY eventSource;
```
Essa consulta mostra:  
- Todos os usuários ativos no último dia  
- As ações distintas executadas por cada um  

🎯 Objetivos do Desafio  
Você terá concluído com sucesso quando identificar:  
- O nome do usuário AWS que modificou o grupo de segurança do Café Web Server  
- A hora exata da violação  
- O endereço IP utilizado (copie esse valor para referência futura)  
- O método de acesso usado para realizar a invasão: Console? / Programático (CLI / SDK)?  

<img width="1065" height="524" alt="image" src="https://github.com/user-attachments/assets/dcd943f7-b4d3-4d50-94e7-23b563dfce6f" />

🎉 Parabéns! Você descobriu com êxito a identidade do hacker.

## Tarefa 5 — Analisar ainda mais a invasão e melhorar a segurança
Nesta última etapa, vamos reforçar a segurança da instância comprometida e da sua conta AWS, identificando usuários suspeitos no sistema operacional e removendo acessos indevidos.  

### 🔍 Tarefa 5.1 — Verificar os usuários do sistema operacional

1. No terminal conectado via SSH à instância do servidor web, verifique quem fez login recentemente no sistema operacional: `sudo aureport --auth`  
>💡Observação:  
> Há evidências de que um usuário diferente do ec2-user acessou a instância.  
> O usuário suspeito é **chaos-user**.  

<img width="1007" height="305" alt="image" src="https://github.com/user-attachments/assets/a44541ee-8d23-4b3a-96a5-7585594dae9f" />

2. Verifique quem está conectado no momento.  
Execute: `who`  
Você verá que chaos-user ainda está conectado.  
É necessário removê-lo imediatamente!  

<img width="825" height="142" alt="image" src="https://github.com/user-attachments/assets/e8b4afae-1c03-442d-9fda-03e760f62343" />

3. Remova o usuário chaos-user.  
Tente remover o usuário: `sudo userdel -r chaos-user`  
Esse comando falha porque o usuário ainda possui uma sessão ativa.  
O terminal retorna o número do processo associado ao login dele.  

<img width="539" height="65" alt="image" src="https://github.com/user-attachments/assets/e9314e59-5644-4006-8906-099dca3676fc" />

4. Finalize o processo ativo do usuário.  
Substitua *ProcNum* pelo número do processo retornado acima e execute: `sudo kill -9 ProcNum`  
Depois, confirme novamente quem está conectado: `who`  
Agora somente você (ec2-user) deve estar conectado.

<img width="484" height="118" alt="image" src="https://github.com/user-attachments/assets/38ba96ab-2eae-4c61-b1d2-dbba86b3fcfd" />

5. Exclua o usuário definitivamente.  
Com o processo encerrado, execute novamente: `sudo userdel -r chaos-user`  
Dessa vez o comando deve funcionar corretamente.

<img width="542" height="42" alt="image" src="https://github.com/user-attachments/assets/190666cc-75bb-4329-9a59-de0d09fcfca8" />

6. Verifique se há outros usuários suspeitos no sistema operacional
Execute: `sudo cat /etc/passwd | grep -v nologin`  
Esse comando mostra apenas usuários com login permitido (filtrando contas do sistema).  
Usuários normais do Amazon Linux que devem aparecer: `root`, `sync`, `shutdown` e `halt`  
Se somente eles aparecerem (além de ec2-user), significa que não há mais usuários suspeitos.

<img width="601" height="138" alt="image" src="https://github.com/user-attachments/assets/54e69efc-4c15-4f6e-8e24-b90f51562790" />

### 🔐 Tarefa 5.2 — Atualizar a segurança SSH
Agora que o usuário malicioso foi removido, é hora de identificar **como ele conseguiu acesso à instância via SSH** e corrigir a vulnerabilidade.

#### 🔎 Verificar configurações do SSH
1. Liste as informações do arquivo de configuração do SSH: `sudo ls -l /etc/ssh/sshd_config`  

<img width="578" height="63" alt="image" src="https://github.com/user-attachments/assets/abf898a2-9561-4d83-89e5-090c13893087" />

O que esse resultado significa?  
- `-` → Arquivo comum (não é diretório)  
- **`rw-------`** → Apenas o **root** pode ler e escrever no arquivo (permissões corretas e seguras).
- **`root root`** → O arquivo pertence ao usuário e grupo **root**.
- - Usuário root: **rw**  
  - Grupo: **---**  
  - Outros: **---**

✔️ Isso é **correto e seguro** — o arquivo `sshd_config` deve realmente ser acessível apenas pelo root, pois controla a segurança do SSH.

- **Tamanho:** 3957 bytes.
- **Última modificação:** 20 de novembro. 👉🏻 O arquivo foi modificado hoje, o que é um grande alerta de segurança.  
- **Caminho:** `/etc/ssh/sshd_config`, arquivo de configuração do servidor SSH.

2. Edite o arquivo de configuração do SSH.
Edite o arquivo usando o editor VI: `sudo vi /etc/ssh/sshd_config`  
Dentro do VI, ative a visualização de números de linha: `:set number`  
⚠️ Problema identificado (linha 61)
A linha **PasswordAuthentication yes** está habilitando autenticação por senha, o que não é recomendado.  
Isso permite que qualquer pessoa com nome de usuário e senha válidos tente acesso via SSH.  

<img width="276" height="59" alt="image" src="https://github.com/user-attachments/assets/5b8f94a9-82ea-4a66-aa03-4276f31031dd" />

3. Corrija a configuração.
Navegue até a linha: `PasswordAuthentication yes`  
Entre no modo de edição pressionando: `a`  
Comente a linha, adicionando # no início: `#PasswordAuthentication yes`  
Vá para a linha seguinte e descomente: `#PasswordAuthentication no` deixando assim: `PasswordAuthentication no`  
<img width="281" height="59" alt="image" src="https://github.com/user-attachments/assets/6d865872-816b-4c58-80a9-32fef6f93ba8" />

Saia do modo de edição: `Esc`  
Digite `:wq` para salvar e saia com Enter

4. Reinicie o serviço SSH
Para aplicar as alterações: `sudo service sshd restart`  
<img width="477" height="59" alt="image" src="https://github.com/user-attachments/assets/33fdc96e-1378-4189-97e5-25ca0283e37f" />
Esse resultado significa que o comando service foi encaminhado internamente para o systemctl, que é o sistema moderno de gerenciamento de serviços no Linux.  
E como nenhum erro apareceu, isso confirma que:
✔️ O serviço sshd reiniciou com sucesso
✔️ As alterações no sshd_config foram aplicadas corretamente

5. Remover regra insegura no Security Group
No console da AWS:  
Acesse EC2 → Security Groups  
Selecione o SG da instância Café Web Server  

<img width="1432" height="320" alt="image" src="https://github.com/user-attachments/assets/6de51d5f-a1ae-4ba3-9893-e5214ef8dd3b" />

Vá em **Inbound rules / Regras de entrada** → Editar  

<img width="1200" height="317" alt="image" src="https://github.com/user-attachments/assets/f6b6066b-f3dc-430a-99e4-f2cdac877daa" />

Delete a regra criada pelo hacker: `Porta 22 — Origem: 0.0.0.0/0`  

<img width="1431" height="446" alt="image" src="https://github.com/user-attachments/assets/acc1d5de-efdc-41ab-a7ce-9a25bb45be19" />

Salve as alterações.  

6. Excelente trabalho fortalecendo sua segurança! 🔐🚀  
- Removeu o acesso SSH inseguro criado pelo invasor  
- Reforçou a configuração do sshd_config  
- Bloqueou autenticação por senha  
- Garantiu que apenas conexões usando par de chaves e IP permitido possam acessar a instância  

### ☕ Tarefa 5.3 — Corrigir o site após a invasão  
Agora que o invasor foi removido e o acesso à instância está seguro, é hora de restaurar o site da cafeteria Café para o estado original.

1. Acessar o diretório de imagens do site
No terminal conectado via SSH à instância, navegue até o diretório onde as imagens do site são armazenadas:
```bash
cd /var/www/html/cafe/images/
ls -l
```
⚠️ Ao listar os arquivos, você verá que o hacker criou um backup da imagem original — um sinal de que ele substituiu o arquivo principal!  

<img width="696" height="389" alt="image" src="https://github.com/user-attachments/assets/fff7063a-813a-422f-b87b-2e570b3d3601" />

2. Restaurar o arquivo original  
Execute o comando abaixo para substituir a imagem alterada pela versão original criada pelo sistema:  
`sudo mv Coffee-and-Pastries.backup Coffee-and-Pastries.jpg`  
Isso restaura a imagem correta no site.  
<img width="797" height="365" alt="image" src="https://github.com/user-attachments/assets/83ef502a-9106-46aa-8c4e-55b82fdfa447" />

3. Verificar se o site voltou ao normal  
Abra no navegador: `http://<WebServerIP>/cafe`  
**Dica:**  
Se as imagens não atualizarem, pressione **Shift + Recarregar a pág** para forçar o navegador a recarregar sem cache.  

4. Resultado  
A imagem original foi restaurada e o site voltou ao normal — sem o conteúdo colocado pelo invasor.  
Site corrigido com sucesso! 🚀  
<img width="1199" height="659" alt="image" src="https://github.com/user-attachments/assets/331fae0b-9b97-48f4-b8be-8da10a72acdc" />

### 🛡️ Tarefa 5.4: Excluir o usuário hacker da AWS
Lembre-se de que o hacker não apenas acessou a instância EC2 que hospeda o site, mas também executou um comando da AWS CLI que abriu a porta 22 no grupo de segurança para toda a internet.  
Nesta etapa, vamos remover da conta o usuário **chaos** do AWS Identity and Access Management (AWS IAM).  

1. No **Console de Gerenciamento da AWS**, selecione o menu **Services (Serviços)** e escolha **IAM**.  
2. Clique em **Usuários**.  

<img width="1430" height="324" alt="image" src="https://github.com/user-attachments/assets/7fe195c5-3567-48e1-9b6d-596e5d084a7b" />

3. Marque a caixa de seleção ao lado do usuário **chaos**.  
4. Selecione **Excluir**.  
5. Insira o nome do usuário quando solicitado e clique em **Excluir** novamente.

<img width="600" height="468" alt="image" src="https://github.com/user-attachments/assets/fad6c702-0ac9-4479-86af-758b02dc1a89" />

<img width="1124" height="296" alt="image" src="https://github.com/user-attachments/assets/d1ac35fb-ddf2-4749-a32f-74b00e38be03" />

Ótimo trabalho! O usuário **chaos** foi completamente removido da conta. 🚫

---

## ☕ Atualização da cafeteria Café

<img width="227" height="177" alt="image" src="https://github.com/user-attachments/assets/62a31a3f-4c67-4c9a-80e9-53e702b3eae5" />

Todos na cafeteria Café estão aliviados por Sofia ter descoberto quem causou a violação, removido o acesso indevido ao servidor web e restaurado a segurança da conta AWS.  

No fim das contas, o hacker parecia estar apenas tentando se divertir — mas os danos poderiam ter sido sérios.  
Agora, toda a equipe envolvida na atualização e manutenção do site entende a importância de mantê-lo sempre seguro.  

Eles também continuarão utilizando o **AWS CloudTrail** como uma ferramenta essencial para auditar e monitorar atividades na conta AWS.  

✨ *Ótimo trabalho! Segurança reforçada e lições importantes aprendidas.*  

🏁 Atividade Concluída. Parabéns! 🎉  
