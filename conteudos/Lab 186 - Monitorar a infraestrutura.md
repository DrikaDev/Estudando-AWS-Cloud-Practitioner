## 🧪 Lab 186 - Monitorar a infraestrutura

### Índice

1. [Visão Geral do Laboratório](#visão-geral-do-laboratório)  
2. [Objetivos do Laboratório](#objetivos-do-laboratório)  
3. [Tarefa 1: Instalar e Configurar o Agente do CloudWatch](#tarefa-1-instalar-e-configurar-o-agente-do-cloudwatch)  
4. [Tarefa 2: Monitorar Logs da Aplicação usando o CloudWatch Logs](#tarefa-2-monitorar-logs-da-aplicação-usando-o-cloudwatch-logs)  
5. [Tarefa 3: Monitorar Métricas de Instância usando o CloudWatch](#tarefa-3-monitorar-métricas-de-instância-usando-o-cloudwatch)  
6. [Tarefa 4: Criar Notificações em Tempo Real](#tarefa-4-criar-notificações-em-tempo-real)  
7. [Tarefa 5: Monitorar a Conformidade da Infraestrutura](#tarefa-5-monitorar-a-conformidade-da-infraestrutura)  

## Visão geral do laboratório

A capacidade de monitorar aplicações e infraestrutura é essencial para o fornecimento de serviços de TI confiáveis e consistentes.  
Os requisitos de monitoramento variam da coleta de estatísticas para a análise de longo prazo até a reação rápida a alterações e interrupções.  
O monitoramento também pode oferecer compatibilidade com relatórios de conformidade, conferindo continuamente se a infraestrutura está atendendo aos 
padrões organizacionais.  

Este laboratório mostra como usar:
- **Amazon CloudWatch Metrics**
- **Amazon CloudWatch Logs**
- **Amazon CloudWatch Events**
- **AWS Config**  

para monitorar aplicações e infraestrutura.  

---

## Objetivos do laboratório
Depois de concluir este laboratório, você será capaz de:

- Usar o **AWS Systems Manager Run Command** para instalar o **agente do CloudWatch** nas instâncias do **Amazon Elastic Compute Cloud (Amazon EC2)**.  
- Monitorar **logs de aplicações** usando o **CloudWatch Agent** e o **CloudWatch Logs**.  
- Monitorar **métricas do sistema** usando o **CloudWatch Agent** e as **métricas do CloudWatch**.  
- Criar **notificações em tempo real** usando o **CloudWatch Events**.  
- Acompanhar a **conformidade da infraestrutura** usando o **AWS Config**.  

---

## Tarefa 1: Instalar e configurar o agente do CloudWatch

Nesta tarefa, vamos usar o **Systems Manager** para instalar o agente do **CloudWatch** em uma instância do EC2.  
Vamos configurar ele para coletar métricas da aplicação e do sistema.  

<img width="1270" height="223" alt="image" src="https://github.com/user-attachments/assets/e54e336e-a8c0-47ec-99de-e727c7de3f25" />

### Passo 1: Acessar o Systems Manager

1. No **Console de Gerenciamento da AWS**, no menu **Serviços**, selecione **Systems Manager**.  

2. No painel de navegação à esquerda, selecione **Executar comando**.

---

### Passo 2: Executar comando para instalar o CloudWatch Agent

1. Clique em **Executar um comando**.  

2. Selecione o documento **AWS-ConfigureAWSPackage** (normalmente aparece no topo da lista).

<img width="1316" height="601" alt="image" src="https://github.com/user-attachments/assets/399e2450-02dd-46b0-94b6-cacce3cd7910" />

3. Na seção **Parâmetros de comando**, configure:  
   - **Ação**: `Instalar`  
   - **Nome**: `AmazonCloudWatchAgent`  
   - **Versão**: `latest`  
4. Em **Destinos**, selecione **Escolher instâncias manualmente** e marque a instância **Web Server**.  
5. Clique em **Executar**.  
6. Aguarde até que o **Status geral** mude para **Com êxito** (atualize a página, se necessário).  

<img width="1305" height="201" alt="image" src="https://github.com/user-attachments/assets/b7f1c80d-0344-4830-80f5-44c66ace7fc9" />
<img width="1308" height="217" alt="image" src="https://github.com/user-attachments/assets/03a3667e-10e4-4947-b3a5-6e6bfd3b990b" />

7. Role para baixo e verifique:  
   - Em **Destinos e saídas**, selecione a instância e clique em **Visualizar saída**.  
   
   <img width="1314" height="322" alt="image" src="https://github.com/user-attachments/assets/3fde06ad-7ed1-46ef-bf66-c8152ec6db4e" />

   - Expanda **Etapa 1 - Descrição e status do comando** → deve aparecer: ` arn:aws:ssm:::package/AmazonCloudWatchAgent instalado com êxito `
     <img width="1244" height="550" alt="image" src="https://github.com/user-attachments/assets/9068b199-522d-489f-88cd-d500d9248a6f" />

   > Se aparecer uma mensagem sobre **pré-condições insatisfeitas para Windows**, ignore (a instância é Linux).  

---

### Passo 3: Criar configuração no Parameter Store

1. No painel de navegação à esquerda, selecione **Armazenamento de parâmetros**.  

2. Clique em **Criar parâmetro** e configure:  
   - **Nome**: `Monitor-Web-Server`  
   - **Descrição**: `Collect web logs and system metrics`  
   - **Valor**: copie e cole a configuração abaixo:  

   ```
   {
     "logs": {
       "logs_collected": {
         "files": {
           "collect_list": [
             {
               "log_group_name": "HttpAccessLog",
               "file_path": "/var/log/httpd/access_log",
               "log_stream_name": "{instance_id}",
               "timestamp_format": "%b %d %H:%M:%S"
             },
             {
               "log_group_name": "HttpErrorLog",
               "file_path": "/var/log/httpd/error_log",
               "log_stream_name": "{instance_id}",
               "timestamp_format": "%b %d %H:%M:%S"
             }
           ]
         }
       }
     },
     "metrics": {
       "metrics_collected": {
         "cpu": {
           "measurement": [
             "cpu_usage_idle",
             "cpu_usage_iowait",
             "cpu_usage_user",
             "cpu_usage_system"
           ],
           "metrics_collection_interval": 10,
           "totalcpu": false
         },
         "disk": {
           "measurement": [
             "used_percent",
             "inodes_free"
           ],
           "metrics_collection_interval": 10,
           "resources": ["*"]
         },
         "diskio": {
           "measurement": ["io_time"],
           "metrics_collection_interval": 10,
           "resources": ["*"]
         },
         "mem": {
           "measurement": ["mem_used_percent"],
           "metrics_collection_interval": 10
         },
         "swap": {
           "measurement": ["swap_used_percent"],
           "metrics_collection_interval": 10
         }
       }
     }
   }
   ```

Examine a configuração acima. Ela define os seguintes itens a serem monitorados:  

- **Logs**: dois arquivos de log do servidor web que serão coletados e enviados ao **CloudWatch Logs**.  
- **Métricas**: métricas de **CPU**, **disco** e **memória** que serão enviadas ao **CloudWatch Metrics**.  

3. Clique em **Criar parâmetro**. Esse parâmetro será referenciado ao iniciar o agente do CloudWatch.  

---

### Passo 4: Iniciar o CloudWatch Agent

1. No painel de navegação à esquerda, escolha **Executar comando**.  

2. Clique em **Executar comando**.  

3. Configure o filtro de documentos:  

   - **Prefixo do nome do documento**  
   - **É igual a**  
   - Valor: `AmazonCloudWatch-ManageAgent`  
   - Pressione **Enter**.  

4. Verifique se o filtro está correto: `Prefixo do nome do documento : Equals : AmazonCloudWatch-ManageAgent`  

<img width="1324" height="636" alt="image" src="https://github.com/user-attachments/assets/45043841-0ba5-4ccf-8a69-5041aa421602" />

5. Selecione **AmazonCloudWatch-ManageAgent** (clique no nome).

- Uma nova aba será aberta mostrando a definição do comando.  
- Navegue pelas guias para ver como o documento é estruturado.  
- Na guia **Conteúdo**, role até o final para visualizar o script real.  
- Esse script faz referência ao **AWS Systems Manager Parameter Store**, recuperando a configuração criada anteriormente.  
- Feche a aba para retornar à tela de execução do comando.  

---

### Passo 5: Executar o comando para iniciar o agente

1. Certifique-se de que selecionou **AmazonCloudWatch-ManageAgent**.  

2. Na seção **Parâmetros de comando**, configure:  

- **Ação**: `configurar`  
- **Modo**: `ec2`  
- **Origem de configuração opcional**: `ssm`  
- **Local de configuração opcional**: `Monitor-Web-Server`  
- **Reinício opcional**: `sim`  

3. Em **Destinos**, selecione **Escolher instâncias manualmente**.  

4. Marque a instância **Web Server**.  

5. Clique em **Executar**.  

6. Aguarde até que o **Status geral** mude para **Com êxito** (use o botão **Atualizar** se necessário).  

✅ O **CloudWatch Agent** agora está em execução na instância e enviando **logs** e **métricas** para o **Amazon CloudWatch**.  

[⬆ Voltar ao índice](#índice)

---

## Tarefa 2: Monitorar logs da aplicação usando o CloudWatch Logs

Nesta tarefa, vamos **gerar dados de log** no servidor web, e **monitorar os logs** usando o **Amazon CloudWatch Logs**.  

<img width="1307" height="228" alt="image" src="https://github.com/user-attachments/assets/d4534ec7-31f2-4206-97e8-3ed7d7953cc3" />

### Passo 1: Gerar dados de log no servidor web

O servidor web gera dois tipos de logs:  
- **Logs de acesso**  
- **Logs de erro**  

1. No console do laboratório, selecione o menu suspenso **Detalhes** (acima das instruções) e escolha **Mostrar**.  

2. Copie o valor **WebServerIP**.  

3. Abra uma nova guia do navegador, cole o **WebServerIP** e pressione **Enter**.  
   - Você deverá ver a **Página de teste do servidor da web**.
   
   <img width="1908" height="653" alt="image" src="https://github.com/user-attachments/assets/afb15ae7-6449-4a3f-8336-891a653cc8e7" />
  
4. Agora, gere dados de log tentando acessar uma página inexistente:  
   - No navegador, adicione `/start` ao final do endereço e pressione **Enter**.  
   - Você verá uma mensagem de erro (**página não encontrada**).  
   
   <img width="1717" height="231" alt="image" src="https://github.com/user-attachments/assets/5e5f83e5-4a58-42a5-b495-664dcc730ffd" />

   > Isso é esperado: a tentativa gerará dados nos **logs de acesso**, que estão sendo enviados para o **CloudWatch Logs**.  

   > ⚠️ Mantenha essa guia aberta, mas volte para o **Console da AWS**.  

---

### Passo 2: Acessar os logs no CloudWatch

1. No **Console de Gerenciamento da AWS**, vá até o menu **Serviços** e selecione **CloudWatch**.  

2. No painel de navegação à esquerda, clique em **Grupos de logs**.  

3. Você deverá ver dois grupos de log listados:  
   - `HttpAccessLog`  
   - `HttpErrorLog`  
   - *(Se não aparecerem, aguarde 1 minuto e clique em **Atualizar**).*  

4. Clique em **HttpAccessLog** (nome do grupo).  

5. Na seção **Streams de logs**, clique no **fluxo de log** exibido na tabela.  
   - Ele terá o mesmo **ID da instância EC2** associada.  

<img width="1447" height="366" alt="image" src="https://github.com/user-attachments/assets/da68b6d5-bd6d-416d-b7d8-7c0bd59ca9eb" />

---

### Passo 3: Explorar os dados de log

- Os **dados de log** devem aparecer, mostrando as solicitações `GET` enviadas ao servidor web.  
- É possível expandir linhas para visualizar informações adicionais sobre o **computador** e o **navegador** que fizeram a requisição.  
- Você deverá ver uma linha com sua solicitação `/start`, retornando o **código 404** (página não encontrada).  

<img width="1481" height="672" alt="image" src="https://github.com/user-attachments/assets/bc15f16e-d710-429f-a54f-c1a57750bbd7" />

✅ Essa tarefa demonstra como arquivos de log podem ser enviados automaticamente de uma **instância EC2** ou de um **servidor on-premises** para o 
**CloudWatch Logs**.  
- Os dados podem ser acessados sem precisar entrar em cada servidor individual.  
- É possível coletar logs de **múltiplos servidores**, como uma frota de servidores web em **Auto Scaling**.  

---

### Passo 4: Criar um filtro de métrica no CloudWatch Logs

Agora vamos configurar um filtro para identificar erros **404** no arquivo de log.  
Esse tipo de erro normalmente indica que o servidor web está gerando links inválidos, acessados pelos usuários.

1. No **painel de navegação** à esquerda, selecione **Grupos de logs**.  

2. Marque a caixa de seleção ao lado de **HttpAccessLog**.  

3. No menu suspenso **Ações**, selecione **Criar filtro de métrica**.  

<img width="1897" height="571" alt="image" src="https://github.com/user-attachments/assets/91edeab2-37bc-4b12-93dc-8aecfbdd207b" />

4. Cole a seguinte linha na caixa **Padrão de filtro**: ` [ip, id, user, timestamp, request, status_code=404, size] `

- Essa linha informará ao **CloudWatch Logs** como interpretar os campos nos dados de log e define um filtro para encontrar apenas linhas com
  `status_code=404`, que indica que uma página não foi encontrada.  

5. Em **Testar padrão**, use o menu suspenso para selecionar o **ID da instância EC2**  
   *(exemplo: `i-0f07ab62aae4xxxx9`)*.  

6. Clique em **Testar padrão**.  

7. Na seção **Resultados**, selecione **Exibir resultados de teste**.  

   ✅ Você deve ver pelo menos um resultado com `$status_code` de **404**, que indica que foi solicitada uma página inexistente.  

   <img width="1035" height="461" alt="image" src="https://github.com/user-attachments/assets/daa832e9-8aea-4b95-9608-ee166f22c96e" />

8. Clique em **Próximo**.  
  
9. Em **Atribuir métrica**:

#### Criar Nome do Filtro 
- **Nome do filtro**: `404Errors`

### Detalhes da Métrica
- **Namespace da métrica**: `LogMetrics`  
- **Nome da métrica**: `404Errors`  
- **Valor da métrica**: `1`  

- Clique em **Próximo**.  

### Revisar e Criar

- Clique no botão **Criar filtro de métrica**.  

<img width="1459" height="93" alt="image" src="https://github.com/user-attachments/assets/1279f36f-e31f-4a3a-815b-3aa5d5fd66cc" />

### Resultado Final

Esse filtro de métrica agora pode ser usado na criação de um **alarme do CloudWatch**, permitindo monitorar automaticamente quando ocorrerem **erros 404**.

---

### Passo 5: Criar um Alarme Usando o Filtro de Métrica

Agora vamos configurar um **alarme** para enviar uma notificação quando muitos erros **404 Não encontrado** forem recebidos.  

1. No painel **404Errors**, marque a caixa de seleção no canto superior direito.  

2. Na seção **Filtros da métrica**, selecione **Criar alarme**.  

<img width="1458" height="324" alt="image" src="https://github.com/user-attachments/assets/6bf79ef6-946d-4f1d-99a2-ee2fa7f24861" />

3. Especificar métrica e condições:

### Métricas
- **Período**: `1 minuto`  

### Condições
- Sempre que `404Errors` for: **Maior/igual**  
- Que...: **5**  

<img width="1409" height="564" alt="image" src="https://github.com/user-attachments/assets/2b34e9f8-03b6-4d8c-adce-b73b5103bb85" />

- Clique em **Próximo**.  

### Notificação
- **Enviar uma notificação para o seguinte tópico do SNS**: escolha **Criar novo tópico**.  
- **Endpoints de e-mail**: insira um endereço de e-mail válido.  
- Clique em **Criar tópico**.  
- Clique em **Próximo**.  

### Nome e Descrição
- **Nome do alarme**: `404 Error`  
- **Descrição do alarme**: `Alerta quando muitos 404s são detectados em uma instância`  
- Clique em **Próximo** e depois em **Criar alarme**.  

---

### Confirmação da Assinatura

1. Acesse o e-mail informado.  
2. Procure a mensagem de confirmação e selecione o link **Confirmar assinatura**.  
3. Volte ao **Console de Gerenciamento da AWS**.  

---

### Testando o Alarme

1. No painel de navegação à esquerda, selecione **CloudWatch**.  
   - O alarme aparecerá em **laranja**: *Dados insuficientes* (nenhum dado recebido no último minuto).  

<img width="1889" height="683" alt="image" src="https://github.com/user-attachments/assets/7fcfd43c-6799-4009-b04a-8f3e1b516af4" />

2. Volte para a guia do navegador com o **servidor web**.  
   - Se ela não estiver aberta, recupere o **WebServerIP** em **Detalhes** > **Mostrar**.  

3. Cole o **WebServerIP** em uma nova guia do navegador.  

4. Acesse páginas inexistentes pelo menos **5 vezes**:  
   - Exemplo: `http://192.0.2.0/start2`  

   🔹 Cada tentativa gera uma entrada separada de log.  

5. Aguarde **1–2 minutos** e atualize a página do CloudWatch.  
   - O gráfico ficará **vermelho**, indicando que o alarme foi acionado.  

<img width="754" height="470" alt="image" src="https://github.com/user-attachments/assets/079c473e-b4f9-40ee-8778-5f655b435bf9" />

---

### Resultado Final

- Você receberá um e-mail com o assunto:  
  **ALARME: "Erro 404"**  

<img width="799" height="41" alt="image" src="https://github.com/user-attachments/assets/a08044b1-48f9-4fff-8ede-cae67067f59a" />

<img width="1029" height="429" alt="image" src="https://github.com/user-attachments/assets/c30661be-8690-41f5-a6be-3ca3898936aa" />

Esse alarme foi criado com base nos dados de log do aplicativo.  
Além do alerta, o arquivo de log permanece acessível no **CloudWatch Logs** para realizar análises adicionais e diagnosticar o que causou os erros.  

[⬆ Voltar ao índice](#índice)

---

### Tarefa 3: Monitorar Métricas de Instância Usando o CloudWatch

Nesta tarefa, vamos usar as métricas que o **CloudWatch** fornece de forma nativa para monitorar sua instância.  

<img width="1240" height="209" alt="image" src="https://github.com/user-attachments/assets/3450fc3b-5c29-48ee-9586-84d20e0c303c" />

### Passo 1: Acessar métricas da instância no EC2

1. No **menu Serviços**, selecione **Elastic Compute Cloud (EC2)**.  

2. No painel de navegação à esquerda, selecione **Instâncias**.  

3. Marque a caixa de seleção ao lado de **Web Server**.  

4. Selecione a guia **Monitoramento** na parte inferior da página.  

5. Examine as métricas apresentadas.  
   - É possível clicar em qualquer gráfico para exibir mais informações.  

<img width="1402" height="681" alt="image" src="https://github.com/user-attachments/assets/27730b20-d0c5-42e6-825d-d94de7685f61" />


💡 Observação:  
O **CloudWatch** captura métricas sobre **CPU, disco e rede** da instância, oferecendo uma visão "externa" da máquina virtual.  
- Não mostra informações internas da instância, como **memória livre** ou **espaço livre em disco**.  
- Para isso, usamos o **CloudWatch Agent**, que coleta métricas diretamente de dentro da instância.

---

### Passo 2: Visualizar métricas no CloudWatch

1. No **menu Serviços**, selecione **CloudWatch**.  

2. No painel de navegação à esquerda, clique em **Métricas**.  

3. Expanda **Métricas** e selecione **Todas as métricas**.  

4. Na metade inferior da página, você verá as várias métricas coletadas:  
   - Algumas são geradas automaticamente pela AWS.  
   - Outras são coletadas pelo **CloudWatch Agent**.

---

### Passo 3: Explorar métricas do CloudWatch Agent

1. Selecione **CWAgent** e escolha: `dispositivo, fstype, host, caminho`.  
   <img width="709" height="116" alt="image" src="https://github.com/user-attachments/assets/174cbee4-9ae6-4174-9ddd-e856ddf5639d" />
   - Você verá as **métricas de espaço em disco** capturadas pelo agente.  

3. Acima da tabela, selecione **CWAgent** (linha **Todos > CWAgent > dispositivo, fstype, host, caminho**).  
4. Selecione **host**.  
   - Agora você verá métricas relacionadas à **memória do sistema**.  
5. Selecione **Todos** (linha **Todos > CWAgent > dispositivo, fstype, host, caminho**) para explorar as demais métricas coletadas.  

💡 Dica:  
- Métricas geradas automaticamente provêm dos serviços AWS usados na conta.  
- É possível selecionar métricas específicas para exibir em um **gráfico** no CloudWatch.  

[⬆ Voltar ao índice](#índice)

---

### Tarefa 4: Criar Notificações em Tempo Real

Nesta tarefa, vamos criar uma notificação em tempo real para informar quando uma instância for **interrompida** ou **encerrada**.

<img width="1241" height="224" alt="image" src="https://github.com/user-attachments/assets/d538a3c4-77ca-4736-881c-f165f585eb1c" />

### Passo 1: Criar uma regra no CloudWatch Events

1. No painel de navegação à esquerda, expanda **Eventos** e selecione **Regras**.  

2. Clique em **Criar regra**.  

---

### Passo 2: Configurar a origem do evento

1. Em **Origem do evento**, defina:  
   - **Nome do serviço**: `Elastic Compute Cloud`  
   - **Tipo de evento**: `Notificação de alteração do estado da instância do EC2`  
2. Marque a caixa **Estado(s) específico(s)**.  
3. No menu suspenso, selecione:  
   - `interrompido`  
   - `encerrado`  

---

### Passo 3: Configurar o destino

1. Na seção **Destinos**, clique em **Adicionar destino**.  
2. No menu suspenso, selecione **Tópico do SNS**.  
3. Em **Tópico**, escolha `Padrão_CloudWatch_Alarmes_Tópico`.  

---

### Passo 4: Configurar detalhes da regra

1. Clique em **Configurar detalhes**.  
2. Em **Nome**, insira: `Instance_Stopped_Terminated`.  
3. Clique em **Criar regra**.  

✅ Resultado:  
A regra agora enviará notificações em tempo real para o **SNS** sempre que uma instância do EC2 for **interrompida** ou **encerrada**, permitindo monitoramento proativo e ações automatizadas.

---

### Configurar uma Notificação em Tempo Real

Além de configurar o **Amazon Simple Notification Service (SNS)** para enviar notificações em tempo real para e-mail ou SMS, nesta tarefa vamos usar 
**e-mail**, pois mensagens SMS requerem abertura de ticket com o AWS Support e tempo de configuração.

---

### Passo 1: Verificar o Tópico SNS

1. No **menu Serviços**, selecione **Simple Notification Service (SNS)**.  
2. No painel de navegação à esquerda, selecione **Tópicos**.  
3. Clique no **link do nome do tópico**.  
   - Você deve ver **uma única assinatura** associada ao seu endereço de e-mail.  
   - Este é o tópico configurado na **Tarefa 2**.

---

### Passo 2: Interromper a instância EC2

1. No **menu Serviços**, selecione **Elastic Compute Cloud (EC2)**.  
2. No painel de navegação à esquerda, selecione **Instâncias**.  
3. Marque a caixa de seleção ao lado de **Web Server**.  
4. Clique em **Estado da instância** > **Interromper instância** > **Interromper**.  

- A instância entrará no estado **Interrompendo**.  
- Após cerca de **1 minuto**, a instância estará **Interrompida**.  

---

### Passo 3: Receber notificação

- Você deve receber um **e-mail** com detalhes sobre a instância que foi interrompida.  
- A mensagem é enviada em **formato JSON**.  

💡 Dica:  
- Para uma mensagem mais legível, é possível criar uma **função do AWS Lambda** acionada pelo **CloudWatch Events**.  
- A função do Lambda poderia **formatar a mensagem** e enviá-la via SNS.  

---

### Resultado

Essa tarefa demonstra como receber **notificações em tempo real** quando houver alterações na infraestrutura da AWS, permitindo monitoramento proativo 
e respostas rápidas.

[⬆ Voltar ao índice](#índice)

---

### Tarefa 5: Monitorar a Conformidade da Infraestrutura

O **AWS Config** permite **avaliar, auditar e verificar** as configurações dos recursos do seu aplicativo.  
- Monitora e grava continuamente registros de configuração dos recursos da AWS.  
- Permite automatizar a avaliação das configurações com base em padrões desejados.  

Com o AWS Config, você pode:  
- Analisar alterações feitas nas configurações e nos relacionamentos entre recursos AWS.  
- Examinar detalhes do histórico de configuração de recursos.  
- Determinar a conformidade geral em relação às diretrizes internas.  
- Simplificar auditoria, análise de segurança, gerenciamento de alterações e solução de problemas.

Nesta tarefa, vamos ativar **regras do AWS Config** para garantir:  
- Conformidade de **marcação** (tags).  
- Conformidade dos volumes do **Amazon Elastic Block Store (EBS)**.

---

### Passo 1: Configuração inicial do AWS Config

1. No **menu Serviços**, escolha **Config**.  
2. Se for exibido o botão **Comece a usar**, clique nele.  
3. Selecione **Próximo** → **Próximo** → **Confirmar**.  
4. Feche a janela **Bem-vindo ao AWS Config**, se aparecer.

---

### Passo 2: Adicionar regra de tags obrigatórias

1. No painel de navegação à esquerda, selecione **Regras**.  
2. Clique em **Adicionar regra**.  
3. Na seção **Regras gerenciadas pela AWS**, no campo de pesquisa, digite `required-tags`.  
4. Selecione o botão ao lado de `required-tags`.  
5. Clique em **Próximo**.  

#### Configurar regra

1. Na página **Configurar regra**, role até **Parâmetros**.  
2. À direita de `tag1Key`, insira: `project` (substituindo qualquer valor existente).  
3. Clique em **Próximo** → **Adicionar regra**.  

> A regra agora procurará recursos sem a tag `project`. Isso pode levar alguns minutos.

---

### Passo 3: Adicionar regra de volumes do EBS

1. Clique em **Adicionar regra**.  
2. Na seção **Regras gerenciadas pela AWS**, no campo de pesquisa, digite `ec2-volume-inuse-check`.  
3. Selecione o botão ao lado de `ec2-volume-inuse-check`.  
4. Clique em **Próximo** → **Próximo** → **Adicionar regra**.  

> Aguarde até que pelo menos uma das regras conclua a avaliação.  
> - Atualize a página, se necessário.  
> - A mensagem **Sem recursos no escopo** indica que o AWS Config ainda está verificando os recursos disponíveis.

---

### Passo 4: Visualizar resultados de auditoria

1. Escolha cada regra para ver os resultados da auditoria.  
2. Em **Recursos dentro do escopo**, selecione **Compatível**.  

Resultados esperados:  
- **required-tags**:  
  - Instância EC2 compatível (Web Server possui tag `project`).  
  - Recursos não compatíveis sem a tag `project`.  
- **ec2-volume-inuse-check**:  
  - Volume compatível (anexado a uma instância).  
  - Volume não compatível (não anexado a nenhuma instância).  

> O AWS Config possui uma **biblioteca ampla de verificações predefinidas**.  
> - É possível criar regras adicionais usando o **Lambda** para validações personalizadas.

[⬆ Voltar ao índice](#índice)

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
