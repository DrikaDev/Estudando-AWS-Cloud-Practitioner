## 🧪 Lab 281 - Monitorar uma instância do EC2

## 📝 Introdução

Neste laboratório, vamos criar um alarme com o **CloudWatch** que iniciará quando a instância do Amazon EC2 exceder o limite de utilização de uma unidade de processamento central (CPU) específica.  
Vamos criar uma assinatura usando o **Amazon SNS** que enviará um e-mail se esse alarme disparar.  
Faremos login na instância do EC2 e executaremos um comando de teste de estresse que fará com que a utilização de CPU da instância do EC2 alcance 100%.  

Este teste simula um agente malicioso ganhando controle da instância do EC2 e atingindo o pico da CPU.  
Picos na CPU podem ter diferentes causas, e uma delas é um **malware**!  

## 🎯 Objetivos do Lab

- Criar uma notificação do Amazon SNS  
- Criar um alarme no CloudWatch  
- Testar uma instância do EC2 sob estresse  
- Confirmar o envio de e-mail do Amazon SNS  
- Criar um painel no CloudWatch

## ⚙️ Fundamentos de Registro e Monitoramento

**Registro** e **monitoramento** são técnicas implementadas para alcançar um objetivo comum.  
Eles trabalham juntos para assegurar que as referências de desempenho de um sistema e as diretrizes de segurança sejam sempre atendidas.  

### Registro  
Refere-se a gravar e armazenar eventos de dados em arquivos de log.  
Do ponto de vista de segurança, o registro ajuda administradores a identificar sinais de alerta que poderiam passar despercebidos.  

### Monitoramento  
É o processo de coletar e analisar dados para garantir desempenho ideal.  
O monitoramento detecta acessos não autorizados e alinha o uso dos serviços com a segurança organizacional.  

--- 

> PS:
> O ambiente deste laboratório inclui uma instância do EC2 pré-configurada chamada Stress Test (Teste por estresse) com uma função anexada do AWS Instância and Access Management (IAM) que podemos usar para se conectar via gerente de sessão do AWS Systems Manager.  
> Todos os componentes de back-end, como Amazon EC2, perfis do IAM e alguns serviços da AWS, já foram integrados ao laboratório.  

---

## Tarefa 1: Configurar o Amazon SNS

Nesta tarefa, vamos criar um tópico do SNS e se inscrever nele usando um endereço de e-mail.  
O **Amazon SNS** é um serviço de mensagens totalmente gerenciado para comunicações de aplicativo com aplicativo (A2A) e de aplicativo com pessoa (A2P).  

1. Acesse o serviço **SNS (Simple Notification Service)** no console da AWS.
   
2. Do lado esquerdo, selecione o Menu, escolha **Topics** e depois **Create topic**.  
   <img width="1402" height="102" alt="image" src="https://github.com/user-attachments/assets/19980d49-14cd-43e4-8d96-f8c23275773d" />

3. Na página **Create topic**, na seção **Details**, configure as seguintes opções:  

   - Type: escolha Standard  
   - Name: insira **'MyCwAlarm'**  

  <img width="1390" height="449" alt="image" src="https://github.com/user-attachments/assets/f4c993b1-66be-43c4-84d6-6473bf39594e" />
  Clique em **Create topic**  

4. Na página de detalhes **MyCwAlarm**, escolha a aba **Subscriptions** e clique em **Create subscription**
   <img width="1116" height="421" alt="image" src="https://github.com/user-attachments/assets/31d5e839-df0a-4451-9816-6d617a29c5f7" />

5. Na página **Create subscription**, na seção **Details**, configure as seguintes opções:  

   - Topic ARN (ARN do tópico): deixe a opção padrão selecionada.  
   - Protocol (Protocolo): na lista suspensa, escolha Email.  
   - Endpoint: insira um endereço de e-mail válido que você possa acessar.  
  <img width="1391" height="436" alt="image" src="https://github.com/user-attachments/assets/4e3e0dbe-c128-4d58-91e8-94194a50b535" />

6. Na seção Details, o status deve estar como **Pending confirmation**.  
   Você deve ter recebido uma mensagem de e-mail de Notificação da AWS confirmando a inscrição no endereço de e-mail que você forneceu na etapa anterior.  
   <img width="1111" height="245" alt="image" src="https://github.com/user-attachments/assets/1323e92b-7fb3-4fd4-86e2-fec2f6886f18" />

7. Abra o e-mail que você recebeu com a notificação de inscrição ao Amazon SNS e escolha **Confirm subscription**.  
   <img width="531" height="229" alt="image" src="https://github.com/user-attachments/assets/ef1e8fd5-3e9a-484e-b275-de5534d6bfa0" />

8. Volte ao console de gerenciamento da AWS.  
   No painel de navegação à esquerda, escolha **Subscriptions**.  
   O Status agora deverá estar como **Confirmed**.  
   <img width="129" height="68" alt="image" src="https://github.com/user-attachments/assets/be645b3c-978e-449a-a84a-0df22e199416" />

9. Parabéns! Agora você consegue enviar alertas para o endereço de e-mail associado à assinatura do Amazon SNS.
    
---

## Tarefa 2: Criar um alarme do CloudWatch

Nesta tarefa, vamos criar um **alarme do Amazon CloudWatch** para monitorar a utilização da CPU da instância **EC2 Stress Test**.  
O alarme será configurado para enviar uma notificação ao tópico SNS quando a utilização da CPU ultrapassar **60%**.  

### O que é o Amazon CloudWatch?

O **Amazon CloudWatch** é um serviço de monitoramento e observabilidade projetado para fornecer **dados e insights práticos** que ajudam a:  
- Monitorar aplicativos  
- Responder a alterações de performance  
- Otimizar a utilização de recursos  

O **CloudWatch** coleta **logs, métricas e eventos**, oferecendo uma visão unificada da integridade operacional e visibilidade de recursos, aplicativos e serviços da **AWS** e ambientes **on-premises**.

1. No console de gerenciamento da AWS, insira **Cloudwatch** na barra de pesquisa e o selecione.  
   
2. No painel de navegação à esquerda, escolha a lista suspensa **Metrics** e, depois, **All metrics**.  
   O CloudWatch geralmente leva de 5 a 10 minutos após a criação da instância do EC2 para começar a obter detalhes de métricas.

3. Na página **Metrics** escolha **EC2** e selecione **Per-Instance Metrics**.  
   Nessa página, você poderá ver todas as métricas que estão sendo registradas e a instância do EC2 específica para as métricas.  

4. Marque a caixa de seleção com **CPUUtilization** como 'Metric name' para a instância do EC2 Stress Test.  
   <img width="1278" height="215" alt="image" src="https://github.com/user-attachments/assets/73078242-1981-4bb0-a893-48d8c0d15c5b" />
   Escolha **Select metric**.  

5. Para criar um alarme de métrica:  
   No painel de navegação à esquerda, escolha a lista suspensa **Alarms** e depois **All alarms**.  
   <img width="1408" height="253" alt="image" src="https://github.com/user-attachments/assets/dcceb325-eb45-4b4f-b210-2f0c8659ad5e" />
   Escolha **Create alarm**.  
   
6. Escolha **Select metric**, **EC2** e **Per-Instance Metrics**.  

7. Marque a caixa de seleção com **CPUUtilization** como 'Metric name' para a instância **Stress Test**.  
   <img width="1278" height="215" alt="image" src="https://github.com/user-attachments/assets/ba2d4755-fecd-4d99-9646-7b887e489f01" />
   Escolha **Select metric**.  

8. Na página **Specify metric and conditions** configure as seguintes opções:  
   
- Metric name: insira CPUUtilization  
- InstanceId: deixe a opção padrão selecionada.  
- Statistic: Insira Average  
- Period: na lista suspensa, escolha 1 minuto.  

  <img width="1100" height="592" alt="image" src="https://github.com/user-attachments/assets/f8f0143c-9e47-4050-b402-660d6b05aaa1" />

9. Condições:  
    
- Tipo de limite: escolha Static (Estático).  
- Sempre que CPUUtilization for...: escolha Greater (Maior que) > threshold (limite).  
- que... Defina o valor do limite : insira 60  

  <img width="1099" height="391" alt="image" src="https://github.com/user-attachments/assets/b43ff8a8-4966-4066-b59d-5e6932640882" />
  Selecione **Next**  

10. Na página **Configure actions** / **Notification**, configure as seguintes opções:  

- **Alarm state trigger**: clique em 'In alarm'.  
- **Select an SNS topic**: escolha 'Select an existing SNS topic'.  
- **Send a notification to...**: escolha a caixa de texto e depois 'MyCwAlarm'.  
  <img width="1097" height="405" alt="image" src="https://github.com/user-attachments/assets/d58e1d5a-d7e5-4d16-b6cf-7be597660328" />
  Selecione **Next**  

11. Na página **Add alarm details** / **Name and description**, configure as seguintes opções:  

- **Alarm name**: insira 'LabCPUUtilizationAlarm'  
- **Alarm description**: insira 'CloudWatch alarm for Stress Test EC2 instance CPUUtilization'  
  <img width="719" height="341" alt="image" src="https://github.com/user-attachments/assets/402fc582-b770-4196-9138-18ae4baab004" />  
  Selecione **Next**  

12. Veja novamente a página **Preview and create** e selecione **Create alarm**.

13. Prontinho, criamos um alarme do CloudWatch que inicia um estado de In alarm (No alarme) quando o limite de utilização da CPU excede os 60%.  
    <img width="907" height="529" alt="image" src="https://github.com/user-attachments/assets/1aa67c55-134c-4216-a054-a982415c349b" />

---

## Tarefa 3: Testar o alarme do CloudWatch

Nesta tarefa, vamos fazer login na instância do **EC2 Stress Test** e executar um comando que force a CPU a carregar até 100%.  
Esse aumento de utilização da CPU ativará o alarme do CloudWatch, que fará com que o Amazon SNS envie uma notificação para o e-mail associado com o tópico SNS.  

1. Navegue até à página do console Vocareum e selecione o botão **AWS Details**.  
   Ao lado de EC2InstanceURL, há um link. Copie e cole-o em uma nova guia do navegador.  
   Esse link conectará você à instância do EC2 Stress Test.  

2. Para aumentar manualmente a carga da CPU da instância EC2, execute o seguinte comando:  
```
sudo stress --cpu 10 -v --timeout 400s
```
  Esse comando é executado por 400 segundos, carrega a CPU a 100% e diminui a CPU para 0% após o período alocado.  

3. Navegue até à página do console Vocareum novamente e selecione o botão AWS Details.  
   Copie e cole o texto do URL ao lado de EC2InstanceURL em outra guia do navegador para abrir um segundo terminal para a instância do Stress Test.  
   No terminal, execute o comando a seguir:
```
top
```
   Esse comando mostra o uso da CPU em tempo real.

4. Navegue novamente até o console da AWS onde está com a página Alarms (Alarmes) do CloudWatch aberta.

5. Escolha **LabCPUUtilizationAlarm**.

6. Monitore o grafo enquanto seleciona o botão refresh a cada minuto até que o status do alarme seja **In alarm**.  

   > O status do alarme demora alguns minutos para mudar para **In alarm** e para enviar o e-mail.  
   
   No grafo, você pode ver onde a **CPUUtilization** aumentou acima do limite de 60%.
   
   <img width="903" height="526" alt="image" src="https://github.com/user-attachments/assets/92cd6598-686e-4a44-8389-db070b2126b6" />

8. Acesse o e-mail que você forneceu ao configurar a assinatura do Amazon SNS.
   Deverá haver uma notificação de e-mail da AWS Notifications (Notificações da AWS).

---

## Tarefa 4: Criar um painel do CloudWatch 

Nesta tarefa, vamos criar um **painel do CloudWatch** usando as mesmas métricas utilizadas para **CPUUtilization**, que foram usadas ao longo deste laboratório.  

Os **painéis do CloudWatch** são páginas de início personalizáveis no console do CloudWatch que permitem monitorar seus recursos em uma única visualização.  

Eles possibilitam:

- Monitorar recursos distribuídos em **diferentes regiões**.  
- Criar **visualizações personalizadas** de métricas e alarmes.  
- Facilitar a análise de desempenho e saúde operacional dos seus recursos AWS.  

1. Acesse a seção do CloudWatch no console da AWS e no painel de navegação à esquerda, escolha **Dashboards**.  
   Escolha **Create dashboard**.  

2. Em **Dashboard name** insira 'LabEC2Dashboard' e escolha **Create dashboard**.  
   <img width="1429" height="366" alt="image" src="https://github.com/user-attachments/assets/b2d56b22-245b-49fd-b9bb-44091c280e4c" />

3. Selecione Metrics / Line - next.  

4. Selecione EC2 e escolha Per-Instance Metrics.  

5. Marque a caixa de seleção com **Stress Test** para o 'Instance name' e **CPUUtilization** para o 'Metric name'.  
   <img width="1281" height="311" alt="image" src="https://github.com/user-attachments/assets/1ee863f1-fc81-4a49-8217-ed65952bdd73" />
   Escolha **Create widget** e **Save**.  
   
7. Agora você criou um atalho de rápido acesso para visualizar a métrica **CPUUtilization** para a instância 'Stress Test'.  
   <img width="360" height="308" alt="image" src="https://github.com/user-attachments/assets/88919e21-8a88-4e5d-a4a7-c8709261a1f3" />

---

## ✅ Conclusão

Com este laboratório, entendemos a importância de criar um alarme no **CloudWatch** que é acionado quando a instância de teste de estresse (**Stress Test**) excede um limite específico de utilização de CPU.  

Também configuramos uma assinatura no **Amazon SNS** para envio de e-mails quando o alarme for disparado.  

Além disso, realizamos login na instância do **EC2** e executamos um comando de teste de estresse que elevou a utilização da CPU a 100%.  

Esse experimento simula o que poderia ocorrer caso um **agente malicioso** obtivesse controle da instância EC2 e provocasse picos de CPU.  
Vale destacar que esses picos podem ter diferentes causas, sendo uma delas a presença de **malware**.

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
