## 🧪 Lab 176 - Roteamento de Failover do Amazon Route 53

## Visão geral do laboratório
Nesta atividade, vamos configurar o **roteamento de failover** para um aplicativo web simples.  

O ambiente da atividade já conta com **duas instâncias do Amazon Elastic Compute Cloud (Amazon EC2)** que possuem a pilha **LAMP** 
instalada, com o site da cafeteria implantado e em execução.  
As instâncias estão distribuídas em **diferentes Zonas de Disponibilidade (AZs)**.  

Por exemplo, se os servidores da web estiverem na **Região us-west-2**:  
- Uma instância estará na **Zona de Disponibilidade us-west-2a** (primária)  
- A outra instância estará na **Zona de Disponibilidade us-west-2b** (secundária)  

## Objetivo
Configurar o domínio de forma que, caso o site na Zona de Disponibilidade **primária** fique indisponível, o **Amazon Route 53** 
faça automaticamente o **failover** do tráfego para a instância na Zona de Disponibilidade **secundária**.  

## Arquitetura final

<img width="1136" height="922" alt="image" src="https://github.com/user-attachments/assets/0061bfec-c8b9-4df2-8c20-a36151a6d451" />

Ao final do laboratório, o ambiente terá a seguinte arquitetura:

- Os **registros do Amazon Route 53** armazenam o **endereço IP** de cada instância do EC2 em suas respectivas Zonas de Disponibilidade.
- As solicitações dos usuários são inicialmente enviadas para o **endereço IP da Instance1** (Zona de Disponibilidade primária).
- Se a **Instance1** ficar indisponível, as solicitações são automaticamente encaminhadas para a **Instance2** (Zona de Disponibilidade
  secundária), de acordo com a configuração de failover do Route 53.
- Quando a **Instance1** estiver indisponível, o **Health Check do Route 53** é acionado e um **alerta por e-mail** é enviado para o
  endereço configurado.

### Fluxo resumido:

1. Usuário acessa o site.
2. Route 53 direciona a requisição para a **Instance1** (AZ primária).
3. Health Check monitora o status da Instance1.
4. Caso a Instance1 falhe:
   - Route 53 redireciona o tráfego para a **Instance2** (AZ secundária).  
   - Um **alerta por e-mail** é enviado informando a indisponibilidade.

---

## Tarefa 1: Confirmar os sites da cafeteria

Nesta tarefa, vamos analisar os recursos que o **AWS CloudFormation** criou automaticamente:

1. **Obter credenciais**
   - Na parte superior da página, selecione **Detalhes**.
   - Em **AWS**, selecione **Mostrar**.
   - Um painel **Credenciais** será aberto.
   - Copie os valores dos seguintes parâmetros em um editor de texto para uso posterior:
     - `CafeInstance1IPAddress`
     - `PrimaryWebSiteURL`
     - `SecondaryWebsiteURL`
     - `CafeInstance2IPAddress`
   - Clique em **X** para fechar o painel Credenciais.

2. **Acessar o Console do EC2**
   - Abra uma nova guia do navegador com o **Console de Gerenciamento da AWS**.
   - Na barra de pesquisa, digite e selecione **EC2**.
   - No painel de navegação à esquerda, em **Instâncias**, selecione **Instâncias**.
   - Verifique que já existem **duas instâncias do EC2**:
     - `CafeInstance1`: em execução na sub-rede pública 1 (us-west-2a)
     - `CafeInstance2`: em execução na sub-rede pública 2 (us-west-2b)

<img width="1177" height="192" alt="image" src="https://github.com/user-attachments/assets/760c5667-8bc1-422e-a450-474e4f503ea0" />

3. **Verificar os sites da cafeteria**
   - Os URLs copiados anteriormente correspondem à aplicação da cafeteria em cada instância.
   - Abra uma nova guia do navegador e cole o valor de **PrimaryWebSiteURL**.
     - A página da aplicação da cafeteria será exibida.
     - Observe as **Informações do servidor**: instância do EC2 e Zona de Disponibilidade.
   - Abra outra guia do navegador e cole o valor de **SecondaryWebsiteURL**.
     - Confirme que a segunda instância possui configurações semelhantes à primeira.

4. **Testar a aplicação**
   - Em qualquer um dos sites, selecione **Menu**.
   - Escolha qualquer item do menu e clique em **Enviar pedido**.
   - A página **Confirmação do pedido** exibirá o horário do pedido no fuso horário do servidor da web.

## Resultado
- Ambas as instâncias estão executando a aplicação da cafeteria.
- Cada instância está em **uma Zona de Disponibilidade diferente**, garantindo **alta disponibilidade**.

## 2a
<img width="974" height="547" alt="image" src="https://github.com/user-attachments/assets/0e9cb287-3066-418c-bc60-4a09dc43f148" />

## 2b
<img width="970" height="547" alt="image" src="https://github.com/user-attachments/assets/fa224a68-e077-420b-adcb-bb88e3af50bd" />

---

## Tarefa 2: Configurar uma Health Check do Route 53

A primeira etapa para configurar o **failover** é criar uma **health check** para o site primário.

1. **Acessar o Route 53**
   - No **Console de Gerenciamento da AWS**, no menu **Serviços**, selecione **Route 53**.
   - É possível **ignorar mensagens de erro** relacionadas a restrições do **AWS IAM**.

2. **Criar Health Check**
   - No painel de navegação à esquerda, selecione **Verificações de integridade**.
   <img width="1428" height="210" alt="image" src="https://github.com/user-attachments/assets/ef43f3ff-5271-4aa1-9b38-d820e9228772" />

   - Clique em **Criar verificação de integridade** e configure os seguintes campos:
     - **Nome:** `Primary-Website-Health`
     - **O que monitorar:** `Endpoint`
     - **Especifique o endpoint por:** `Endereço IP`
     - **Endereço IP:** cole o **Endereço IPv4 público de CafeInstance1** (ou use o valor de `CafeInstance1IPAddress`)
     - **Caminho:** `cafe`
     <img width="1393" height="543" alt="image" src="https://github.com/user-attachments/assets/539a3cb5-7048-4745-a3a5-fe4363cabcae" />

   - Expanda **Configuração avançada**:
     - **Intervalo de solicitações:** `Rápido (10 segundos)`
     - **Limite de falha:** `2`
   - Clique em **Criar verificação de integridade**.
   <img width="1383" height="240" alt="image" src="https://github.com/user-attachments/assets/9fca8ef8-058b-467c-bb6e-3f9e831be4f8" />

3. **Criar um alarme no CloudWatch**
   <img width="1118" height="175" alt="image" src="https://github.com/user-attachments/assets/6c87234d-4c06-4d9c-9a2e-f89b13f40eff" />
   - Em **Especificar métrica e condições**, defina o valor limite e clique em "Próximo":
   <img width="245" height="99" alt="image" src="https://github.com/user-attachments/assets/4079c929-3377-48da-91b8-50836df3ee7c" />

   - Em **Configurações / Notificação**, configure:
     - **Criar alarme:** `Sim`
     - **Enviar notificação para:** `Novo tópico do SNS`
     - **Nome do tópico:** `Primary-Website-Health`
     - **Endereço de e-mail do destinatário:** insira um e-mail válido que você possa acessar
     - Clique em **Criar tópico**.
   <img width="1100" height="568" alt="image" src="https://github.com/user-attachments/assets/b245ca56-5b10-4bc7-9b67-d1b77a1cc239" />

5. **Verificar status da Health Check**
   - O **Route 53** verificará periodicamente se o site está respondendo corretamente.
   - A health check pode levar até **1 minuto** para mostrar o status `Íntegro`.  
   - Volte para **Verificações de integridade** e depois a guia **Monitoramento** para visualizar o status ao longo do tempo.
   - Pode demorar alguns segundos para o gráfico ficar disponível. Clique em **atualizar** se necessário.
   <img width="1414" height="305" alt="image" src="https://github.com/user-attachments/assets/89566167-49f6-4788-8a22-83b3981b2b6a" />

6. **Confirmar assinatura do e-mail**
   - Verifique seu e-mail. Você deverá ter recebido uma mensagem de notificação da AWS.
   - No e-mail, clique em **Confirmar assinatura** para concluir a configuração do alerta por e-mail.

## Resultado
- O **Route 53 Health Check** está configurado para monitorar o site primário.
- Caso o site primário fique indisponível, o sistema enviará notificações para o e-mail configurado e permitirá o **failover** para
  o site secundário.

---

## Tarefa 3: Configurar registros do Route 53

Nesta etapa, vamos criar registros do **Route 53** para a zona hospedada configurando o **roteamento de failover** para o 
aplicativo web.

## Tarefa 3.1: Criar um registro A para o site principal

1. No console do **Route 53**, no painel de navegação à esquerda, selecione **Zonas hospedadas**.
2. Localize e selecione o domínio fornecido:  
   `XXXXXX_XXXXXXXXXX.vocareum.training` (os Xs representam números exclusivos da sua conta AWS).
3. Observe os dois registros existentes:
   - **NS (Nameserver):** lista os servidores de nomes autoritativos da zona hospedada. Não deve ser alterado.  
   - **SOA (Start of Authority):** contém informações sobre o domínio no DNS. Também não deve ser alterado.

<img width="1118" height="363" alt="image" src="https://github.com/user-attachments/assets/8695864c-ae44-4d50-afb6-b28369c28b04" />

4. Clique em **Criar registro** e configure os seguintes campos:
   - **Nome do registro:** `www`
   - **Tipo de registro:** `A` (rotear tráfego para um endereço IPv4 e alguns recursos da AWS)
   - **Valor:** insira o endereço IP de `CafeInstance1IPAddress`
   - **TTL (segundos):** `15`
   - **Política de roteamento:** `Failover`
   - **Tipo de registro de failover:** `Primário`
   - **ID da verificação de integridade:** selecione `Integridade do site principal`
   - **ID do registro:** `FailoverPrimary`
<img width="1387" height="590" alt="image" src="https://github.com/user-attachments/assets/22f40ad6-dd24-4bf9-aa3c-460128e6bbed" />

5. Clique em **Criar registros**.  
   O registro do tipo A criado aparecerá como o terceiro registro na página **Zonas hospedadas**.
   <img width="1105" height="52" alt="image" src="https://github.com/user-attachments/assets/9b8412f1-3dee-4e8c-b78d-16e66ab96cf5" />

---

## Tarefa 3.2: Criar um registro A para o site secundário

1. Clique em **Criar registro** novamente e configure:
   - **Nome do registro:** `www`
   - **Tipo de registro:** `A` (rotear tráfego para um endereço IPv4 e alguns recursos da AWS)
   - **Valor:** insira o endereço IP de `CafeInstance2IPAddress`
   - **TTL (segundos):** `15`
   - **Política de roteamento:** `Failover`
   - **Tipo de registro de failover:** `Secundário`
   - **ID da verificação de integridade:** deixe em branco
   - **ID do registro:** `FailoverSecondary`
<img width="1387" height="613" alt="image" src="https://github.com/user-attachments/assets/97268ad1-1e48-4c7d-aa3a-49dc3fcf4930" />

2. Clique em **Criar registros**.  
   O novo registro do tipo A aparecerá na página **Zonas hospedadas**.
   <img width="1100" height="50" alt="image" src="https://github.com/user-attachments/assets/025ce311-fbc4-496b-aa4e-5f7c9c5832f7" />

## Resultado

- O aplicativo web está configurado para executar **failover automático**.  
- Caso a instância primária (AZ primária) fique indisponível, o tráfego será redirecionado automaticamente para a instância secundária (AZ secundária).

---

## Tarefa 4: Verificar a resolução de DNS

Nesta tarefa, vamos acessar os registros do **DNS** em um navegador para verificar se o **Route 53** está apontando corretamente para 
o site principal:

1. No console do **Route 53**, marque a caixa de seleção de um dos registros do tipo **A**.
2. No painel **Detalhes do registro**, copie o valor de **Nome do registro**.
3. Abra uma nova guia do navegador e cole o **Nome do registro**.
   - Acrescente `/cafe` ao final do URL.
   - O URL final deverá ser algo como:  
     ```
     http://www.XXXXXX_XXXXXXXXXX.vocareum.training/cafe/
     ```
     *(os Xs representam dígitos exclusivos da sua conta AWS)*
4. Carregue a página.
5. Verifique se o **site principal da cafeteria** foi carregado corretamente.
   - Na seção **Informações do servidor** da página, confira a **Região/Zona de Disponibilidade** exibida.

## Resultado

- O **DNS do Route 53** está correto para o site principal da cafeteria.

---

## Tarefa 5: Verificar a funcionalidade do failover

Nesta tarefa, vamos testar se o **Route 53** realiza failover corretamente para o servidor secundário caso ocorra uma falha no primário:

1. **Simular falha na instância primária**
   - No **Console de Gerenciamento da AWS**, selecione **EC2** > **Instâncias**.
   - Selecione **CafeInstance1**.
   - No menu **Estado da instância**, clique em **Interromper instância**.
   - Na janela de confirmação, selecione **Interromper**.
   - A instância primária deixará de funcionar, acionando a **health check do Route 53**.

2. **Verificar health check**
   - No menu **Serviços**, selecione **Route 53**.
   - No painel de navegação à esquerda, clique em **Verificações de integridade**.
   - Selecione **Integridade do site principal** e vá até a guia **Monitoramento**.
   - Aguarde até que o **Status de integridade** seja `Não íntegro`.  
     > Pode levar alguns minutos para que o status seja atualizado. Use o botão **atualizar** se necessário.

3. **Testar failover**
   - Volte para a guia do navegador onde o site `vocareum_XXXXXX_XXXXXXXXXX.training/cafe` está aberto.
   - Atualize a página.
   - Verifique a **Região/Zona de Disponibilidade** exibida na seção **Informações do servidor**.  
     - Ela deve mostrar a **Zona de Disponibilidade secundária** (por exemplo, `us-west-2b` em vez de `us-west-2a`).
   - Agora você está acessando o site fornecido pela **instância CafeInstance2**.

4. **Verificar notificação por e-mail**
   - Você deve ter recebido um e-mail da AWS intitulado:
     ```
     ALARM: Primary-Website-Health-awsroute53-...
     ```
   - O e-mail detalha o que acionou o alarme.
   - Obs.: Pode levar alguns minutos para o e-mail chegar.

## Resultado

- O ambiente da aplicação realiza **failover automático** da Zona de Disponibilidade principal para a secundária caso a instância primária falhe.
- O **Route 53** redireciona o tráfego DNS e envia alertas por e-mail conforme configurado.

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
