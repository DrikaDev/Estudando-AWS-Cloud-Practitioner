## Amazon DynamoDB

O **Amazon DynamoDB** é um banco de dados **NoSQL totalmente gerenciado** que oferece **desempenho rápido e previsível** com **escalabilidade automática**.

## Características principais

- **Modelo de dados:** chave-valor e documentos.  
- **Desempenho:** latência de **milissegundos** em qualquer escala.  
- **Escalabilidade:** suporta **mais de 10 trilhões de solicitações por dia** e picos de **mais de 20 milhões de solicitações por segundo**.  
- **Durabilidade:** mantém dados seguros através de **replicação automática em três zonas de disponibilidade** dentro de uma região da AWS.

## Tabelas, Itens e Atributos no Amazon DynamoDB

No **Amazon DynamoDB**, os dados são organizados em **tabelas**, que contêm **itens**, e cada item é composto por **atributos**.

### Tabelas
- Uma **tabela** é uma coleção de **itens**.  
- Cada tabela possui uma **chave primária** que identifica unicamente cada item.

### Itens
- Um **item** é um grupo de **atributos**.  
- Semelhante a **linhas** ou **registros** em bancos de dados tradicionais.  
- Cada item deve ter uma **chave primária**:  
  - **Simples:** apenas chave de partição.  
  - **Composta:** chave de partição + chave de classificação.

### Atributos
- Um **atributo** é um **par nome-valor**.  
- O **nome do atributo** é uma string.  
- O **valor do atributo** pode ser:  
  - String  
  - Número  
  - Binário  
  - Booleano  
  - Nulo  
  - Lista  
  - Mapa  
  - Conjunto de Strings  
  - Conjunto de Números  
  - Conjunto Binário

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
