## Amazon RDS (Relational Database Service)

O **Amazon RDS (Relational Database Service)** é um serviço web da **AWS** projetado para **simplificar a configuração, operação e dimensionamento** de 
bancos de dados relacionais na nuvem.  

Ele fornece **capacidade redimensionável e econômica** para bancos de dados padrão do setor e gerencia automaticamente tarefas comuns de administração, como:  
- Aplicação de patches  
- Backups automáticos  
- Monitoramento  
- Segurança com criptografia em repouso e em trânsito  

### Recursos principais
- **Suporte a múltiplos mecanismos de banco de dados:**  
  - Amazon Aurora  
  - PostgreSQL  
  - MySQL  
  - MariaDB  
  - Oracle Database  
  - SQL Server  

- **Escalabilidade flexível:**  
  - Instâncias variando de **5 GB a 6 TB de memória**, permitindo adaptação conforme a necessidade.  

- **Alta disponibilidade:**  
  - Backups automáticos, snapshots manuais e replicação multi-AZ.  

- **Segurança:**  
  - Criptografia em repouso e em trânsito.  
  - Atualizações automáticas de patches.  

---

💡 **Resumindo:**  
O **Amazon RDS** elimina grande parte da complexidade de gerenciar bancos de dados relacionais, permitindo que você se concentre na **aplicação e nos 
dados**, e não na manutenção da infraestrutura.

---

## Instâncias de Banco de Dados  

O termo **"Instância de Banco de Dados"** é usado no contexto do Serviço de Banco de Dados Relacional (RDS) da Amazon.  

Uma Instância de Banco de Dados é essencialmente um **ambiente de banco de dados isolado na nuvem**, executado dentro do **Amazon RDS**.  

- Pode conter vários bancos de dados criados pelo usuário.  
- Pode ser acessada usando as mesmas ferramentas e aplicativos que você usaria com uma instância de banco de dados independente.  
- Pode ser criada e gerenciada por meio de:  
  - Console de Gerenciamento da AWS  
  - Interface de Linha de Comando (CLI) do AWS RDS  
  - Chamadas de API simples  

---

## Backup / Restauração no AWS RDS

O **AWS RDS** permite restaurar sua **instância de banco de dados** para um **ponto específico no tempo**.

## Como funciona?

- Ao iniciar uma **restauração pontual**, uma **nova instância de banco de dados** é criada.  
- Todas as transações ocorridas após o ponto especificado **não fazem parte** da nova instância.  
- É possível restaurar até o **último momento restaurável** (normalmente nos últimos cinco minutos), conforme indicado no Console de Gerenciamento do AWS RDS.  

## Observações importantes

- O tempo necessário para criar a restauração depende da **diferença de tempo** entre o momento de início e o ponto de restauração.  
- O processo ocorre **sem impacto** no banco de dados de origem.  
- É possível continuar usando seu banco de dados **durante a restauração**.  

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
