## 🧪Lab 160 - Criando e interagindo um servidor de banco de dados RDS

### Índice

[0. Introdução]()  
[1. Criar um Grupo de Segurança para a Instância de Banco de Dados do RDS](#1-criar-um-grupo-de-segurança-para-a-instância-de-banco-de-dados-do-rds)  
[2. Criar um grupo de sub-redes de banco de dados ](#2-criar-um-grupo-de-sub-redes-de-banco-de-dados)  
[3. Criar uma instância de banco de dados do Amazon RDS](#3-criar-uma-instância-de-banco-de-dados-do-amazon-rds)  
[4. Interagindo com o banco de dados](#4-interagindo-com-o-banco-de-dados)  

---

### 0. Introdução

Este laboratório tem como objetivo praticar a criação e utilização de um servidor de banco de dados relacional na nuvem por meio do **Amazon RDS**.  
Durante o processo, vamos aprender a executar uma instância de banco de dados com alta disponibilidade, configurá-la para conexões externas e interagir com ela através de um aplicativo web.  

O **Amazon RDS** simplifica a configuração, operação e escalabilidade de bancos de dados relacionais, oferecendo opções como *Amazon Aurora, Oracle, Microsoft SQL Server, PostgreSQL, MySQL* e *MariaDB*.  
Além disso, gerencia automaticamente tarefas administrativas, permitindo foco no desenvolvimento de aplicações.  

Vamos começar com a seguinte infraestrutura:  
<img width="1067" height="511" alt="image" src="https://github.com/user-attachments/assets/583b9607-eb04-40ec-bd70-1688c7ff81e3" />

No final, teremos a seguinte infraestrutura:  
<img width="1077" height="510" alt="image" src="https://github.com/user-attachments/assets/acf5251b-9d88-48c0-bcbb-f411c72c1858" />

---

### 1. Criar um Grupo de Segurança para a Instância de Banco de Dados do RDS  

Nesta tarefa, vamos criar um **grupo de segurança** para permitir que o servidor web acesse a instância de banco de dados do RDS.  
O grupo de segurança será usado quando você executar a instância de banco de dados.  

1. No **Console de Gerenciamento da AWS**, selecione o menu **Serviços** e escolha **VPC** em *Redes e entrega de conteúdo*.  
2. No painel de navegação à esquerda, clique em **Grupos de segurança**.  
3. Clique em **Criar grupo de segurança** e configure:  
   - **Nome do grupo de segurança:** `DB Security Group`  
   - **Descrição:** `Permit access from Web Security Group`  
   - **VPC:** `Lab VPC` (VPC do laboratório)  

<img width="1412" height="343" alt="image" src="https://github.com/user-attachments/assets/649fd5da-5bbf-475b-a089-5de79f5bd273" />

**Agora vamos adicionar uma regra ao grupo de segurança para permitir solicitações de entrada do banco de dados.**  

4. Na seção **Regras de entrada**, clique em **Adicionar regra** e configure:  
   - **Tipo:** `MySQL/Aurora (3306)`  
   - **Origem:** digite `sg` no campo de pesquisa e selecione **Web Security Group** (Grupo de segurança da web).

<img width="1403" height="226" alt="image" src="https://github.com/user-attachments/assets/8f79e78a-a269-4565-ae3e-40aabdcd96dd" />

Isso configura o grupo de segurança do banco de dados para permitir tráfego de entrada na **porta 3306** de qualquer instância do **EC2** associada ao *Web Security Group*.  

5. Role até a parte inferior da tela e clique em **Criar grupo de segurança**.  

Vamos usar esse grupo de segurança ao iniciar o banco de dados do **Amazon RDS**.  

<img width="1205" height="179" alt="image" src="https://github.com/user-attachments/assets/d5daf43d-0ab8-4c59-9551-2ea9372cc232" />  

[⬆ Voltar ao índice](#índice)

---

### 2. Criar um grupo de sub-redes de banco de dados 

Nesta tarefa, vamos criar um **grupo de sub-redes de banco de dados**, que informa ao **Amazon RDS** quais sub-redes podem ser usadas com o banco de dados.  

Cada grupo de sub-redes de banco de dados requer sub-redes em pelo menos **duas Zonas de Disponibilidade**.  

1. No **Console de Gerenciamento da AWS**, selecione o menu **Serviços** e escolha **RDS** em *Banco de dados*.  

2. No painel de navegação esquerdo, clique em **Grupos de sub-redes**.  

3. Clique em **Criar grupo de sub-redes de banco de dados** e configure:  
   - **Nome:** `DB Subnet Group`  
   - **Descrição:** `DB Subnet Group`  
   - **ID da VPC:** `Lab VPC` (VPC do laboratório)    

<img width="1104" height="447" alt="image" src="https://github.com/user-attachments/assets/e681accf-8ec7-4369-9ad7-b07ff516fa99" />

1. Na seção **Adicionar sub-redes para Zonas de disponibilidade**, clique em **Adicionar** e configure:  
   - **Zona de Disponibilidade 1:** selecione a primeira Zona de Disponibilidade  
   - **Zona de Disponibilidade 2:** selecione a segunda Zona de Disponibilidade  

2. Para **Sub-redes**, clique em **Adicionar** e configure:  
   - Para a primeira Zona de Disponibilidade, selecione `10.0.1.0/24`  
   - Para a segunda Zona de Disponibilidade, selecione `10.0.3.0/24`  

3. Clique em **Criar**.

<img width="1387" height="565" alt="image" src="https://github.com/user-attachments/assets/74fc7b87-bda9-4365-aac9-7d03c8f71fc3" />

Isso adiciona a **sub-rede privada 1 (10.0.1.0/24)** e a **sub-rede privada 2 (10.0.3.0/24)**.  

Você usará esse grupo de sub-redes de banco de dados ao criar o banco de dados na próxima tarefa.  

<img width="1146" height="163" alt="image" src="https://github.com/user-attachments/assets/f18d4d41-01c1-4b42-bcb8-ddc712106747" />

[⬆ Voltar ao índice](#índice)

---

### 3. Criar uma instância de banco de dados do Amazon RDS  

Nesta tarefa, vamos configurar e iniciar uma instância de banco de dados **multi-AZ** do Amazon RDS para **MySQL**.  

As implantações multi-AZ do Amazon RDS proporcionam **alta disponibilidade e durabilidade** para instâncias de banco de dados, sendo ideais para cargas de trabalho de produção.  

Quando você provisiona uma instância multi-AZ, o Amazon RDS cria automaticamente uma instância primária e replica os dados sincronicamente para uma instância de espera em uma **Zona de Disponibilidade diferente**.  

1. No painel de navegação à esquerda, clique em **Bancos de dados**.  

2. Clique em **Criar banco de dados**.  
   - Se aparecer a opção *Switch to the new database creation flow* (Alternar para o novo fluxo de criação de banco de dados), clique nela.  

3. Escolha **Criar banco de dados** e selecione **Criação padrão**.  

4. Na seção **Opções de Mecanismo**, em Tipo de Mecanismo, escolha `MySQL`.  

<img width="1389" height="404" alt="image" src="https://github.com/user-attachments/assets/5ee8a2a0-03be-4fcb-a4ba-48a96bb61fb0" />

5. Em **Versão do Mecanismo**, escolha a versão mais recente.  

<img width="1388" height="136" alt="image" src="https://github.com/user-attachments/assets/e8abd7c6-86ec-498e-8059-9004d090a2f5" />

6. Em **Modelos**, escolha `Desenvolvimento/Teste`.  

<img width="1385" height="154" alt="image" src="https://github.com/user-attachments/assets/f6e351f6-8846-4599-94b6-6b604c72b9a2" />

7. Em **Disponibilidade e Durabilidade**, escolha `Instância de Banco de Dados Multi-AZ`.  

<img width="1387" height="478" alt="image" src="https://github.com/user-attachments/assets/fe7441e9-deed-4e73-a816-4d0a230ad972" />

8. Em **Configurações**:  
- Identificador da instância do banco de dados: `lab-db`  
- Nome de usuário principal: `main`  
- Senha principal: `lab-password`  
- Confirmar senha: `lab-password`  

<img width="1389" height="628" alt="image" src="https://github.com/user-attachments/assets/62c14024-4445-4971-8284-124f5d3eeb57" />

9. Em **Configuração da instância**:  
- Classe: `Classes com capacidade de intermitência (inclui classes t)`  
- Tipo: `db.t3.medium`   

<img width="1389" height="349" alt="image" src="https://github.com/user-attachments/assets/5627ad39-4c1c-4db7-8f3d-52f2512febd9" />

10. Em **Armazenamento**:  
- Selecione Uso geral (SSD) em Tipo de armazenamento: `Finalidade geral (SSD)`  

<img width="1385" height="128" alt="image" src="https://github.com/user-attachments/assets/8518414d-4780-4420-b218-06186e1bcdcc" />

11. Em **Conectividade**:  
- VPC: `Lab VPC` (VPC do laboratório)  

<img width="1387" height="103" alt="image" src="https://github.com/user-attachments/assets/3b4a8841-b3aa-46cf-9c0e-dd89197f76ed" />

12. Grupo de segurança da VPC:  
- Clique em **Escolher existente**  
- Remova o grupo **padrão**  
- Selecione `DB Security Group`

<img width="1390" height="196" alt="image" src="https://github.com/user-attachments/assets/b61e38aa-f5e5-49f8-9fe6-3568fc28b19b" />

13. Em **Monitoramento**:
- Expanda **Configuração de monitoramento adicional**  
- Em *Monitoramento avançado*, desmarque **Habilitar monitoramento avançado**  

<img width="1395" height="126" alt="image" src="https://github.com/user-attachments/assets/50adfb07-0459-42a7-bb65-d4c51d8243f6" />

14. Role para baixo até a seção 'Configuração adicional' e expanda esta opção.  
Em seguida, configure:  
- Nome do banco de dados inicial: `lab`  

15. Em **Backup**:  
- Desmarque **Habilitar backups automatizados**  
> Isso desativará os backups, o que normalmente não é recomendado, mas tornará a implantação do banco de dados mais rápida para este laboratório.

<img width="1387" height="391" alt="image" src="https://github.com/user-attachments/assets/3086a459-aa57-4864-9491-b2f08ebd6fb5" />

16. Role até a parte inferior da tela e clique em **Criar banco de dados**.  
Seu banco de dados será iniciado.  

<img width="1153" height="262" alt="image" src="https://github.com/user-attachments/assets/ccfd4e7c-4d8c-497f-b106-1a113827539c" />

17. Clique em **lab-db** (no próprio link).  
> Aguarde aproximadamente **4 minutos** até que o status mude para **Modificando** ou **Disponível**.  
> O processo de implantação está implantando um banco de dados em duas Zonas de Disponibilidade diferentes.  

<img width="1134" height="193" alt="image" src="https://github.com/user-attachments/assets/d9849bdb-d661-4a2d-ba41-5382f0ba8f4c" />

18. Quando o status estiver **Disponível**, role até a seção **Conectividade e Segurança** e copie o campo Endpoint e cole num editor de testo pois vamos
    usar ele mais adiante no laboratório.  

<img width="1134" height="229" alt="image" src="https://github.com/user-attachments/assets/849ae634-eb30-403f-8d43-ac77e5f8d93d" />

---

### 4. Interagindo com o banco de dados

Nesta tarefa, vamos abrir um **aplicativo web** em execução no servidor da web e configurar para usar o banco de dados.  

1. Vá em EC2, selecione a instância, e copie o endereço do IP público do servidor web selecionando em "Detalhes".  

<img width="1419" height="427" alt="image" src="https://github.com/user-attachments/assets/71952117-b9d0-45ce-9c62-d82d9c7ae70c" />

2. Abra uma nova aba do navegador e cole o endereço IP do servidor web no http:// e pressione Enter.  
A aplicação web será exibida, mostrando informações sobre a instância EC2.  

<img width="1197" height="450" alt="image" src="https://github.com/user-attachments/assets/2faa4d51-e052-45e3-be06-90b7dbfffbb4" />

3. Na parte superior da página da aplicação web, clique no link RDS.  
Defina as seguintes configurações:  
- **Endpoint:** cole o endpoint que você copiou anteriormente em um editor de texto  
- **Banco de dados:** `lab`  
- **Nome do usuário:** `main`  
- **Senha:** `lab-password`  
- Clique em **Enviar**  
  
> Uma mensagem será exibida explicando que a aplicação está executando um comando para copiar informações para o banco de dados.  

<img width="1149" height="417" alt="image" src="https://github.com/user-attachments/assets/6118cb50-cc70-4d04-b5b7-65443782cc7e" />

4. Após alguns segundos, a aplicação exibirá um 'Catálogo de Endereços'.  

<img width="1186" height="368" alt="image" src="https://github.com/user-attachments/assets/5f52a822-d353-451a-b8b7-e7266ffa64ca" />

5. O aplicativo **Catálogo de Endereços** está usando o banco de dados RDS para armazenar informações.  
Teste a aplicação web **adicionando, editando e removendo contatos**.  

![Imagem do WhatsApp de 2025-08-27 à(s) 20 31 49_7cb00592](https://github.com/user-attachments/assets/01da9179-9d68-4f4e-95c0-30cff4d67493)

Observe que os dados estão sendo armazenados no **banco de dados RDS**.  
As informações são **replicadas automaticamente** para a segunda Zona de Disponibilidade, garantindo alta disponibilidade.  

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
