🧪Lab - Criando um servidor de banco de dados RDS

Neste lab vamos criar um banco de dados que atenda a seguinte estrutura:

<img width="1077" height="510" alt="image" src="https://github.com/user-attachments/assets/acf5251b-9d88-48c0-bcbb-f411c72c1858" />

---

### 1. Criar um Grupo de Segurança para a Instância de Banco de Dados RDS

No Console de Gerenciamento da AWS, selecione o menu Serviços e, em seguida, VPC.
No painel de navegação esquerdo, clique em Grupos de Segurança.
Clique em Criar grupo de segurança e configure:
- Nome do grupo de segurança: Grupo de Segurança de Banco de Dados
- Descrição: Permitir acesso do Grupo de Segurança Web
- VPC: VPC de Laboratório

<img width="1412" height="343" alt="image" src="https://github.com/user-attachments/assets/649fd5da-5bbf-475b-a089-5de79f5bd273" />

Agora vamos adicionar uma regra ao grupo de segurança do banco de dados para permitir conexões de entrada.  
Como o grupo de segurança ainda não possui regras, criaremos uma regra que permita acesso somente a partir do Grupo de Segurança Web.  

Na seção Regras de entrada, clique em Adicionar regra e configure:  
- Digite: MySQL/Aurora (3306)  
- Origem: Digite sg no campo de pesquisa e selecione Grupo de Segurança Web.  
Isso configura o grupo de segurança do Banco de Dados para permitir tráfego de entrada na porta 3306 de qualquer instância do EC2 associada ao
Grupo de Segurança da Web.  
Role até a parte inferior da tela e clique em Criar grupo de segurança.

<img width="1403" height="226" alt="image" src="https://github.com/user-attachments/assets/8f79e78a-a269-4565-ae3e-40aabdcd96dd" />

Usaremos este grupo de segurança ao iniciar o banco de dados Amazon RDS.  

<img width="1205" height="179" alt="image" src="https://github.com/user-attachments/assets/d5daf43d-0ab8-4c59-9551-2ea9372cc232" />

---

### 2. Criar grupo de sub-redes

Aqui precisamos criar um grupo de sub-redes de banco de dados que será usado pelo RDS para identificar quais sub-redes privadas podem hospedar a
instância do banco de dados.  
Cada grupo de sub-redes requer, no mínimo, sub-redes privadas em duas Zonas de Disponibilidade distintas.  

No Console de Gerenciamento da AWS, selecione o menu Serviços e, em seguida, RDS em Banco de Dados.  
No painel de navegação esquerdo, clique em Grupos de sub-redes.  
Clique em Criar Grupo de Sub-redes de BD e configure:  
Nome: Grupo de Sub-redes de BD  
Descrição: Grupo de Sub-redes de BD  
ID da VPC: VPC de Laboratório  

<img width="1104" height="447" alt="image" src="https://github.com/user-attachments/assets/e681accf-8ec7-4369-9ad7-b07ff516fa99" />

Na seção Adicionar sub-redes:  
Em Zonas de Disponibilidade, selecione:  
- a primeira Zona de Disponibilidade   
- a segunda Zona de Disponibilidade  

Em Sub-redes, selecione:  
- para a primeira AZ, selecione a sub-rede privada 10.0.1.0/24  
- para a segunda AZ, selecione a sub-rede privada 10.0.3.0/24  
Clique em Create

<img width="1387" height="565" alt="image" src="https://github.com/user-attachments/assets/74fc7b87-bda9-4365-aac9-7d03c8f71fc3" />

Grupo de Sub-rede criada:  

<img width="1146" height="163" alt="image" src="https://github.com/user-attachments/assets/f18d4d41-01c1-4b42-bcb8-ddc712106747" />

---

### 3. Criar uma Instância de Banco de Dados do Amazon RDS
Vamos configurar e iniciar uma instância de banco de dados Multi-AZ do Amazon RDS para MySQL.  
Ao provisionar uma instância de Banco de Dados Multi-AZ, o Amazon RDS cria automaticamente uma instância de Banco de Dados primária e replica os dados
de forma síncrona para uma instância em espera em uma Zona de Disponibilidade (AZ) diferente.  

No painel de navegação esquerdo, clique em Bancos de Dados.  
Clique em Criar Banco de Dados.  
Escolha Criação Padrão.  
Na seção Opções de Mecanismo, em Tipo de Mecanismo, escolha MySQL.  

<img width="1389" height="404" alt="image" src="https://github.com/user-attachments/assets/5ee8a2a0-03be-4fcb-a4ba-48a96bb61fb0" />

Em Versão do Mecanismo, escolha a versão mais recente.  

<img width="1388" height="136" alt="image" src="https://github.com/user-attachments/assets/e8abd7c6-86ec-498e-8059-9004d090a2f5" />

Em Modelos, escolha Desenvolvimento/Teste.  

<img width="1385" height="154" alt="image" src="https://github.com/user-attachments/assets/f6e351f6-8846-4599-94b6-6b604c72b9a2" />

Em Disponibilidade e Durabilidade, escolha Instância de Banco de Dados Multi-AZ.  

<img width="1387" height="478" alt="image" src="https://github.com/user-attachments/assets/fe7441e9-deed-4e73-a816-4d0a230ad972" />

Em Configurações, configure o seguinte:  
- Identificador da instância do banco de dados: lab-db  
- Nome de usuário mestre: main  
- Senha mestre: lab-password  
- Confirmar senha: lab-password

<img width="1389" height="628" alt="image" src="https://github.com/user-attachments/assets/62c14024-4445-4971-8284-124f5d3eeb57" />

Em Configuração da instância, configure o seguinte para a classe da instância do banco de dados:  
- Selecione Classes com capacidade de expansão (inclui classes t)  
- Selecione db.t3.medium  

<img width="1389" height="349" alt="image" src="https://github.com/user-attachments/assets/5627ad39-4c1c-4db7-8f3d-52f2512febd9" />

Em Armazenamento, configure:  
- Selecione Uso geral (SSD) em Tipo de armazenamento  

<img width="1385" height="128" alt="image" src="https://github.com/user-attachments/assets/8518414d-4780-4420-b218-06186e1bcdcc" />

Em Conectividade, configure:  
- Nuvem privada virtual (VPC): Lab VPC

<img width="1387" height="103" alt="image" src="https://github.com/user-attachments/assets/3b4a8841-b3aa-46cf-9c0e-dd89197f76ed" />

Em Grupo de segurança da VPC, selecione Escolher existente  
E em Grupos de segurança da VPC existente, remova o default e selecione Grupo de segurança do banco de dados que foi criado  

<img width="1390" height="196" alt="image" src="https://github.com/user-attachments/assets/b61e38aa-f5e5-49f8-9fe6-3568fc28b19b" />

Em Monitoramento, expanda Configuração de monitoramento adicional e configure o seguinte:  
- Para Monitoramento avançado, desmarque 'Habilitar monitoramento avançado'  

<img width="1395" height="126" alt="image" src="https://github.com/user-attachments/assets/50adfb07-0459-42a7-bb65-d4c51d8243f6" />

Role para baixo até a seção 'Configuração adicional' e expanda esta opção.  
Em seguida, configure:  
- Nome inicial do banco de dados: lab  
- Em Backup, desmarque 'Habilitar backups automatizados'  
Isso desativará os backups, o que normalmente não é recomendado, mas tornará a implantação do banco de dados mais rápida para este laboratório.

<img width="1387" height="391" alt="image" src="https://github.com/user-attachments/assets/3086a459-aa57-4864-9491-b2f08ebd6fb5" />

Role até a parte inferior da tela e clique em Criar banco de dados.
Seu banco de dados será iniciado.

<img width="1153" height="262" alt="image" src="https://github.com/user-attachments/assets/ccfd4e7c-4d8c-497f-b106-1a113827539c" />

Clique no link do lab-db.  
Agora precisamos aguardar aproximadamente 4 minutos para que o banco de dados esteja disponível.  
O processo de implantação está implantando um banco de dados em duas Zonas de Disponibilidade diferentes.  

Aguarde até que o status mude para 'Modificando' ou 'Disponível'.  

<img width="1134" height="193" alt="image" src="https://github.com/user-attachments/assets/d9849bdb-d661-4a2d-ba41-5382f0ba8f4c" />

Role até a seção Conectividade e Segurança e copie o campo Endpoint.  

<img width="1134" height="229" alt="image" src="https://github.com/user-attachments/assets/849ae634-eb30-403f-8d43-ac77e5f8d93d" />

---

### 4. Interaja com seu banco de dados

Agora vamos abrir uma aplicação web em execução no seu servidor web e a configurará para usar o banco de dados.  
Vá em EC2, selecione a instância, e copie o endereço do IP público do servidor web selecionando em "Detalhes".

<img width="1419" height="427" alt="image" src="https://github.com/user-attachments/assets/71952117-b9d0-45ce-9c62-d82d9c7ae70c" />

Abra uma nova aba do navegador e cole o endereço IP do servidor web no http:// e pressione Enter.  
A aplicação web será exibida, mostrando informações sobre a instância EC2.  

<img width="1197" height="450" alt="image" src="https://github.com/user-attachments/assets/2faa4d51-e052-45e3-be06-90b7dbfffbb4" />

Na parte superior da página da aplicação web, clique no link RDS.  
Defina as seguintes configurações:  
- Endpoint: Cole o Endpoint  
- Banco de Dados: lab  
- Nome de Usuário: main  
- Senha: lab-password  
- Clique em Enviar
  
> Uma mensagem será exibida explicando que a aplicação está executando um comando para copiar informações para o banco de dados.  

<img width="1149" height="417" alt="image" src="https://github.com/user-attachments/assets/6118cb50-cc70-4d04-b5b7-65443782cc7e" />

Após alguns segundos, a aplicação exibirá um 'Catálogo de Endereços'.  

<img width="1186" height="368" alt="image" src="https://github.com/user-attachments/assets/5f52a822-d353-451a-b8b7-e7266ffa64ca" />

O aplicativo 'Catálogo de Endereços' está usando o banco de dados RDS para armazenar informações.  
Teste a aplicação web adicionando, editando e removendo contatos.  

![Imagem do WhatsApp de 2025-08-27 à(s) 20 31 49_7cb00592](https://github.com/user-attachments/assets/01da9179-9d68-4f4e-95c0-30cff4d67493)

Os dados estão sendo persistidos no banco de dados e replicados automaticamente para a segunda Zona de Disponibilidade.  

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
