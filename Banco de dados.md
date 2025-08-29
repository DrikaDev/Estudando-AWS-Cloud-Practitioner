## 🎲 O que é um Banco de Dados? (EM CONSTRUÇÃO!!!!!!!!!!!)

Um **banco de dados** é como um **grande armário organizado para guardar informações**, permitindo que você:

- **Armazene** dados (guardar).
- **Consulte** dados (procurar).
- **Atualize** dados (alterar).
- **Exclua** dados (remover).

---

## Exemplo simples
Imagine que você tem uma lista de contatos no celular.

- **Nome**: Ana  
- **Telefone**: (11) 91234-5678  
- **Email**: ana@email.com  

Essa lista é um **banco de dados em versão simples**, pois guarda informações organizadas para consulta.

---

## Tipos de Banco de Dados

### 1. Relacional (SQL)
- Organiza dados em **tabelas** (linhas e colunas, como no Excel).
- Usa **SQL (Structured Query Language)** para consultas.
- Exemplos: MySQL, PostgreSQL, Oracle, SQL Server.

**Exemplo de tabela "Clientes":**

| ID | Nome  | Telefone    | Email           |
|----|-------|-------------|-----------------|
| 1  | Ana   | 91234-5678  | ana@email.com   |
| 2  | Pedro | 99876-5432  | pedro@email.com |

### 👉 Resumindo:
Um banco de dados relacional guarda dados em tabelas que podem se relacionar entre si, garantindo organização, consistência e integridade.

---

### 2. Não Relacional (NoSQL)
- É um tipo de banco de dados que não utiliza o modelo de tabelas (linhas e colunas) como os relacionais.
- Não segue a rigidez das tabelas.
- Foi criado para lidar com grandes volumes de dados, muitas vezes não estruturados (como redes sociais, sensores IoT, big data, logs etc.).
- Exemplos: MongoDB, DynamoDB, Cassandra.

**Exemplo em formato JSON:**

```json
{
  "id": 1,
  "nome": "Ana",
  "telefone": "91234-5678",
  "email": "ana@email.com"
}
```
### 👉 Resumindo:
Um banco de dados Não Relacional (NoSQL) guarda dados de forma flexível e distribuída, sem depender de tabelas, e é ideal para dados variados, dinâmicos e em grande escala.

---

O que é o RDS?

O RDS (Amazon Relational Database Service) é um serviço gerenciado da AWS que facilita a criação, a operação e a escalabilidade de banco e dados relacional na nuvem.

👉 Em vez de você precisar instalar, configurar, manter e atualizar manualmente um banco de dados em um servidor físico ou em uma VM, o RDS automatiza tarefas.  

---

O que é o DynamoDB?

O **Amazon DynamoDB** é um serviço de **banco de dados NoSQL rápido e flexível totalmente gerenciado** da AWS.  
Ele foi projetado para oferecer **alta performance** (milissegundos de resposta) e **escalabilidade automática**, sem que você precise se preocupar em configurar ou manter servidores.  
O modelo de dados flexível e o desempenho confiável fazem dele a escolha perfeita para aplicativos móveis e da web como jogos, tecnologia de anúncios, Internet das Coisas (IoT) e muitos outros aplicativos.  

---

### 🔑 Principais características do DynamoDB
- 📂 **NoSQL**: trabalha com dados em formato de **tabelas** que armazenam **itens** (linhas) e **atributos** (colunas), mas não segue a estrutura rígida de bancos relacionais (como MySQL ou PostgreSQL).  
- ⚡ **Baixa latência**: respostas em **milissegundos**, mesmo com grandes volumes de dados.  
- 📈 **Escalabilidade automática**: aumenta ou diminui a capacidade de leitura e escrita conforme a demanda (sem precisar de intervenção manual).  
- 🛡️ **Alta disponibilidade e durabilidade**: os dados são replicados automaticamente em múltiplas zonas de disponibilidade (AZs) dentro da região escolhida.  
- 🔐 **Segurança**: integração com o **IAM** (controle de permissões), criptografia em repouso e em trânsito.  
- 🔄 **Integração com outros serviços AWS**: Lambda, API Gateway, Kinesis, entre outros, sendo bastante usado em arquiteturas **serverless**.  

---

### 📌 Quando usar o DynamoDB?
- Aplicações que exigem **respostas rápidas** e **grande volume de dados**.  
- Jogos online, e-commerce, apps móveis, IoT, chatbots.  
- Situações em que os dados mudam com frequência e precisam estar sempre disponíveis em tempo real.  

---

👉 **Resumindo:**  
O **Amazon DynamoDB** é um **banco de dados NoSQL da AWS**, rápido, escalável, seguro e sem a necessidade de gerenciamento de servidores.

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒

