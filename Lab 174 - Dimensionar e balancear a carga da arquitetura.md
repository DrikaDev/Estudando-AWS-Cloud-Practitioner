## 🧪 Lab 174 - Dimensionar e balancear a carga da arquitetura

## Resumo do Desafio

Neste laboratório vamos aprender a usar o **Elastic Load Balancing (ELB)** e o **Amazon EC2 Auto Scaling** para criar uma 
infraestrutura escalável e resiliente.  

- **Elastic Load Balancing (ELB):** distribui automaticamente o tráfego de entrada entre várias instâncias EC2, garantindo
  balanceamento de carga e maior tolerância a falhas.  
- **Auto Scaling:** ajusta automaticamente o número de instâncias EC2 conforme a demanda, aumentando a capacidade em picos de tráfego
  e reduzindo-a em períodos ociosos, ajudando a manter o desempenho e otimizar custos.  

Ao final, teremos uma aplicação capaz de se adaptar dinamicamente à demanda e com alta disponibilidade.  

## Veja abaixo a arquitetura inicial mostrando a infraestrutura da AWS com o **Servidor Web 1** em uma subnet pública.  

<img width="1202" height="688" alt="image" src="https://github.com/user-attachments/assets/9bdaf73a-0d06-4c9b-b4f4-82c37d8a3d31" />

## Veja abaixo a arquitetura final mostrando o **ELB** e as instâncias do **EC2** no grupo do **Auto Scaling** em subnets privadas distribuídas por duas AZ´s.  

<img width="2534" height="1550" alt="image" src="https://github.com/user-attachments/assets/6d69d7f9-22c3-47aa-83ef-ca19d0cfb401" />

## Objetivos

Após concluir o laboratório, seremos capazes de:

- Criar uma **AMI** a partir de uma instância do **EC2**.  
- Criar um **balanceador de carga (ELB)**.  
- Criar um **modelo de execução** e um **grupo de Auto Scaling**.  
- Configurar um grupo de Auto Scaling para dimensionar novas instâncias em **sub-redes privadas**.  
- Criar **alarmes do Amazon CloudWatch** para monitorar o desempenho da infraestrutura.

---

## Tarefa 1: Criar uma AMI para o Auto Scaling

Nesta tarefa, vamos criar uma **AMI** usando o **Web Server 1** existente.  
Essa ação salvará o conteúdo do disco de inicialização para que novas instâncias possam ser iniciadas com conteúdo idêntico.  

1. No **Console da AWS**, na barra Pesquisar, insira e escolha **EC2** para abrir o Console do Amazon EC2.  
2. No painel de navegação à esquerda, localize a seção **Instâncias** e selecione **Instâncias**.  
3. A instância **Web Server 1** será listada. Agora, vamos criar uma AMI com base nessa instância.    
4. Selecione a instância **Web Server 1**.  

   <img width="1417" height="248" alt="image" src="https://github.com/user-attachments/assets/63880022-2e75-484e-99f9-024af83ba0f7" />  

5. Na lista suspensa **Ações**, selecione **Imagem e modelos > Criar imagem** e configure as seguintes opções:  
   - **Nome da imagem:** `Web Server AMI`  
   - **Descrição da imagem (opcional):** `Lab AMI for Web Server`   

  <img width="1411" height="409" alt="image" src="https://github.com/user-attachments/assets/f34e6613-05dd-49d1-b33a-b013044eda19" />

6. Clique em **Criar imagem**.  

A tela de confirmação exibirá o **ID da nova AMI**, que será usado ao iniciar o grupo de **Auto Scaling** posteriormente no laboratório.

---

## Tarefa 2: Criar um balanceador de carga

Nesta tarefa, vamos criar um **balanceador de carga** capaz de distribuir o tráfego entre várias instâncias EC2 e Zonas de Disponibilidade.  

1. No painel de navegação à esquerda, vá em **Balanceamento de carga** e selecione **Balanceadores de carga**.  
2. Clique em **Criar balanceador de carga**.  
3. Na seção **Tipos de balanceador de carga**, para **Application Load Balancer**, selecione **Criar**.  

   <img width="807" height="602" alt="image" src="https://github.com/user-attachments/assets/5063c82f-7576-4bc4-96ca-67743b9a1fb2" />

> O **Application Load Balancer** distribui o tráfego HTTP e HTTPS de entrada em vários destinos, como instâncias do Amazon EC2,
> microsserviços e contêineres, com base nos atributos da solicitação.
> Quando o balanceador de carga recebe uma solicitação de conexão, ele avalia as regras do listener em ordem de prioridade para
> determinar qual regra deve ser aplicada e, se aplicável, seleciona um destino do grupo de destino para a ação da regra.  

<img width="932" height="225" alt="image" src="https://github.com/user-attachments/assets/f9cd978b-6c93-49c7-ba15-5e78b13d0255" />


### Configuração básica
- **Nome do balanceador de carga:** `LabELB`  

<img width="1388" height="153" alt="image" src="https://github.com/user-attachments/assets/95090c75-911a-490f-908c-2fa1e5f56afe" />

### Mapeamento de rede
- **VPC:** selecione a **VPC do laboratório**  
- **Mapeamentos:** selecione as duas Zonas de Disponibilidade listadas  
  - Primeira Zona de Disponibilidade: **Sub-rede pública 1**  
  - Segunda Zona de Disponibilidade: **Sub-rede pública 2**  

<img width="1384" height="305" alt="image" src="https://github.com/user-attachments/assets/2cc581ac-65cc-4681-bc9b-428a4bf004aa" />

### Grupos de segurança
- Clique em **X** do grupo de segurança padrão para removê-lo  
- Na lista suspensa **Grupos de segurança**, selecione **Grupo de segurança da web**

<img width="1383" height="218" alt="image" src="https://github.com/user-attachments/assets/badb4975-f12e-45af-b319-ba1f6c535067" />

### Listeners e roteamento
1. Clique em **Criar grupo de destino** que está em azul.  

<img width="1383" height="239" alt="image" src="https://github.com/user-attachments/assets/dfef0f27-288c-495d-bd6e-b9c201724d27" />

2. Na nova guia **Grupos de destino**, configure:  
   - **Escolher um tipo de destino:** `Instâncias`  
   - **Nome do grupo de destino:** `lab-target-group`  
3. Clique em **Próximo** e depois **Criar grupo de destino**  
4. Feche a guia **Grupos de destino** e volte para a guia **Balanceadores de carga**  
5. Na seção **Listeners e roteamento**, clique no círculo **Atualizar**  
6. Selecione `lab-target-group` na lista suspensa **Selecionar um grupo de destino**  

<img width="537" height="149" alt="image" src="https://github.com/user-attachments/assets/08dd1631-f492-4afe-b30e-e8bc1b42f5a0" />

7. Clique em **Criar balanceador de carga**  
   - Você deverá ver a mensagem: `Balanceador de carga criado com êxito: LabELB`  

<img width="1145" height="94" alt="image" src="https://github.com/user-attachments/assets/50b23ace-c88e-430b-9045-7d39e6f1270d" />

8. Copie o **Nome do DNS** do balanceador de carga e cole em um editor de texto — essas informações serão necessárias posteriormente no laboratório.

---

## Tarefa 3: Criar um modelo de execução

Nesta tarefa, vamos criar um **modelo de execução** para o grupo do **Auto Scaling**.  
Um modelo de execução é utilizado pelo Auto Scaling para iniciar instâncias EC2, especificando informações como AMI, 
tipo de instância, par de chaves, grupo de segurança e discos.  

1. No **Console de Gerenciamento da AWS**, na barra de pesquisa, insira e escolha **EC2**.  
2. No painel de navegação à esquerda, vá em **Modelos de execução**.  
3. Clique em **Criar modelo de execução**.  

### Configuração do modelo de execução

- **Nome do modelo de execução (obrigatório):** `lab-app-launch-template`  
- **Descrição da versão do modelo:** `A web server for the load test app`  
- **Orientação sobre o Auto Scaling:** selecione `Fornecer orientação para me ajudar a configurar um modelo que eu possa usar com o EC2 Auto Scaling`  

<img width="932" height="495" alt="image" src="https://github.com/user-attachments/assets/da454eb9-e417-467e-8bca-73219078974d" />

### Imagem da aplicação e do sistema operacional (AMI)
- Selecione a guia **Minhas AMIs**  
- Confirme que a **WebServerAMI** já está selecionada  

<img width="921" height="493" alt="image" src="https://github.com/user-attachments/assets/9778bf22-c302-44fc-8fd1-2a75ef227d96" />

### Tipo de instância
- Selecione **t3.micro**

<img width="917" height="259" alt="image" src="https://github.com/user-attachments/assets/98f080bb-c71f-4c56-9aa9-10101d29ffd7" />

### Par de chaves (login)
- Confirme que está definido como **Não incluir no modelo de execução**  

<img width="921" height="198" alt="image" src="https://github.com/user-attachments/assets/b39aac7e-0207-45f3-b26a-c140ba57a564" />

> **Observação:** neste laboratório, você não precisará se conectar à instância.  
> O Amazon EC2 usa criptografia de chave pública para login.
> Normalmente, seria necessário criar um par de chaves para acessar a instância.

### Configurações de rede
- Em **Grupo de segurança**, selecione **Grupo de segurança da web**  

<img width="925" height="119" alt="image" src="https://github.com/user-attachments/assets/918f2596-3c30-4e01-a6a2-c488816728cd" />

> É possível passar dados do usuário para executar scripts de configuração na instância, se necessário.

4. Clique em **Criar modelo de execução**  

5. Você deverá ver a mensagem: `lab-app-launch-template criado com êxito`  

<img width="1388" height="77" alt="image" src="https://github.com/user-attachments/assets/3611c0ce-b023-4dd8-9d20-7687dc60295a" />

6. Clique em **Visualizar modelos de execução**

---

## Tarefa 4: Criar um grupo do Auto Scaling

Nesta tarefa, vamos usar o **modelo de execução** para criar um **grupo do Auto Scaling**.  

1. Selecione `lab-app-launch-template` e, na lista suspensa **Ações**, escolha **Criar grupo do Auto Scaling**.  

<img width="1428" height="324" alt="image" src="https://github.com/user-attachments/assets/c7dfbebf-71c9-47fe-81d0-c330db2d61a5" />

2. Na página **Escolher o modelo ou a configuração de execução**, configure:  
   - **Nome do grupo do Auto Scaling:** `Lab Auto Scaling Group`  

<img width="1404" height="344" alt="image" src="https://github.com/user-attachments/assets/f9130506-8385-448d-b343-f8bf7f504822" />

3. Clique em **Próximo**.  

### Opções de execução de instância
- **VPC:** selecione **VPC do laboratório**  
- **Zonas de disponibilidade e sub-redes:** selecione  
  - Sub-rede privada 1 (10.0.1.0/24)  
  - Sub-rede privada 2 (10.0.3.0/24)  

<img width="1062" height="499" alt="image" src="https://github.com/user-attachments/assets/9d3baa4f-d933-456d-8770-3df8dbad31f2" />

- Clique em **Próximo**  

### Integrar com outros serviços (opcional)
- **Balanceamento de carga:** selecione **Anexar a um balanceador de carga existente**  

<img width="1341" height="302" alt="image" src="https://github.com/user-attachments/assets/294151dc-63a7-4348-93c0-526957e95da8" />

- **Grupos de destino de balanceador de carga existentes:** selecione `lab-target-group | HTTP`  

<img width="1061" height="325" alt="image" src="https://github.com/user-attachments/assets/8adeef13-2d0f-4bb7-bf31-bc0c447fa607" />

- **Verificações de integridade:** selecione **ELB**  

<img width="1062" height="326" alt="image" src="https://github.com/user-attachments/assets/7089f5e6-8c4d-4bfa-a487-03ac75fd1f9e" />

- Clique em **Próximo**  

### Configurar tamanho do grupo e ajuste de escala (opcional)
- **Tamanho do grupo:**  
  - Capacidade desejada: 2  

<img width="1346" height="403" alt="image" src="https://github.com/user-attachments/assets/fa599984-d032-4b00-82a2-39d016f40943" />

  - Capacidade mínima: 2  
  - Capacidade máxima: 4  

<img width="1062" height="222" alt="image" src="https://github.com/user-attachments/assets/a0581127-98a8-40cd-b003-81653058e309" />

- **Ajuste de escala automática:**  
  - Selecione **Política de dimensionamento com monitoramento do objetivo**  
  - **Tipo de métrica:** Média de utilização da CPU  
  - **Valor de destino:** 50  

<img width="1062" height="418" alt="image" src="https://github.com/user-attachments/assets/562101ca-a2c8-46a1-9605-979fade8f6e9" />

> Isso garante que o **Auto Scaling** ajuste automaticamente o número de instâncias para manter a utilização média da CPU em 50%,
> acompanhando flutuações de carga.

- Clique em **Próximo**  

### Adicionar notificações (opcional)
- Clique em **Próximo**  

<img width="1080" height="161" alt="image" src="https://github.com/user-attachments/assets/f8e56dc4-1409-424b-a37e-d7009f838e8b" />

### Adicionar tags (opcional)
- Clique em **Adicionar tag** e configure:  
  - **Chave:** `Name`  
  - **Valor:** `Lab Instance`  
- Clique em **Próximo**  

<img width="1079" height="350" alt="image" src="https://github.com/user-attachments/assets/3cb0340f-aa17-4147-aa95-ec1c5d871a19" />

4. Clique em **Criar grupo do Auto Scaling**  

<img width="1353" height="195" alt="image" src="https://github.com/user-attachments/assets/215d1961-3fa2-484d-be9e-85fdd5199b38" />

> As instâncias EC2 serão iniciadas nas sub-redes privadas das duas Zonas de Disponibilidade.  
> O grupo do **Auto Scaling** mostrará inicialmente uma contagem de instâncias igual a zero, mas novas instâncias serão iniciadas para
> atingir a contagem desejada de duas instâncias.

> **Observação:** se houver erro relacionado ao tipo de instância `t3.micro`, repita a tarefa usando `t2.micro`.

---

## Tarefa 5: Verificar se o balanceamento de carga está funcionando

Nesta tarefa, vamos verificar se o **balanceamento de carga** está funcionando corretamente.

1. No painel de navegação à esquerda, vá em **Instâncias**.  
   - Devem aparecer duas novas instâncias chamadas **Instância do laboratório**, iniciadas pelo Auto Scaling.  
   - Se as instâncias ou os nomes não forem exibidos, aguarde 30 segundos e depois atualize a página.  

<img width="1408" height="225" alt="image" src="https://github.com/user-attachments/assets/ad3d9a61-a6f4-4dce-8edf-933d327de9d6" />

2. Confirme que as novas instâncias foram aprovadas na **health check**:  
   - No painel de navegação à esquerda, vá em **Balanceamento de carga > Grupos de destino**.  
   - Selecione `lab-target-group`.  

<img width="1159" height="167" alt="image" src="https://github.com/user-attachments/assets/8f151f6f-1aae-4d99-bd1f-ea1ea1abdee6" />

   - Na seção **Destinos registrados**, devem aparecer dois destinos da **Instância do laboratório**.  
   - Aguarde até que o **Status de integridade** das duas instâncias mude para **Healthy**.  
   - Atualize a página para conferir as atualizações.  

<img width="1219" height="308" alt="image" src="https://github.com/user-attachments/assets/cd4d84f9-522b-4e86-9134-fe9d31d135fb" />

> O **Status de integridade** indica que a instância passou no **health check** do balanceador de carga, garantindo que o tráfego
> será enviado para ela.  

3. Teste o balanceamento de carga:  
   - Abra uma nova guia do navegador.  
   - Cole o **nome do DNS** do balanceador de carga que você copiou anteriormente e pressione **Enter**.  
   - A aplicação **Teste de carga** deve aparecer no navegador, indicando que o balanceador de carga recebeu a solicitação,
     encaminhou para uma das instâncias EC2 e repassou o resultado.

<img width="1195" height="456" alt="image" src="https://github.com/user-attachments/assets/6ebbc4c0-e00b-4af4-b26a-22111138ad8b" />

---

## Tarefa 6: Testar o Auto Scaling

Criamos um **grupo do Auto Scaling** com, no mínimo, duas instâncias e, no máximo, quatro instâncias.  
Atualmente, duas instâncias estão em execução porque o tamanho mínimo é duas e o grupo não está sob carga.  

Nesta tarefa, vamos aumentar a carga para acionar o **Auto Scaling** e adicionar novas instâncias.  

1. No **Console de Gerenciamento da AWS**, feche a guia da aplicação **Teste de carga** temporariamente.  
2. Na barra de pesquisa, insira e escolha **CloudWatch**.  
3. No painel de navegação à esquerda, vá em **Alarmes > Todos os alarmes**.  
   - Dois alarmes devem estar listados, criados automaticamente pelo grupo do Auto Scaling.  
   - Esses alarmes mantêm a carga média da CPU próxima a 50%, garantindo que o número de instâncias fique entre 2 e 4.  

4. Selecione o alarme com **AlarmHigh** no nome.  
   - Verifique se o **Estado** está **OK**.  
   - Se não estiver, aguarde um minuto e atualize a página até que o estado mude.  
   - O estado **OK** indica que o alarme ainda não foi acionado.  

5. Volte para a guia do navegador com a aplicação **Teste de carga**.  
   - Ao lado do logotipo da AWS, selecione **Teste de carga**.  
   - Isso fará a aplicação gerar cargas elevadas, aumentando a utilização da CPU em todas as instâncias do grupo.  

6. Volte para o **CloudWatch** e monitore os alarmes:  
   - Em menos de cinco minutos, o alarme **AlarmLow** deve mudar para **OK** e o status do **AlarmHigh** deve mudar para **Em alarme**.  
   - Atualize a cada 60 segundos para acompanhar o grafo de utilização da CPU.  
   - Quando a CPU média ultrapassar 50% por mais de três minutos, o Auto Scaling adicionará novas instâncias.  

7. Verifique as instâncias adicionais:  
   - No **Console de Gerenciamento da AWS**, vá em **EC2 > Instâncias**.  
   - Agora, mais de duas instâncias chamadas **Instância do laboratório** devem estar em execução, indicativo de que o Auto Scaling respondeu ao alarme.

---

