## Modelo de Responsabilidade Compartilhada AWS

Na AWS, o conceito de **"Responsabilidade Compartilhada"** refere-se à distribuição de responsabilidades de segurança e conformidade entre a AWS e o usuário/cliente.  
Esse modelo compartilhado visa reduzir a carga operacional para os usuários e fornecer controles de segurança flexíveis.  

## Segurança **DA** nuvem
Nesse modelo, a AWS é responsável pela segurança **"da nuvem"** — incluindo a infraestrutura global composta por hardware, software, redes e as instalações que executam os serviços de nuvem da AWS.  

<img width="541" height="262" alt="image" src="https://github.com/user-attachments/assets/f5b3f647-41a7-4fc0-9c43-83c6f21ea37c" />

## Segurança **NA** nuvem
Aqui o usuário é responsável pela segurança **"na nuvem"** — isso inclui gerenciar e configurar os serviços controlados pelo cliente, proteger as credenciais da conta e proteger os dados do cliente.  
Os clientes devem examinar cuidadosamente os serviços que escolherem, pois suas respectivas responsabilidades variam de acordo com os serviços utilizados.  

Para os serviços categorizados como **infraestrutura como serviço (IaaS)**, o cliente é responsável por executar as tarefas necessárias de configuração e de gerenciamento de segurança, como:  
- Configurações de security group  
- Configuração de firewall  
- Aplicação de patches de segurança  
- Atualizações de sistema operacional convidado  

<img width="477" height="313" alt="image" src="https://github.com/user-attachments/assets/1b75295d-d347-4f8f-ac47-f216279905d4" />

---

### Responsabilidades: AWS x Cliente

| **Área**                       | **AWS**                                                   | **Cliente**                                                                 |
|--------------------------------|----------------------------------------------------------|------------------------------------------------------------------------------|
| **Gerenciamento de patches**   | Aplica patches e corrige falhas na **infraestrutura**.     | Aplica patches no **sistema operacional convidado** e nos **aplicativos**.   |
| **Gerenciamento de configuração** | Mantém a configuração dos **dispositivos de infraestrutura**. | Configura **bancos de dados**, **aplicativos** e **sistemas operacionais convidados**. |
| **Conhecimentos e treinamento**  | Treina os **funcionários da AWS**.                      | Treina os **funcionários da própria empresa**.                               |

---

🔗 Referência: [AWS Shared Responsibility Model](https://aws.amazon.com/compliance/shared-responsibility-model/)  

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
