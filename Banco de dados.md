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
RDS (Amazon Relational Database Service) é um serviço gerenciado da AWS que facilita a criação, a operação e a escalabilidade de banco e dados relacional na nuvem.

👉 Em vez de você precisar instalar, configurar, manter e atualizar manualmente um banco de dados em um servidor físico ou em uma VM, o RDS automatiza tarefas.  

