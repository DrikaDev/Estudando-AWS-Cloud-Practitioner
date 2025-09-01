## ☁️ AWS Well-Architected Framework (WAF)

O **AWS Well-Architected Framework** é um conjunto de boas práticas e princípios criado pela AWS para ajudar arquitetos de nuvem a **projetar, construir e manter workloads** (aplicações, sistemas, infraestruturas) que sejam **seguros, eficientes, resilientes, econômicos e sustentáveis**.  

Ele funciona como uma **bússola**, orientando empresas sobre como avaliar e melhorar suas arquiteturas na nuvem.  

Em vez de fornecer detalhes de implementação, o framework oferece um **conjunto de perguntas essenciais** que ajudam a avaliar como uma arquitetura específica se alinha às melhores práticas da nuvem.  

Cada pergunta vem acompanhada de informações sobre serviços e soluções relevantes, além de referências a recursos adicionais para aprofundamento.

---

## 📝 Perguntas Essenciais

### 1️⃣ Operational Excellence (Excelência Operacional)
Foco: monitoramento, automação, melhoria contínua e processos eficientes.
- Como você monitora e opera sua carga de trabalho?  
- Como você realiza mudanças e gerencia incidentes?  
- Como você evolui procedimentos e processos?  

### 2️⃣ Security (Segurança) 
Foco: proteção de dados, sistemas e ativos; gerenciamento de identidade e acesso; resposta a incidentes.
- Como você protege dados, sistemas e ativos?  
- Como você gerencia identidade e acesso?  
- Como você detecta e responde a eventos de segurança?  

### 3️⃣ Reliability (Confiabilidade)
Foco: resiliência, recuperação de falhas, redundância e continuidade do serviço.
- Como você se prepara para falhas de serviço ou picos de demanda?  
- Como você monitora a integridade do sistema?  
- Como você planeja a recuperação de desastres?  

### 4️⃣ Performance Efficiency (Eficiência de Performance)
Foco: uso otimizado de recursos, escalabilidade e avaliação de novas tecnologias.
- Como você seleciona os recursos certos para cada carga de trabalho?  
- Como você monitora e ajusta a performance?  
- Como você avalia novas tecnologias para otimização?  

### 5️⃣ Cost Optimization (Otimização de Custos)
Foco: gestão eficiente de custos, evitar desperdícios e dimensionamento conforme demanda.
- Como você gerencia e monitora custos?  
- Como você evita gastos desnecessários?  
- Como você dimensiona os recursos conforme a demanda?  

---

## ⚡ Os 5 pilares do Well-Architected Framework

| **Pilar**                    | **Objetivo**                                                                 | **Exemplos de Boas Práticas**                                                                 |
|-------------------------------|-------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------|
| **Excelência Operacional**    | Melhorar processos, monitoramento e automação, visando eficiência operacional. | Uso de *Infrastructure as Code (IaC)*, documentação de processos, automação de tarefas.        |
| **Segurança**                 | Proteger dados, sistemas e ativos.                                            | Princípio de privilégio mínimo, criptografia, gerenciamento de identidades e acessos (IAM).    |
| **Confiabilidade**            | Garantir resiliência, recuperação de falhas e adaptação a mudanças.           | Multi-AZ, backups automáticos, testes de recuperação de desastres.                             |
| **Eficiência de Performance** | Usar recursos de forma otimizada e escalável para atender às demandas.        | Escolha correta de instâncias (EC2, Lambda), escalabilidade automática (*Auto Scaling*).       |
| **Otimização de Custos**      | Evitar desperdícios e otimizar o uso de recursos.                              | Instâncias reservadas ou spot, desligar recursos ociosos, monitorar custos com AWS Cost Explorer. |

---

## 🏗️ Princípios de Design

- **Parar de adivinhar suas necessidades de capacidade**  
  Ajuste automaticamente a infraestrutura conforme a demanda muda. Monitore e automatize a adição ou remoção de recursos para manter níveis ideais.

- **Testar os sistemas em escala de produção**  
  Crie ambientes duplicados sob demanda, realize testes e desative os recursos após o uso, pagando apenas pelo período de execução.

- **Automatizar para facilitar experimentos de arquitetura**  
  Permite criar e replicar sistemas com baixo custo e esforço manual, monitorando alterações e revertendo parâmetros quando necessário.

- **Permitir que as arquiteturas evoluam**  
  Automatize e teste sob demanda, reduzindo riscos e permitindo que sistemas evoluam junto com as necessidades do negócio.

- **Impulsionar arquiteturas orientadas por dados**  
  Coletar dados sobre o comportamento das cargas de trabalho permite decisões baseadas em fatos e aprimoramentos contínuos da arquitetura.

- **Aprimorar por meio de simulações**  
  Teste operações e processos programando eventos aleatórios em produção, identificando melhorias e desenvolvendo experiências na gestão de incidentes.

---

## 💡Resumo

O AWS Well-Architected Framework funciona como um checklist estratégico que ajuda a:

🔒 Garantir segurança  
⚡ Otimizar performance  
🔄 Melhorar confiabilidade  
💰 Controlar custos  
♻️ Evoluir a arquitetura continuamente  

---

🔗 Referência oficial: [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected)

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
