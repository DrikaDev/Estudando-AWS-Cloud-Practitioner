## 🧪 Lab - Introdução ao Amazon Aurora  

## Tarefa 1: Criar uma instância do Aurora  
Nesta tarefa, vamos criar uma instância de banco de dados (DB) do Aurora.  

Na parte superior do Console de Gerenciamento da AWS, na barra de pesquisa, pesquise e escolha 'RDS'.  
No menu de navegação à esquerda, escolha 'Bancos de dados'.  
Escolha 'Criar banco de dados' e configure as seguintes opções:  

- Em 'Escolher um método de criação de banco de dados', selecione 'Criação padrão'.  
- Em 'Opções de mecanismo', escolha 'Aurora (MySQL Compatible)'.

<img width="1387" height="464" alt="image" src="https://github.com/user-attachments/assets/d4e26cb2-d2c6-411a-b69b-27de6254c192" />

- Em 'Versão do mecanismo', escolha a versão especificada como 'default for major version 8.0'.

<img width="1385" height="158" alt="image" src="https://github.com/user-attachments/assets/bc2990aa-0654-4fe0-bea4-5a84b97cb66a" />

- Em 'Modelos', selecione 'Dev/teste'.

<img width="1384" height="161" alt="image" src="https://github.com/user-attachments/assets/93e21f45-4d40-4098-bb34-3dba3d3b4fb5" />

Na seção 'Configurações', defina as seguintes opções:
- Em 'Identificador do cluster de banco de dados', insira 'aurora'.

<img width="1384" height="187" alt="image" src="https://github.com/user-attachments/assets/aac1d870-e53e-486d-b7ae-2b735f30b9f6" />

- Em 'Nome do usuário principal', insira 'admin'.  
- Em 'Senha principal', insira 'admin-12345'.  
- Em "Confirmar senha", insira 'admin-12345'.  
<img width="1385" height="530" alt="image" src="https://github.com/user-attachments/assets/55de0d6a-9557-4f3f-9b07-78497dde26eb" />

Na seção 'Configuração da instância':
- para a seção 'Classe da instância de banco de dados': escolha 'Classes com capacidade de intermitência (inclui classes t),
e selecione 'db.t3.medium' na lista suspensa.

<img width="1383" height="336" alt="image" src="https://github.com/user-attachments/assets/0cfb7ba3-e793-4d82-acbd-d7e90282add8" />

Na seção 'Disponibilidade e durabilidade':
- para 'Implantação Multi-AZ': selecione 'Não criar uma réplica do Aurora'.
> Como este é um ambiente de laboratório, você não precisa executar uma 'implantação multi-AZ'.

<img width="1385" height="168" alt="image" src="https://github.com/user-attachments/assets/7615be6c-f1cc-4565-8514-f1458fc9556e" />  

Na seção 'Conectividade', configure as seguintes opções e mantenha a valor padrão em todas que não forem mencionadas:  
- Em 'Nuvem privada virtual (VPC)', escolha 'LabVPC'.  
- Em 'Grupo de sub-redes', escolha 'dbsubnetgroup'.

<img width="1382" height="282" alt="image" src="https://github.com/user-attachments/assets/c08553da-988c-45bd-8bcd-b1183827d156" />

- Em 'Acesso público', selecione 'Não'.  
- Em 'Grupo de segurança da VPC existentes', remova o Defalut e escolha 'DBSecurityGroup'.

<img width="1384" height="374" alt="image" src="https://github.com/user-attachments/assets/fd4c71cd-ba8d-4a18-8a34-5d291a9ebaf5" />

> Observação:
> Sub-redes são divisões da sua rede virtual (VPC) para organizar recursos.
> Um grupo de sub-redes de banco de dados é uma coleção dessas sub-redes (geralmente privadas) que você associa às suas instâncias de banco de dados.
> Isso permite que você especifique a VPC e as sub-redes ao criar instâncias de banco de dados, seja via CLI/API ou diretamente pelo console.
> O grupo de sub-redes aurora foi criado para você ao iniciar o laboratório usando o AWS CloudFormation.

## A considerar: 
É possível usar o serviço Amazon Virtual Private Cloud (Amazon VPC) para iniciar recursos da AWS em uma rede virtual definida por você.  
Essa rede virtual é semelhante a uma rede tradicional que você operaria no seu data center, com as vantagens de usar a infraestrutura dimensionável da AWS.  

Na seção 'Monitoramento', desmarque a caixa de seleção de 'Habilitar monitoramento avançado'.  

<img width="1384" height="139" alt="image" src="https://github.com/user-attachments/assets/b5a7b071-c909-4ff9-8631-6f5a23738651" />

Expanda a seção 'Configuração adicional'.  
- Em 'Nome do banco de dados inicial', insira 'world'.

<img width="1381" height="192" alt="image" src="https://github.com/user-attachments/assets/17e593d8-412a-4431-8541-adb194110c19" />

- Na seção 'Backup', desmarque a caixa de verificação de 'Habilitar criptografia'.

<img width="1383" height="203" alt="image" src="https://github.com/user-attachments/assets/c79e684c-4850-4980-936c-bb6a34b75cff" />

## Observação: 
É possível criptografar as instâncias e os snapshots em repouso do Amazon RDS habilitando a opção de criptografia para a instância de banco de dados do RDS.  
Os dados em repouso criptografados incluem o armazenamento subjacente de uma instância de banco de dados, seus backups automatizados, suas réplicas de leitura 
e seus snapshots.  

- Na seção 'Manutenção', desmarque a caixa de seleção de 'Habilitar a atualização automática da versão secundária'.

<img width="1385" height="111" alt="image" src="https://github.com/user-attachments/assets/07d22fb3-f60b-476b-a588-0ea1ba034577" />

Role até a parte inferior da tela e selecione 'Criar banco de dados'.  

> A instância de banco de dados do Aurora está no processo de inicialização e pode levar até cinco minutos para abrir.  

🤩 Tarefa concluída: você criou uma instância do Aurora!  

<img width="1120" height="239" alt="image" src="https://github.com/user-attachments/assets/05c1a6f2-a5ab-40d1-8964-0b78c220408c" />

---

## Tarefa 2: Conectar-se a uma instância do Amazon EC2 Linux  

Nesta tarefa, você fará login na sua instância Windows do Amazon EC2.  
Essa instância foi iniciada para você ao começar o laboratório usando o CloudFormation.  

Na parte superior do Console de Gerenciamento da AWS, na barra de pesquisa, pesquise e escolha EC2.  
No menu de navegação à esquerda, escolha Instâncias.  
Selecione a caixa 'Command Host', e escolha Conectar.  

<img width="1383" height="168" alt="image" src="https://github.com/user-attachments/assets/dae63a7e-0d22-409f-a6b0-dc9df45fb2f3" />  

Em Conectar-se à instância, escolha Gerenciador de sessões.  
Escolha Connect (Conectar) para abrir uma janela do terminal.  

<img width="975" height="90" alt="image" src="https://github.com/user-attachments/assets/869ae908-69f1-440c-9fce-68b0b5402317" />

🤩 Tarefa concluída: você se conectou à instância do Amazon EC2 chamada Command Host (Host de comando).  

--- 

## Tarefa 3: Configurar a instância do Amazon EC2 Linux para conectar-se ao Aurora.  

Nesta tarefa, vamos usar o gerenciador de pacotes yum para instalar o cliente MariaDB.  
Depois vamos configurar a instância do Amazon EC2 Linux para se conectar ao banco de dados Aurora.  

**Comando:** para instalar o cliente MariaDB, execute o comando a seguir.  
```
sudo yum install mariadb -y
```

Volte ao Console de Gerenciamento da AWS, Aurora e RDS, Bancos de dados.  
Aguarde até aurora-instance-1 mostrar 'Disponível'.  
Em 'Segurança e conexão', na seção Endpoints, copie o endpoint da 'Instância do gravador'.  

<img width="1382" height="545" alt="image" src="https://github.com/user-attachments/assets/7bc96aad-c7ff-450b-b787-f0b84981c8a2" />

## Endpoint de cluster:
Um endpoint de cluster de banco de dados do Aurora se conecta à instância de DB primário para esse cluster de banco de dados.  
Este endpoint é o único que pode realizar operações de gravação, como declarações DDL.  
Por isso, o endpoint do cluster é aquele que você se conecta quando configura pela primeira vez um cluster ou quando seu cluster contém apenas uma única 
instância de banco de dados.

O comando a seguir, substitua <endpoint_goes_here> pelo endpoint do aurora:  
```
mysql -u admin --password='admin123' -h <endpoint_goes_here>
```
O comando deve ser semelhante ao seguinte:
mysql -u admin --password='admin123' -h mydbcluster.cluster-123456789012.us-west-2.rds.amazonaws.com  

<img width="1204" height="260" alt="image" src="https://github.com/user-attachments/assets/ce30b067-c91a-4f2b-bde2-05efe280a9fc" />

O cliente da linha de comando do MySQL é um shell de SQL que permite a interação com mecanismos de banco de dados.  
Mais informações estão disponíveis aqui.  

<img width="1050" height="288" alt="image" src="https://github.com/user-attachments/assets/86fa365d-2b48-47e0-937e-fce277d48a7d" />  

🤩 Tarefa concluída: você configurou a instância do Amazon EC2 Linux para se conectar ao Aurora.  

--- 

## Tarefa 4: Criar uma tabela e inserir e consultar registros  

- SHOW DATABASES;  

<img width="285" height="227" alt="image" src="https://github.com/user-attachments/assets/46061e60-1ee2-4df8-b598-b3547c71b9c2" />  

- USE world;  
<img width="243" height="59" alt="image" src="https://github.com/user-attachments/assets/64591358-4335-4783-928d-e78055893c54" />  

- Para criar uma tabela no banco de dados world, execute o comando a seguir.  

```
CREATE TABLE `country` (
`Code` CHAR(3) NOT NULL DEFAULT '',
`Name` CHAR(52) NOT NULL DEFAULT '',
`Continent` enum('Asia','Europe','North America','Africa','Oceania','Antarctica','South America') NOT NULL DEFAULT 'Asia',
`Region` CHAR(26) NOT NULL DEFAULT '',
`SurfaceArea` FLOAT(10,2) NOT NULL DEFAULT '0.00',
`IndepYear` SMALLINT(6) DEFAULT NULL,
`Population` INT(11) NOT NULL DEFAULT '0',
`LifeExpectancy` FLOAT(3,1) DEFAULT NULL,
`GNP` FLOAT(10,2) DEFAULT NULL,
`GNPOld` FLOAT(10,2) DEFAULT NULL,
`LocalName` CHAR(45) NOT NULL DEFAULT '',
`GovernmentForm` CHAR(45) NOT NULL DEFAULT '',
`Capital` INT(11) DEFAULT NULL,
`Code2` CHAR(2) NOT NULL DEFAULT '',
PRIMARY KEY (`Code`)
);
```

- Para inserir novos registros na tabela de países que você acabou de criar, execute os seguintes comandos:  

```
INSERT INTO `country` VALUES ('GAB','Gabon','Africa','Central Africa',267668.00,1960,1226000,50.1,5493.00,5279.00,'Le Gabon','Republic',902,'GA');
INSERT INTO `country` VALUES ('IRL','Ireland','Europe','British Islands',70273.00,1921,3775100,76.8,75921.00,73132.00,'Ireland/Éire','Republic',1447,'IE');
INSERT INTO `country` VALUES ('THA','Thailand','Asia','Southeast Asia',513115.00,1350,61399000,68.6,116416.00,153907.00,'Prathet Thai','Constitutional Monarchy',3320,'TH');
INSERT INTO `country` VALUES ('CRI','Costa Rica','North America','Central America',51100.00,1821,4023000,75.8,10226.00,9757.00,'Costa Rica','Republic',584,'CR');
INSERT INTO `country` VALUES ('AUS','Australia','Oceania','Australia and New Zealand',7741220.00,1901,18886000,79.8,351182.00,392911.00,'Australia','Constitutional Monarchy, Federation',135,'AU');
```

- Para consultar a tabela, execute a seguinte instrução SELECT:  

```
SELECT * FROM country WHERE GNP > 35000 and Population > 10000000; 
```

<img width="1168" height="280" alt="image" src="https://github.com/user-attachments/assets/2e2d5473-3433-4de4-bf62-208c45fb452c" />  

🤩 Tarefa concluída: você criou uma tabela chamada 'country', inseriu dados na tabela e consultou registros que retornam dois resultados.  

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒

