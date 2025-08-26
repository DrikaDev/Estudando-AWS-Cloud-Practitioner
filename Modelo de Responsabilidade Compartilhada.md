## Modelo de Responsabilidade Compartilhada AWS

Na AWS, o conceito de **"Responsabilidade Compartilhada"** refere-se à distribuição de responsabilidades de segurança e conformidade entre a AWS e o usuário/cliente.  

Nesse modelo, a AWS é responsável pela segurança **"da nuvem"** — incluindo a infraestrutura global composta por hardware, software, redes e as instalações que executam os serviços de nuvem da AWS.  

Por outro lado, o usuário é responsável pela segurança **"na nuvem"** — isso inclui gerenciar e configurar os serviços controlados pelo cliente, proteger as credenciais da conta e proteger os dados do cliente.  
Os clientes devem examinar cuidadosamente os serviços que escolherem, pois suas respectivas responsabilidades variam de acordo com os serviços utilizados.  

Esse modelo compartilhado visa reduzir a carga operacional para os usuários e fornecer controles de segurança flexíveis.  

---

### Responsabilidades: AWS x Cliente

| **Área**                       | **AWS**                                                   | **Cliente**                                                                 |
|--------------------------------|----------------------------------------------------------|------------------------------------------------------------------------------|
| **Gerenciamento de patches**   | Aplica patches e corrige falhas na **infraestrutura**.     | Aplica patches no **sistema operacional convidado** e nos **aplicativos**.   |
| **Gerenciamento de configuração** | Mantém a configuração dos **dispositivos de infraestrutura**. | Configura **bancos de dados**, **aplicativos** e **sistemas operacionais convidados**. |
| **Conhecimentos e treinamento**  | Treina os **funcionários da AWS**.                      | Treina os **funcionários da própria empresa**.                               |

