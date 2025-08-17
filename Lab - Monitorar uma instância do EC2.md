## 🧪 Lab - Monitorar uma instância do EC2

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

## Tarefa 1: configurar o Amazon SNS - criar um tópico do SNS e se inscrever nele usando um endereço de e-mail.

1. Acesse o serviço **SNS (Simple Notification Service)** no console da AWS.  
2. Do lado esquerdo, selecione o Menu, escolha **Topics** e depois **Create topic**.
   <img width="1402" height="102" alt="image" src="https://github.com/user-attachments/assets/19980d49-14cd-43e4-8d96-f8c23275773d" />

3. Na página **Create topic**, na seção **Details**, configure as seguintes opções:

- Type: escolha Standard  
- Name: insira **'MyCwAlarm'**

  <img width="1390" height="449" alt="image" src="https://github.com/user-attachments/assets/f4c993b1-66be-43c4-84d6-6473bf39594e" />
  Clique em **Create topic**  

4. Na página de detalhes **MyCwAlarm**, escolha a aba **Subscriptions** e clique em **Create subscription**
   <img width="1143" height="375" alt="image" src="https://github.com/user-attachments/assets/44b7e970-3747-4b05-998c-4742bca5c9e0" />

5. Na página **Create subscription**, na seção **Details**, configure as seguintes opções:

- Topic ARN (ARN do tópico): deixe a opção padrão selecionada.
- Protocol (Protocolo): na lista suspensa, escolha Email.
- Endpoint: insira um endereço de e-mail válido que você possa acessar.
  <img width="1388" height="362" alt="image" src="https://github.com/user-attachments/assets/eed2c885-2275-4302-a26a-89f178190af7" />
