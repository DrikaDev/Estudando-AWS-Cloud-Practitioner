## 🧪 Lab 178 - Trabalhar com o AWS Lambda

## Visão geral do laboratório

Neste laboratório, você vai implantar e configurar uma solução de computação sem servidor baseada no **AWS Lambda**. A função do Lambda vai gerar um relatório de análise de vendas, extraindo dados de um banco de dados e enviando os resultados diariamente.  

As informações de conexão do banco de dados são armazenadas no **armazenamento de parâmetros**, um recurso do **AWS Systems Manager**. O próprio banco de dados é executado em uma instância do Linux do **Amazon Elastic Compute Cloud (Amazon EC2)**, do **Apache**, do **MySQL** e do **PHP (LAMP)**.

O diagrama a seguir mostra a arquitetura da solução do relatório de análise de vendas e ilustra a ordem em que as ações ocorrem:

![Diagrama da Arquitetura](link-para-seu-diagrama.png)
