## Introdução ao Amazon EC2  

Este laboratório apresenta uma visão geral básica de como executar, redimensionar, gerenciar e monitorar uma instância do **Amazon EC2**.  

## Índice

- [Sobre o EC2](#sobre-o-ec2)
- [Diagrama de Arquitetura](#diagrama-de-arquitetura)  
- [Tarefa 1: Iniciar sua instância do EC2](#tarefa-1-iniciar-sua-instância-do-ec2)  
- [Tarefa 2: Monitorar a instância](#tarefa-2-monitorar-a-instância)  
- [Tarefa 3: Atualizar o grupo de segurança e acessar o servidor web](#tarefa-3-atualizar-o-grupo-de-segurança-e-acessar-o-servidor-web)  
- [Tarefa 4: Redimensionar a instância (tipo de instância e volume do EBS)](#tarefa-4-redimensionar-a-instância-tipo-de-instância-e-volume-do-ebs)  
- [Tarefa 5: Testar a proteção contra encerramento](#tarefa-5-testar-a-proteção-contra-encerramento)  

## Sobre o EC2

O **Amazon Elastic Compute Cloud (Amazon EC2)** é um serviço da web que fornece capacidade computacional redimensionável na nuvem.  
Ele foi projetado para facilitar a computação em nuvem em escala da web para desenvolvedores.  

A interface de serviço da web simples do Amazon EC2 permite que você obtenha e configure capacidade com o mínimo de esforço.  
Ela oferece **controle completo** de seus recursos de computação e permite a execução no ambiente comprovado da Amazon.  

O Amazon EC2 reduz o tempo necessário para **obter e inicializar novas instâncias de servidor em minutos**, permitindo o rápido 
escalonamento da capacidade para mais ou para menos, de acordo com a evolução dos requisitos de computação.  

Além disso, o Amazon EC2 altera a economia da computação, permitindo que você **pague somente pela capacidade que realmente utiliza**.  
Ele fornece ferramentas para os desenvolvedores criarem aplicações resistentes a falhas e isoladas de situações comuns de falha.  

## Diagrama de Arquitetura  

<img width="602" height="487" alt="image" src="https://github.com/user-attachments/assets/859ce258-a719-4bb7-a846-bebdf98e9e27" />

---

## Tarefa 1: Iniciar sua instância do EC2

Nesta tarefa, vamos iniciar uma instância do **Amazon EC2** com **proteção contra encerramento**.  
A proteção contra encerramento impede que você encerre acidentalmente uma instância do EC2.  

Vamos implantar a instância com um **script de dados do usuário**, que permitirá implantar um servidor web simples.

---

### Acessando o Console

1. No **Console de Gerenciamento da AWS**, no menu **Serviços**, selecione **EC2**.  
2. No painel de navegação da esquerda, selecione **Painel do EC2** para garantir que você esteja na página inicial.  
3. Clique em **Executar instância** e selecione **Executar instância**.

<img width="624" height="252" alt="image" src="https://github.com/user-attachments/assets/2f3cd50b-125a-41fc-a317-f85af158f98b" />

---

### Etapa 1: Nomear sua instância do EC2

- Quando você nomear sua instância, a AWS criará um par de chave-valor.  
- A chave desse par é **Nome**, e o valor é o nome que você inseriu para sua instância do EC2.  

👉 No painel **Name and tags (Nome e tags)**, na caixa de texto **Name (Nome)**, digite: `Web Server` 

<img width="938" height="215" alt="image" src="https://github.com/user-attachments/assets/b0dc8d60-5311-42e7-8587-f14b79c07beb" />


---

### Etapa 2: Selecionar uma AMI (Amazon Machine Image)

Uma **AMI** fornece as informações necessárias para iniciar uma instância (um servidor virtual na nuvem).  

Ela inclui:  
- Um **modelo** para o volume-raiz (SO ou servidor de aplicações).  
- **Permissões de execução** que controlam quais contas podem usar a AMI.  
- Um **mapeamento de dispositivos de blocos** para especificar volumes anexados.  

👉 No menu **Quick Start (Início rápido)**, observe que a imagem **Amazon Linux 2023** já está selecionada.  
Mantenha essa configuração.

<img width="922" height="421" alt="image" src="https://github.com/user-attachments/assets/a1b55954-3554-4101-bec6-234a5c8e82a0" />

---

### Etapa 3: Selecionar um tipo de instância

O Amazon EC2 oferece diversos tipos de instância com diferentes combinações de **CPU, memória, armazenamento e rede**.  

👉 Para este laboratório:  
- Selecione **t3.micro** (2 vCPUs e 1 GiB de memória).  

<img width="926" height="232" alt="image" src="https://github.com/user-attachments/assets/25bfd761-d42d-41b6-9806-97a620e37749" />

---

### Etapa 4: Configurar um par de chaves

O EC2 usa **criptografia de chave pública** para login.  
Neste laboratório, não será necessário login.  

👉 Em **Key pair (login)**, selecione: `Proceed without a key pair (Not recommended)`  

<img width="926" height="178" alt="image" src="https://github.com/user-attachments/assets/7b0dfbbe-3b6c-456e-b42e-b1695212e62a" />

---

### Etapa 5: Definir configurações de rede

A **VPC** define em qual nuvem privada virtual sua instância será criada.  

👉 No painel **Configurações de rede**:  
- Clique em **Editar**.  

<img width="941" height="103" alt="image" src="https://github.com/user-attachments/assets/e77a3032-b160-4726-b698-64f0e761efd6" />

- Em **VPC (obrigatório)**, selecione **Lab VPC (VPC do laboratório)**.  

<img width="925" height="75" alt="image" src="https://github.com/user-attachments/assets/169127ff-593d-457f-bb17-d3093aea5ad3" />

Agora configure o **grupo de segurança**:  
- Nome: `Web Server security group`  
- Descrição: `Security group for my web server`  

<img width="924" height="153" alt="image" src="https://github.com/user-attachments/assets/7d6a19f7-aa1a-4153-bb4b-b2f5c64965d3" />

Um grupo de segurança funciona como um **firewall virtual**, controlando tráfego de entrada e saída.  

👉 Em **Regras do grupo de segurança de entrada**, selecione **Remover**.  
*(Não será feito login via SSH para reforçar a segurança.)*

<img width="923" height="62" alt="image" src="https://github.com/user-attachments/assets/918d8c72-6109-4dae-84b1-61a890f298b5" />

---

### Etapa 6: Adicionar armazenamento

O Amazon EC2 armazena dados no **Amazon EBS (Elastic Block Store)**.  

👉 Neste laboratório, mantenha a configuração padrão de:  
- Volume raiz: **8 GiB**

<img width="925" height="102" alt="image" src="https://github.com/user-attachments/assets/bef7856f-307c-4ce6-963a-2c982b061f40" />

---

### Etapa 7: Configurar detalhes avançados

1. Expanda o painel **Advanced details (Detalhes avançados)**.  

2. Em **Termination protection (Proteção contra encerramento)**, selecione **Enable (Habilitar)**.  

<img width="927" height="62" alt="image" src="https://github.com/user-attachments/assets/c5a2829c-a840-4ef6-9b4a-ffa93e75ca2f" />

3. Em **User data (Dados do usuário)**, cole o script abaixo:  

```bash
#!/bin/bash
yum -y install httpd
systemctl enable httpd
systemctl start httpd
echo '<html><h1>Hello From Your Web Server!</h1></html>' > /var/www/html/index.html
```

O que o script faz o seguinte:
- Instala o servidor web Apache (httpd).
- Configura o Apache para iniciar automaticamente.
- Inicia o servidor web.
- Cria uma página HTML simples: `<html><h1>Hello From Your Web Server!</h1></html>`

---

### Etapa 8: Iniciar uma instância do EC2

Agora que você configurou sua instância do EC2, é hora de iniciá-la.

1. No painel direito, selecione **Executar instância**.  

<img width="1408" height="71" alt="image" src="https://github.com/user-attachments/assets/28beba3d-7a86-4a8f-8258-837ca8b36e48" />

2. Selecione **Visualizar todas as instâncias**.  

A instância aparece em um estado **Pendente**, o que significa que está sendo iniciada.  
Depois, o estado muda para **Em execução**, indicando que a instância começou sua inicialização.  

<img width="1204" height="143" alt="image" src="https://github.com/user-attachments/assets/0b457702-af3b-4f20-9bca-d51898dd389f" />

👉 A instância recebe um **nome DNS público**, que pode ser usado para acessá-la pela internet.  

- Marque a caixa ao lado do seu **Web Server**.  
- A guia **Details (Detalhes)** exibe informações detalhadas sobre sua instância.  
- Para visualizar mais informações, arraste o divisor da janela para cima.  

Analise as informações exibidas nas guias **Details (Detalhes)**, **Security (Segurança)** e **Networking (Rede)**.  

✅ Aguarde até que a instância exiba:  
- **Estado da instância:** Em execução  
- **Verificações de status:** 2/2 verificações aprovadas  

---

## Tarefa 2: Monitorar a instância

O monitoramento é essencial para manter a **confiabilidade, disponibilidade e desempenho** das instâncias EC2.  

1. Selecione a instância e vá até a guia **Status checks (Verificações de status)**.  
   - Aqui você pode verificar rapidamente se há problemas de hardware ou software.  
   - O EC2 realiza verificações automáticas de acessibilidade do sistema e da instância.  

✅ Certifique-se de que:  
- **System reachability (Acessibilidade do sistema):** aprovado  
- **Instance reachability (Acessibilidade da instância):** aprovado  

2. Vá até a guia **Monitoring (Monitoramento)**.  
   - Exibe métricas do **Amazon CloudWatch**.  
   - Por padrão, está ativado o monitoramento básico (a cada 5 minutos).  
   - É possível ativar monitoramento detalhado (a cada 1 minuto).  

3. No menu **Ações > Monitorar e solucionar problemas**, selecione:  
   - **Get Instance Screenshot (Obter captura de tela da instância)**.  

<img width="1195" height="414" alt="image" src="https://github.com/user-attachments/assets/5e3ed47d-5d8f-4260-8747-3cbd6e2bfd70" />

Isso mostra como seria o console gráfico da sua instância.  

<img width="1419" height="176" alt="image" src="https://github.com/user-attachments/assets/d50b2448-1795-453d-b2f2-147e728bca5f" />

Se não for possível acessá-la via SSH ou RDP, essa captura pode ajudar na solução de problemas.  

👉 Clique em **Cancelar** para sair da visualização.  

🎉 Parabéns! Você explorou diferentes maneiras de monitorar sua instância.

---

## Tarefa 3: Atualizar o grupo de segurança e acessar o servidor web

Você instalou um servidor web com um script, mas ainda não consegue acessá-lo.  

🔎 Motivo: o **grupo de segurança** não permite tráfego de entrada na porta **80 (HTTP)**.  

### Corrigindo isso:

1. No **EC2 Management Console**, vá até **Security Groups (Grupos de segurança)**.  

2. Selecione **Web Server security group**.  

<img width="1429" height="304" alt="image" src="https://github.com/user-attachments/assets/dfc5100e-fc01-4844-8f16-0e59ae0f0a20" />

3. Vá até a guia **Inbound rules (Regras de entrada)**.  

4. Clique em **Editar regras de entrada 

<img width="1203" height="280" alt="image" src="https://github.com/user-attachments/assets/51b22005-cf0a-4656-ae7d-dd7d314d2e6c" />

- Adicionar regra**  
   - **Tipo:** HTTP  
   - **Origem:** IPv4 – Anywhere (Qualquer lugar)  

<img width="1426" height="371" alt="image" src="https://github.com/user-attachments/assets/19a2f228-b6a5-4297-a639-094253771e02" />

5. Clique em **Salvar regras**.  

<img width="1209" height="266" alt="image" src="https://github.com/user-attachments/assets/81c70c97-8768-414d-a693-0af72844d82f" />

Agora, volte à aba do navegador com o endereço **IPv4 público** da instância e atualize a página.  

👉 Você deve ver a mensagem `Hello From Your Web Server!`  

<img width="1270" height="104" alt="image" src="https://github.com/user-attachments/assets/5e6ca2de-ca5d-4fb8-8f83-bb740f734c7a" />

🎉 Parabéns! O grupo de segurança foi atualizado corretamente.

---

## Tarefa 4: Redimensionar a instância (tipo de instância e volume do EBS)

Se sua instância estiver **superutilizada ou subutilizada**, é possível redimensioná-la.

### Passo 1: Interromper a instância

1. Selecione a instância Web Server.  

2. Clique em **Estado da instância > Interromper instância > Interromper**.  

<img width="1422" height="217" alt="image" src="https://github.com/user-attachments/assets/6d005928-47c4-425b-9cbb-c477a57e594a" />

3. Aguarde até o status mudar para **Stopped (Interrompida)**.  

---

### Passo 2: Alterar o tipo de instância

1. No menu **Ações > Configurações de instância > Alterar tipo de instância**.  

<img width="1208" height="283" alt="image" src="https://github.com/user-attachments/assets/70f3df04-8988-4701-99bb-b31ef6062e97" />

2. Selecione: **t3.small** (o dobro de memória em relação à t3.micro).  

<img width="1401" height="281" alt="image" src="https://github.com/user-attachments/assets/666c9ca8-e946-45a4-891d-1e74297bec7b" />

3. Clique em **Alterar tipo de instância**.

---

### Passo 3: Redimensionar o volume EBS

1. No painel de navegação esquerdo, selecione **Volumes** em **Elastic Block Store**.  

2. Selecione o volume associado à instância.  

3. Vá em **Ações > Modificar volume**.  

<img width="1423" height="162" alt="image" src="https://github.com/user-attachments/assets/8e88e780-970f-454e-b188-9a6f84aac3b2" />

   - Altere de **8 GiB** para **10 GiB**.  

<img width="1413" height="362" alt="image" src="https://github.com/user-attachments/assets/76c71112-2551-4b54-8655-8299f9966a1e" />

4. Clique em **Modificar > Confirmar**.  

---

### Passo 4: Reiniciar a instância

1. Volte para **Instâncias**.  

2. Selecione sua instância Web Server.  

3. Vá em **Estado da instância > Iniciar instâncias**.  

<img width="1207" height="188" alt="image" src="https://github.com/user-attachments/assets/f0855eee-cb9f-44d1-bcb3-88612d7691fc" />

🎉 Parabéns! Você redimensionou a instância de **t3.micro para t3.small** e o volume de **8 GiB para 10 GiB**.

---

## Tarefa 5: Testar a proteção contra encerramento

Por padrão, ao encerrar uma instância **baseada em EBS**, o volume raiz é excluído.  

Neste laboratório, você ativou a **proteção contra encerramento**.  

1. Selecione a instância Web Server.  

2. Vá em **Estado da instância > Encerrar instância**.  

3. Note que a instância não será encerrada — aparece uma mensagem de erro.  

<img width="1209" height="187" alt="image" src="https://github.com/user-attachments/assets/d1144048-f1a2-47a9-a5bb-75ad25580077" />

👉 Isso ocorre porque a **proteção contra encerramento** está ativada.  

### Desativando a proteção:

1. Vá em **Ações > Configurações de instância > Change termination protection**.  

<img width="1208" height="178" alt="image" src="https://github.com/user-attachments/assets/b638c9fc-1832-4b0c-8afb-4b599f6e674d" />

2. Desmarque **Enable (Ativar)** e clique em **Save (Salvar)**.  

<img width="545" height="228" alt="image" src="https://github.com/user-attachments/assets/1d2ab9d4-2836-4e5c-8e7b-1200155bf270" />

<img width="1202" height="38" alt="image" src="https://github.com/user-attachments/assets/4fe091ee-a72b-4bcb-bfa2-5a257b641a9b" />

Agora, você pode encerrar a instância:  
- Vá em **Estado da instância > Terminate instance**.  
- Clique em **Encerrar**.  

<img width="1206" height="185" alt="image" src="https://github.com/user-attachments/assets/e1f54063-bb5d-4aa8-b127-78c95b1c5a9b" />

🎉 Parabéns! Você testou com sucesso a proteção contra encerramento.

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
