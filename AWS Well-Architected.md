## AWS Well-Architected Framework

O **AWS Well-Architected Framework** é um conjunto de boas práticas e princípios criado pela AWS para ajudar arquitetos de nuvem a **projetar, construir e manter workloads** (aplicações, sistemas, infraestruturas) que sejam **seguros, eficientes, resilientes, econômicos e sustentáveis**.  

Ele funciona como uma **bússola**, orientando empresas sobre como avaliar e melhorar suas arquiteturas na nuvem.  

O **AWS Well-Architected Framework** não fornece detalhes de implementação ou padrões de arquitetura.  

Em vez disso, ele oferece um conjunto de perguntas essenciais que ajudam a avaliar como uma arquitetura específica se alinha às melhores práticas da nuvem.  
Cada pergunta vem acompanhada de informações sobre serviços e soluções relevantes, além de referências a recursos adicionais para aprofundamento.  

---

## Perguntas Essenciais

### 1️⃣ Operational Excellence (Excelência Operacional)
- Como você monitora e opera sua carga de trabalho?  
- Como você realiza mudanças e gerencia incidentes?  
- Como você evolui procedimentos e processos?  

### 2️⃣ Security (Segurança)
- Como você protege dados, sistemas e ativos?  
- Como você gerencia identidade e acesso?  
- Como você detecta e responde a eventos de segurança?  

### 3️⃣ Reliability (Confiabilidade)
- Como você se prepara para falhas de serviço ou picos de demanda?  
- Como você monitora a integridade do sistema?  
- Como você planeja a recuperação de desastres?  

### 4️⃣ Performance Efficiency (Eficiência de Performance)
- Como você seleciona os recursos certos para cada carga de trabalho?  
- Como você monitora e ajusta a performance?  
- Como você avalia novas tecnologias para otimização?  

### 5️⃣ Cost Optimization (Otimização de Custos)
- Como você gerencia e monitora custos?  
- Como você evita gastos desnecessários?  
- Como você dimensiona os recursos conforme a demanda?  

---

💡 **Resumo:**  
O framework não dita “como fazer”, mas fornece **perguntas estratégicas** que ajudam a **identificar pontos fortes, riscos e oportunidades de melhoria** em qualquer arquitetura na nuvem.


---

## Os 5 pilares do Well-Architected Framework

| **Pilar**                    | **Objetivo**                                                                 | **Exemplos de Boas Práticas**                                                                 |
|-------------------------------|-------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------|
| **Excelência Operacional**    | Melhorar processos, monitoramento e automação, visando eficiência operacional. | Uso de *Infrastructure as Code (IaC)*, documentação de processos, automação de tarefas.        |
| **Segurança**                 | Proteger dados, sistemas e ativos.                                            | Princípio de privilégio mínimo, criptografia, gerenciamento de identidades e acessos (IAM).    |
| **Confiabilidade**            | Garantir resiliência, recuperação de falhas e adaptação a mudanças.           | Multi-AZ, backups automáticos, testes de recuperação de desastres.                             |
| **Eficiência de Performance** | Usar recursos de forma otimizada e escalável para atender às demandas.         | Escolha correta de instâncias (EC2, Lambda), escalabilidade automática (*Auto Scaling*).       |
| **Otimização de Custos**      | Evitar desperdícios e otimizar o uso de recursos.                              | Instâncias reservadas ou spot, desligar recursos ociosos, monitorar custos com AWS Cost Explorer. |

<img width="680" height="337" alt="image" src="https://github.com/user-attachments/assets/509c69b1-3136-490c-abf6-2cb7f2f84646" />

---

## Resumindo
O **AWS Well-Architected Framework** funciona como um **checklist estratégico** para garantir que as cargas de trabalho na nuvem sejam:  
- 🔒 Seguros  
- ⚡ Eficientes  
- ♻️ Sustentáveis  
- 💰 Econômicos  
- 🔄 Resilientes  

---

🔗 Referência oficial: [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected)

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
