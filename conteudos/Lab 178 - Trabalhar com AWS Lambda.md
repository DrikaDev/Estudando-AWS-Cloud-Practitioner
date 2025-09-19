## 🧪 Lab 178 - Trabalhar com o AWS Lambda

## Índice

1. [Visão geral do laboratório](#visão-geral-do-laboratório)
2. [Etapas da função Lambda](#etapas-da-função-lambda)
3. [Laboratório AWS Lambda](#laboratório-aws-lambda)
   1. [Tarefa 1: Observar as configurações do perfil do IAM](#tarefa-1-observar-as-configurações-do-perfil-do-iam)
      - [Tarefa 1.1: Observar as configurações do perfil do IAM `salesAnalysisReport`](#tarefa-11-observar-as-configurações-do-perfil-do-iam-salesanalysisreport)
      - [Tarefa 1.2: Observar as configurações do perfil do IAM `salesAnalysisReportDERole`](#tarefa-12-observar-as-configurações-do-perfil-do-iam-salesanalysisreportderole)
   2. [Tarefa 2: Criar uma camada do Lambda e uma função extratora de dados](#tarefa-2-criar-uma-camada-do-lambda-e-uma-função-extratora-de-dados-do-lambda)
      - [Tarefa 2.1: Criar uma camada do Lambda](#tarefa-21-criar-uma-camada-do-lambda)
      - [Tarefa 2.2: Criar a função extratora de dados do Lambda](#tarefa-22-criar-a-função-extratora-de-dados-do-lambda)
      - [Tarefa 2.3: Adicionar a camada do Lambda à função](#tarefa-23-adicionar-a-camada-do-lambda-à-função)
      - [Tarefa 2.4: Importar o código da função extratora de dados do Lambda](#tarefa-24-importar-o-código-da-função-extratora-de-dados-do-lambda)
      - [Tarefa 2.5: Definir configurações de rede para a função](#tarefa-25-definir-configurações-de-rede-para-a-função)
   3. [Tarefa 3: Testar a função extratora de dados do Lambda](#tarefa-3-testar-a-função-extratora-de-dados-do-lambda)
      - [Tarefa 3.1: Iniciar um teste da função do Lambda](#tarefa-31-iniciar-um-teste-da-função-do-lambda)
      - [Tarefa 3.2: Solução de problemas da função](#tarefa-32-solução-de-problemas-da-função-extratora-de-dados-do-lambda)
      - [Tarefa 3.3: Analisando e corrigindo a função Lambda](#tarefa-33-analisando-e-corrigindo-a-função-lambda)
      - [Tarefa 3.4: Realizar um pedido e testar novamente](#tarefa-34-realizar-um-pedido-e-testar-novamente)
   4. [Tarefa 4: Configurar notificações](#tarefa-4-configurar-notificações)
      - [Tarefa 4.1: Criar um tópico do SNS](#tarefa-41-criar-um-tópico-do-sns)
      - [Tarefa 4.2: Assinar o tópico do SNS](#tarefa-42-assinar-o-tópico-do-sns)
   5. [Tarefa 5: Criar a função do Lambda `salesAnalysisReport`](#tarefa-5-criar-a-função-do-lambda-salesanalysisreport)
      - [Tarefa 5.1: Conectar-se à instância CLI Host](#tarefa-51-conectar-se-à-instância-cli-host)
      - [Tarefa 5.2: Configurar a AWS CLI](#tarefa-52-configurar-a-aws-cli)
      - [Tarefa 5.3: Criar a função do Lambda `salesAnalysisReport` usando a AWS CLI](#tarefa-53-criar-a-função-do-lambda-salesanalysisreport-usando-a-aws-cli)
      - [Tarefa 5.4: Configurar a função do Lambda `salesAnalysisReport`](#tarefa-54-configurar-a-função-do-lambda-salesanalysisreport)
      - [Tarefa 5.5: Testar a função do Lambda `salesAnalysisReport`](#tarefa-55-testar-a-função-do-lambda-salesanalysisreport)
      - [Tarefa 5.6: Adicionar um gatilho à função do Lambda `salesAnalysisReport`](#tarefa-56-adicionar-um-gatilho-à-função-do-lambda-salesanalysisreport)
6. [Conclusão](#conclusão)

---

## Visão geral do laboratório

Neste laboratório, vamos implantar e configurar uma solução de computação sem servidor baseada no **AWS Lambda**.  
A função do Lambda vai gerar um relatório de análise de vendas, extraindo dados de um banco de dados e enviando os resultados diariamente.  

Criaremos duas funções do Lambda, cada uma exigindo permissões para acessar os recursos da AWS com os quais interage.  

As informações de conexão do banco de dados são armazenadas no **armazenamento de parâmetros**, um recurso do **AWS Systems Manager**.  
O banco de dados é executado em uma instância do Linux do **Amazon Elastic Compute Cloud (Amazon EC2)**, com **Apache**, **MySQL** e **PHP (LAMP)**.

O diagrama a seguir mostra a arquitetura da solução do relatório de análise de vendas e ilustra a ordem em que as ações ocorrem:
<img width="1210" height="548" alt="image" src="https://github.com/user-attachments/assets/537f6ec5-098b-4a61-b77d-0a52acdabd7a" />

### Etapas da função Lambda

| Etapa | Detalhes |
|-------|----------|
| 1 | Um evento do **Amazon CloudWatch Events** chama a função do Lambda `salesAnalysisReport` às 20h todos os dias, de segunda a sábado. |
| 2 | A função do Lambda `salesAnalysisReport` invoca outra função do Lambda, `salesAnalysisReportDataExtractor`, para recuperar os dados do relatório. |
| 3 | A função `salesAnalysisReportDataExtractor` executa uma consulta analítica no banco de dados da cafeteria (`cafe_db`). |
| 4 | O resultado da consulta retorna para a função `salesAnalysisReport`. |
| 5 | A função `salesAnalysisReport` formata o relatório em uma mensagem e o publica no tópico do **Amazon Simple Notification Service (SNS)** `salesAnalysisReportTopic`. |
| 6 | O tópico do SNS `salesAnalysisReportTopic` envia a mensagem por e-mail ao administrador. |

Neste laboratório, o código Python para cada função do Lambda é fornecido para que você possa se concentrar nas tarefas de **SysOps** de implantação, 
configuração e teste dos componentes da solução sem servidor.

---

# Laboratório AWS Lambda

## Tarefa 1: Observar as configurações do perfil do IAM

Nesta tarefa, vamos analisar os perfis do IAM e as permissões que eles vão conceder às funções do Lambda `salesAnalysisReport` e 
`salesAnalysisReportDataExtractor`.

---

### Tarefa 1.1: Observar as configurações do perfil do IAM `salesAnalysisReport`

1. No **Console de Gerenciamento da AWS**, selecione **Serviços > Segurança, identidade e conformidade > IAM**.
2. No painel de navegação, selecione **Perfis**.
3. Na caixa de pesquisa, insira `sales`.
4. Nos resultados filtrados, selecione o hiperlink `salesAnalysisReportRole`.
<img width="1154" height="304" alt="image" src="https://github.com/user-attachments/assets/20560dcd-c7a5-4274-8633-dce2e9427649" />

5. Selecione a guia **Relações de confiança** e observe que `lambda.amazonaws.com` está listada como uma entidade confiável.
<img width="1432" height="530" alt="image" src="https://github.com/user-attachments/assets/dada1146-07ae-4755-9d25-7cba1d7f6511" />

6. Selecione a guia **Permissões** e observe as quatro políticas atribuídas a este perfil. Para expandir cada política, selecione o ícone `+` ao lado:

| Política | Permissões concedidas |
|----------|---------------------|
| AmazonSNSFullAccess | Acesso total aos recursos do Amazon SNS |
| AmazonSSMReadOnlyAccess | Acesso somente leitura aos recursos do Systems Manager |
| AWSLambdaBasicRunRole | Permissões de gravação para o CloudWatch Logs (exigidas por cada função do Lambda) |
| AWSLambdaRole | Permite a uma função do Lambda invocar outra função do Lambda |

> A função do Lambda `salesAnalysisReport` que você vai criar posteriormente usará o perfil `salesAnalysisReportRole`.

---

### Tarefa 1.2: Observar as configurações do perfil do IAM `salesAnalysisReportDERole`

1. Selecione **Perfis** novamente.
2. Na caixa de pesquisa, insira `sales`.
3. Nos resultados filtrados, selecione o hiperlink `salesAnalysisReportDERole`.
4. Selecione a guia **Relações de confiança** e observe que `lambda.amazonaws.com` está listada como uma entidade confiável.
5. Selecione a guia **Permissões** e observe as permissões concedidas a este perfil:

| Política | Permissões concedidas |
|----------|---------------------|
| AWSLambdaBasicRunRole | Permissões de gravação para o CloudWatch Logs |
| AWSLambdaVPCAccessRunRole | Permissões para gerenciar interfaces de rede elástica e conectar a função a uma VPC |

> A função do Lambda `salesAnalysisReportDataExtractor` usará o perfil `salesAnalysisReportDERole`.

[⬆ Voltar ao índice](#índice)

---

## Tarefa 2: Criar uma camada do Lambda e uma função extratora de dados do Lambda

Nesta tarefa, vamos criar uma camada do Lambda e, em seguida, criará uma função do Lambda que usará essa camada.

> Comece baixando os arquivos necessários:
- [pymysql-v3.zip](link-para-o-arquivo)
- [salesAnalysisReportDataExtractor-v3.zip](link-para-o-arquivo)

> Observação:  
O arquivo `salesAnalysisReportDataExtractor-v3.zip` é uma implementação em Python de uma função do Lambda que usa a biblioteca **PyMySQL** para acessar o banco de dados MySQL da cafeteria. A biblioteca PyMySQL está embalada no `pymysql-v3.zip` e será carregada na camada do Lambda.

---

### Tarefa 2.1: Criar uma camada do Lambda

As camadas do Lambda permitem **reutilizar código entre funções**, evitando que cada função precise incluir o pacote de implantação completo.

1. Abra o **Console de Gerenciamento da AWS** e selecione **Serviços > Computação > Lambda**.  
   - Dica: se o painel de navegação estiver fechado, selecione o ícone do menu recolhido (três linhas horizontais) para abri-lo.
2. Selecione **Camadas**.
3. Clique em **Criar camada**.
<img width="1907" height="494" alt="image" src="https://github.com/user-attachments/assets/06458553-fd2a-4f46-a730-a8f9eb3ed4eb" />

4. Configure as definições da camada:
- **Nome:** `pymysqlLibrary`  
- **Descrição:** `PyMySQL library modules`  
- **Upload de arquivo:** selecione `Fazer upload de um arquivo .zip` e envie `pymysql-v3.zip`  
- **Runtimes compatíveis:** `Python 3.9`
- Clique em **Criar**.  
   - A mensagem “Camada `pymysqlLibrary` versão 1 criada com êxito” será exibida.
   <img width="1839" height="94" alt="image" src="https://github.com/user-attachments/assets/db0e0c59-2cc3-44aa-b7bc-5e016a7a9ec1" />

> Dica: o arquivo `.zip` da camada deve estar em conformidade com uma **estrutura de pastas específica**. O `pymysqlLibrary.zip` fornecido já segue essa estrutura.

---

### Tarefa 2.2: Criar a função extratora de dados do Lambda

1. No painel de navegação do **AWS Lambda**, selecione **Funções** para abrir a página de funções.
2. Clique em **Criar função** e configure as opções:
   - **Criar do zero:** selecione esta opção na parte superior da página.
   - **Nome da função:** `salesAnalysisReportDataExtractor`
   - **Tempo de execução:** `Python 3.9`

3. Expanda **Alterar a função de execução padrão** e configure:
   - **Papel de execução:** selecione **Usar uma função existente**  
   - **Função existente:** `salesAnalysisReportDERole`
   - Clique em **Criar função**.  
   - Uma nova página será aberta com a mensagem:  
     *“A função `salesAnalysisReportDataExtractor` foi criada com êxito.”*
<img width="1844" height="120" alt="image" src="https://github.com/user-attachments/assets/d547af41-37c9-4062-8d44-6fe4815ad53d" />

---

### Tarefa 2.3: Adicionar a camada do Lambda à função

1. No **painel Visão geral da função**, selecione **Camadas**.
2. Na parte inferior da página, no painel **Camadas**, clique em **Adicionar uma camada**.
3. Na página **Adicionar camada**, configure as opções:
   - **Escolha uma camada:** selecione **Camadas personalizadas**
   - **Camadas personalizadas:** `pymysqlLibrary`
   - **Versão:** `1`
   - Clique em **Adicionar**.  
   - O painel **Visão geral da função** agora mostrará uma contagem de `(1)` no nó de Camadas para a função.
   <img width="1243" height="432" alt="image" src="https://github.com/user-attachments/assets/14f20dfa-8467-4245-a732-c605464e2063" />

---

### Tarefa 2.4: Importar o código da função extratora de dados do Lambda

1. Acesse **Lambda > Funções > salesAnalysisReportDataExtractor**.
2. Na guia abaixo **Configuração**, clique em **Editar**.
3. Em **Configurações básicas**, insira: `salesAnalysisReportDataExtractor.lambda_handler`  
4. Clique em **Salvar**.
5. Na guia **Código / Origem do código**, selecione **Fazer upload de**:
- **Arquivo:** `.zip`
- Clique em **Upload** e selecione o arquivo `salesAnalysisReportDataExtractor-v3.zip` que você baixou anteriormente.
6. Clique em **Salvar**.
<img width="1840" height="96" alt="image" src="https://github.com/user-attachments/assets/d54f3000-8007-4638-94d2-9c3735dc2190" />

> O código da função do Lambda será importado e exibido no painel **Origem do código**.  
> Se necessário, no painel de navegação **Ambiente**, clique duas vezes em `salesAnalysisReportDataExtractor.py` para exibir o código.

7. Revise o código Python que implementa a função.  
- Leia os comentários incluídos no código para entender o fluxo lógico.  
- Observe que a função espera receber as informações de conexão do banco de dados (`dbURL`, `dbName`, `dbUser` e `dbPassword`) no parâmetro de entrada de evento.

> Observação: se o código ainda não for exibido no editor, atualize o console para carregá-lo.

---

### Tarefa 2.5: Definir configurações de rede para a função

Antes de testar a função, é necessário definir as **configurações de rede**, pois a função precisa acessar o banco de dados da cafeteria executado em uma instância LAMP do EC2.  

1. No painel da função, selecione a guia **Configuração** e clique em **VPC**.
2. Clique em **Editar** e configure as seguintes opções:
   - **VPC:** selecione a VPC da cafeteria.
   - **Sub-redes:** selecione a **Sub-rede pública 1 da cafeteria**.  
     > Dica: você pode ignorar o aviso sobre escolher pelo menos duas sub-redes para alta disponibilidade, pois não se aplica a esta função.
   - **Grupos de segurança:** selecione **CafeSecurityGroup**.  
     > As regras de entrada e saída do grupo de segurança serão exibidas automaticamente abaixo do campo.

3. Clique em **Salvar**.

[⬆ Voltar ao índice](#índice)

---

## Tarefa 3: Testar a função extratora de dados do Lambda

### Tarefa 3.1: Iniciar um teste da função do Lambda

Agora que tudo está configurado, podemos testar a função `salesAnalysisReportDataExtractor`.  
Para invocá-la, é necessário fornecer os valores dos parâmetros de conexão do banco de dados da cafeteria, que estão armazenados no **Systems Manager Parameter Store**.

1. Abra uma nova guia do navegador, acesse o **Console de Gerenciamento da AWS** e selecione:  
   **Serviços > Gerenciamento e governança > Systems Manager**.
2. No painel de navegação, selecione **Repositório de parâmetros**.
3. Selecione cada um dos parâmetros a seguir e copie o **Valor** para um documento de texto:

   - `/cafe/dbUrl`
   - `/cafe/dbName`
   - `/cafe/dbUser`
   - `/cafe/dbPassword`

4. Retorne à guia do navegador com o **Console do Lambda**. Na função `salesAnalysisReportDataExtractor`, selecione a guia **Testar**.
5. Configure o painel **Eventos de teste** da seguinte forma:
   - **Ação de evento de teste:** selecione **Criar novo evento**  
   - **Nome do evento:** `SARDETestEvent`  
   - **Modelo:** selecione **hello-world**  
   - No painel **JSON do evento**, substitua o objeto JSON pelo seguinte:

```json
{
  "dbUrl": "<value of /cafe/dbUrl parameter>",
  "dbName": "<value of /cafe/dbName parameter>",
  "dbUser": "<value of /cafe/dbUser parameter>",
  "dbPassword": "<value of /cafe/dbPassword parameter>"
}
```
   - Nesse código, substitua o valor de cada parâmetro pelos valores colados em um editor de texto nas etapas anteriores.
   - Insira esses valores entre aspas.

6. Selecione Salvar.
7. Selecione Testar.
Apos alguns momentos, a página mostra a mensagem “Resultado da execução: com falha”.
<img width="1834" height="227" alt="image" src="https://github.com/user-attachments/assets/b05ae884-61e4-4971-88dc-407d314ad18c" />

---

### Tarefa 3.2: Solução de problemas da função extratora de dados do Lambda

1. No **painel Resultado da execução**, clique em **Detalhes** para expandir.  
   - Observe que o objeto de erro retornou uma mensagem semelhante a esta após a execução da função:

```
{
  "errorMessage": "2019-02-14T04:14:15.282Z ff0c3e8f-1985-44a3-8022-519f883c8412 Task timed out after 3.00 seconds"
}
```

Esta mensagem indica que a função **excedeu o tempo limite de 3 segundos**.

Na seção **Log output**, são exibidas linhas que começam com as seguintes palavras-chave:

- **START**: indica que a função começou a ser executada.
- **END**: indica que a função terminou a execução.
- **REPORT**: fornece um resumo de desempenho e estatísticas de utilização de recursos durante a execução da função.

---

### Causa do erro

O erro foi causado porque a função **atingiu o tempo limite configurado** antes de concluir sua execução.  
Possíveis motivos incluem:

- Configurações de rede incorretas, impedindo a função de acessar o banco de dados.
- Conexão lenta ou indisponível com o banco de dados.
- Volume de processamento maior que o esperado para o tempo limite atual da função.

> Solução: ajustar o **timeout** da função Lambda ou verificar as **configurações de VPC, sub-rede e grupo de segurança**.

---

### Tarefa 3.3: Analisando e corrigindo a função Lambda

Nesta tarefa, vamos analisar e corrigir o problema observado ao testar a função `salesAnalysisReportDataExtractor`.

#### Dicas para encontrar a solução

- Uma das primeiras ações da função é **conectar-se ao banco de dados MySQL** que está rodando em uma instância EC2 separada.  
  - A função espera um certo tempo para estabelecer a conexão.  
  - Se a conexão não for bem-sucedida dentro desse tempo, a função **atinge o timeout**.

- Por padrão, um banco de dados MySQL usa o **protocolo MySQL** e escuta na **porta 3306** para acesso de clientes.

#### Como corrigir o problema

1. No console do Lambda, selecione novamente a guia **Configuração** e clique em **VPC**.
2. Verifique as **Regras de entrada (Inbound rules)** do grupo de segurança usado pela instância EC2 que hospeda o banco de dados.  
   - Confirme se a **porta 3306** está listada.  
   - Caso não esteja, clique no link do grupo de segurança para **editar** e adicionar uma regra de entrada permitindo acesso à porta 3306.

3. Após corrigir a configuração, retorne à guia **Testar** da função `salesAnalysisReportDataExtractor` e clique em **Testar** novamente.

#### Resultado esperado

- Agora você deve ver um **quadro verde** com a mensagem:  
  *“Execution result: succeeded (logs).”*  
  - Isso indica que a função foi executada com sucesso.

- Clique em **Details** para expandir e visualizar o resultado.  
  - A função retornará o seguinte objeto JSON:

```
{
  "statusCode": 200,
  "body": []
}
```
> Observação: o campo body, que contém os dados do relatório extraídos pela função, está vazio porque não há dados de pedidos no banco de dados.

---

### Tarefa 3.4: Realizar um pedido e testar novamente

Nesta tarefa, vamos acessar o site da cafeteria e fará alguns pedidos para **popular o banco de dados** com dados de pedidos.

#### Localizando o endereço público do site da cafeteria

O URL do site tem o formato: ` http://publicIP/cafe ` onde `publicIP` é o **endereço IPv4 público** da instância EC2 da cafeteria.

##### Opção 1:

1. No **Console de Gerenciamento da AWS**, selecione:  
   **Serviços > Compute > EC2**
2. No painel de navegação, escolha **Instâncias**.
3. Selecione **CafeInstance**.
4. Copie o **Public IPv4 address** para um editor de texto.
5. Em uma nova guia do navegador, acesse: ` http://publicIP/cafe ` substituindo `publicIP` pelo endereço IPv4 copiado.
6. Pressione **Enter** para carregar o site da cafeteria.

##### Opção 2:

1. No topo das instruções, selecione **Details** e clique em **Show**.
2. Na janela de credenciais, copie o **CafePublicIP** para um editor de texto.
3. Em uma nova guia do navegador, acesse: ` http://publicIP/cafe ` substituindo `publicIP` pelo endereço IPv4 copiado.
4. Pressione **Enter** para carregar o site da cafeteria.

#### Realizando pedidos

1. No site da cafeteria, selecione **Menu**.
2. Faça alguns pedidos para adicionar dados de pedidos ao banco de dados.

#### Testando novamente a função Lambda

1. Retorne à guia do navegador com a função `salesAnalysisReportDataExtractor`.
2. Selecione a guia **Testar** e clique em **Testar**.
3. O objeto JSON retornado agora contém informações de **quantidade de produtos** no campo `body`, similar a:

```
{
  "statusCode": 200,
  "body": [
    {
      "product_group_number": 1,
      "product_group_name": "Pastries",
      "product_id": 1,
      "product_name": "Croissant",
      "quantity": 1
    },
    {
      "product_group_number": 2,
      "product_group_name": "Drinks",
      "product_id": 8,
      "product_name": "Hot Chocolate",
      "quantity": 2
     }
    ]
}
```

### Conclusão

🎉 **Parabéns!** Você criou com sucesso a função **salesAnalysisReportDataExtractor** no AWS Lambda.

A função agora está pronta para extrair dados de pedidos do banco de dados da cafeteria e gerar relatórios diários conforme configurado.

[⬆ Voltar ao índice](#índice)

---

## Tarefa 4: Configurar notificações

Nesta tarefa, vamos criar um tópico do **SNS** e, depois, inscreverá um endereço de e-mail no tópico.

### Tarefa 4.1: Criar um tópico do SNS

1. No Console de Gerenciamento da AWS, selecione:  
   **Serviços > Integração de aplicações > Simple Notification Service (SNS)**.
2. No painel de navegação, selecione **Tópicos** e clique em **Criar tópico**.
   - Tipo: **Padrão**
   - Nome: `salesAnalysisReportTopic`
   - Nome de exibição: `SARTopic`
3. Clique em **Criar tópico**.
4. Copie o **ARN** do tópico para uso futuro (em um editor de texto).

### Tarefa 4.2: Assinar o tópico do SNS

1. Clique em **Criar assinatura**.
   - Protocolo: **E-mail**
   - Endpoint: insira um endereço de e-mail que você possa acessar.
2. Clique em **Criar assinatura**.
3. Na caixa de entrada do e-mail fornecido, abra a mensagem do SNS e clique em **Confirmar assinatura**.
4. Uma nova página exibirá: **“Assinatura confirmada!”**

[⬆ Voltar ao índice](#índice)

---

## Tarefa 5: Criar a função do Lambda salesAnalysisReport

Esta função é o **orquestrador principal** do relatório de análise de vendas. Ela:

- Recupera informações de conexão do banco de dados no **armazenamento de parâmetros**.
- Invoca a função `salesAnalysisReportDataExtractor` para extrair os dados.
- Formata e publica a mensagem com os dados no **tópico SNS**.

### Tarefa 5.1: Conectar-se à instância CLI Host

1. No Console do EC2, selecione **Instâncias**.
2. Marque a instância **CLI Host** e clique em **Conectar-se**.
3. Use a guia **EC2 Instance Connect** para abrir o terminal.

### Tarefa 5.2: Configurar a AWS CLI

No terminal da CLI Host, execute: ` aws configure `

Nos prompts, insira as seguintes informações:

- **AWS Access Key ID (ID da chave de acesso da AWS):**  
  Acima destas instruções, selecione o menu suspenso **Detalhes** e escolha **Mostrar**. A janela Credenciais será exibida. Copie o valor **AccessKey**, cole no terminal e pressione **Enter**.

- **AWS Secret Access Key (Chave de acesso secreta da AWS):**  
  Na mesma janela Credenciais, copie o valor **SecretKey**, cole no terminal e pressione **Enter**.

- **Default region name (Nome da região padrão):**  
  Insira o código da região onde a função Lambda anterior foi criada. Para este laboratório, use **us-west-2** e pressione **Enter**.  
  Para encontrar o código, no canto superior direito do console do Lambda, selecione o menu suspenso **Região**.

- **Default output format (Formato de saída padrão):**  
  Insira `json` e pressione **Enter**.

---

### Tarefa 5.3: Criar a função do Lambda `salesAnalysisReport` usando a AWS CLI

Para verificar se o arquivo `salesAnalysisReport-v2.zip` que contém o código da função do Lambda `salesAnalysisReport` já está na CLI Host, execute os seguintes comandos no terminal: ` cd activity-files ` ` ls `

> **Observação:** antes de criar a função, é necessário recuperar o **ARN** do perfil do IAM `salesAnalysisReportRole`. Especifique-o nas etapas a seguir.

Para encontrar o ARN de um perfil do IAM:

1. Abra o **Console de Gerenciamento do IAM** e selecione **Perfis**.
2. Na caixa de pesquisa, insira `salesAnalysisReportRole` e selecione o nome do perfil.
3. Na página **Resumo**, copie o **ARN** e cole em um documento do editor de texto.

Depois, use o comando `create-function` do Lambda para criar a função do Lambda e configurá-la para usar o perfil do IAM `salesAnalysisReportRole`.

No prompt de comando da janela do terminal, cole o comando a seguir.  

> Substitua `<salesAnalysisReportRoleARN>` pelo valor do ARN que você copiou e `<region>` pelo código da região `us-west-2`, onde você criou a função Lambda anterior. Para encontrar o código da região, no canto superior direito do console do Lambda, selecione o menu suspenso **Região**.

```
aws lambda create-function \
--function-name salesAnalysisReport \
--runtime python3.9 \
--zip-file fileb://salesAnalysisReport-v2.zip \
--handler salesAnalysisReport.lambda_handler \
--region <region> \
--role <salesAnalysisReportRoleARN>
```

Depois que o comando for concluído, ele exibirá um objeto JSON descrevendo os atributos da função. Agora conclua a configuração e teste-a.

### Tarefa 5.4: Configurar a função do Lambda `salesAnalysisReport`

1. Abra o **Console de Gerenciamento do Lambda**.
2. Selecione **Funções** e escolha `salesAnalysisReport`.
3. A página **Detalhes da função** será aberta.
4. Revise os detalhes nos painéis **Visão geral da função** e **Origem do código da função criada**.
5. Leia o código da função e utilize os comentários incorporados para entender a lógica.

> Observação: na linha 26, a função recupera o ARN do tópico para publicação de uma variável de ambiente chamada `topicARN`. Você precisará definir essa variável no painel **Variáveis de ambiente**.

6. Selecione a guia **Configuração** > **Variáveis de ambiente**.
7. Selecione **Editar**.
8. Clique em **Adicionar variável de ambiente** e configure as opções:

   - **Chave:** `topicARN`  
   - **Valor:** cole o valor do ARN do tópico do SNS `salesAnalysisReportTopic` que você copiou anteriormente.

9. Selecione **Salvar**.

> A mensagem “A função `salesAnalysisReport` foi atualizada com êxito” será exibida.

---

### Tarefa 5.5: Testar a função do Lambda `salesAnalysisReport`

Agora, está tudo pronto para testar a função:

1. Selecione a guia **Testar** e configure o evento de teste:
   - **Ação de evento de teste:** Criar novo evento
   - **Nome do evento:** `SARTestEvent`
   - **Modelo:** `hello-world`

> Observação: A função não exige parâmetros de entrada. Mantenha as linhas JSON padrão como estão.

2. Selecione **Salvar**.
3. Selecione **Testar**.

> Uma caixa verde com a mensagem “Resultado da execução: com êxito (logs)” será exibida.

#### Dica
Se ocorrer um erro de tempo limite:
- Selecione **Testar** novamente.  
- Ou aumente o valor de tempo limite:
  1. Selecione a guia **Configuração** > **Configuração geral**.
  2. Selecione **Editar**.
  3. Ajuste o **Tempo limite** conforme necessário.
  4. Selecione **Salvar**.

4. Selecione **Detalhes** para expandir a execução.

A função deve retornar o seguinte objeto JSON:

```json
{
    "statusCode": 200,
    "body": "\"Sale Analysis Report sent.\""
}
```
Após testar a função, confira a caixa de entrada de e-mail.
Se não houver erros, você receberá um e-mail de **Notificações da AWS** com o assunto:
> “Relatório diário de análise de vendas”
O e-mail deverá conter um relatório semelhante a este, dependendo dos pedidos realizados no site da cafeteria:

<img width="770" height="498" alt="image" src="https://github.com/user-attachments/assets/49633c47-44cb-462e-9b8f-fd43998f7c76" />

É possível fazer mais pedidos no site da cafeteria e testar a função para ver as alterações no relatório que você receber.

**Ótimo trabalho!** Você testou com êxito a unidade da função do Lambda `salesAnalysisReport`.

---

## Tarefa 5.6: Adicionar um gatilho à função do Lambda salesAnalysisReport

Para concluir a implementação da função `salesAnalysisReport`, configure o relatório para ser iniciado todos os dias, de segunda a sábado, às 20h.  
Para isso, use um evento do **CloudWatch** como mecanismo de gatilho.

1. No painel **Visão geral da função**, selecione **Adicionar gatilho**.
2. No painel **Adicionar gatilho**, configure as seguintes opções:
   - **Configuração do gatilho:** selecione **EventBridge (CloudWatch Events)**.
   - **Regra:** selecione **Criar uma regra**.
   - **Nome da regra:** `salesAnalysisReportDailyTrigger`.
   - **Descrição da regra:** `Initiates report generation on a daily basis`.
   - **Tipo de regra:** **Expressão de programação**.
   - **Expressão de programação:** especifique a programação usando uma expressão Cron com seis campos separados:
     ```
     cron(Minutes Hours Day-of-month Month Day-of-week Year)
     ```
     > Todos os horários em uma expressão Cron são baseados no fuso horário UTC.

3. Exemplos para testar a função cinco minutos a partir da hora atual:
   - Londres (UTC), hora atual 11h30:
     ```
     cron(35 11 ? * MON-SAT *)
     ```
   - Nova York (UTC-5), hora atual 11h30:
     ```
     cron(35 16 ? * MON-SAT *)
     ```
   > Essas expressões programam o evento para ser invocado às 11h35 de segunda a sábado.

4. Para obter mais informações sobre a sintaxe de expressões Cron, consulte: [Schedule Expressions for Rules](https://docs.aws.amazon.com/eventbridge/latest/userguide/scheduled-events.html).

5. Clique em **Adicionar**.

O novo gatilho será exibido nos painéis **Visão geral da função** e **Gatilhos**.

> Desafio: Ajuste a expressão Cron para produção, garantindo que a função seja invocada todos os dias, de segunda a sábado, considerando o fuso horário UTC.

Aguarde cinco minutos e confira a caixa de entrada de e-mail.  
Se não houver erros, você verá um novo e-mail de **Notificações da AWS** com o assunto:

> “Relatório de análise de vendas diárias”

O evento do CloudWatch Events invocou essa mensagem no momento especificado na expressão Cron.

[⬆ Voltar ao índice](#índice)

---

## Conclusão

**Parabéns!** Você concluiu as seguintes tarefas com êxito:

- Reconheceu as permissões de política do IAM necessárias para habilitar uma função do Lambda para outros recursos da AWS.
- Criou uma camada do Lambda para satisfazer uma dependência de biblioteca externa.
- Criou funções do Lambda que extraem dados do banco de dados e enviam relatórios ao usuário.
- Implantou e testou uma função do Lambda que é iniciada com base em uma programação e que invoca outra função.
- Usou o CloudWatch Logs para solucionar problemas ao executar uma função do Lambda.

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
