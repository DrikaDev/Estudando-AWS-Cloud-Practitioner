## ☁️ AWS Well-Architected Framework

O **AWS Well-Architected Framework** é um conjunto de boas práticas e princípios criado pela AWS para ajudar arquitetos de nuvem a 
**projetar, construir e manter workloads** (aplicações, sistemas, infraestruturas) que sejam **seguros, eficientes, resilientes, 
econômicos e sustentáveis**.  

Ele funciona como uma **bússola**, orientando empresas sobre como avaliar e melhorar suas arquiteturas na nuvem.  

Em vez de fornecer detalhes de implementação, o framework oferece um **conjunto de perguntas essenciais** que ajudam a avaliar como 
uma arquitetura específica se alinha às melhores práticas da nuvem.  

Cada pergunta vem acompanhada de informações sobre serviços e soluções relevantes, além de referências a recursos adicionais para 
aprofundamento.

---

## 📝 Perguntas Essenciais

### 📌 1. Excelência Operacional (monitoramento, automação, melhoria contínua e processos eficientes):

Como você gerencia e monitora operações para apoiar seus objetivos de negócio?  
Como você documenta e evolui procedimentos operacionais?  
Como você antecipa falhas e aprende com elas?  

### 🔒 2. Segurança (proteção de dados, sistemas e ativos, gerenciamento de identidade e acesso, resposta a incidentes):

Como você controla quem pode fazer o quê com os recursos?  
Como você protege dados em trânsito e em repouso?  
Como você detecta e responde a incidentes de segurança?  

### ⚡ 3. Confiabilidade (resiliência, recuperação de falhas, redundância e continuidade do serviço):

Como você projeta sua workload para lidar com falhas?  
Como você testa e valida mecanismos de recuperação?  
Como você monitora métricas de saúde e disponibilidade?  

### 🚀 4. Eficiência de Performance (uso otimizado de recursos, escalabilidade e avaliação de novas tecnologias):

Como você escolhe os tipos e tamanhos de recursos corretos?  
Como você monitora e otimiza o desempenho ao longo do tempo?  
Como você utiliza serviços gerenciados para ganhar eficiência?  

### 💰 5. Otimização de Custos (gestão eficiente de custos, evitar desperdícios e dimensionamento conforme demanda):

Como você garante que está gastando apenas com o necessário?  
Como você analisa e prevê custos?  
Como você otimiza o uso de instâncias e serviços?  

### 🌱 6. Sustentabilidade (minimizar impacto ambiental e uso consciente de recursos):

Como você aproveita instâncias Spot ou serviços serverless para usar capacidade ociosa da AWS?
Como você mede e reduz a pegada de carbono da sua workload?  
Como você avalia impacto ambiental nas decisões de arquitetura?  

> 👉 Essas perguntas não exigem respostas fechadas, mas sim reflexões que ajudam a identificar pontos de melhoria e alinhar workloads
> às boas práticas da AWS.

---

## ⚡ Os 6 pilares do Well-Architected Framework

| **Pilar**                    | **Objetivo**                                                                 | **Exemplos de Boas Práticas**                                                                 |
|-------------------------------|-------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------|
| **Excelência Operacional**    | Melhorar processos, monitoramento e automação, visando eficiência operacional. | Uso de *Infrastructure as Code (IaC)*, documentação de processos, automação de tarefas.        |
| **Segurança**                 | Proteger dados, sistemas e ativos.                                            | Princípio de privilégio mínimo, criptografia, gerenciamento de identidades e acessos (IAM).    |
| **Confiabilidade**            | Garantir resiliência, recuperação de falhas e adaptação a mudanças.           | Multi-AZ, backups automáticos, testes de recuperação de desastres.                             |
| **Eficiência de Performance** | Usar recursos de forma otimizada e escalável para atender às demandas.        | Escolha correta de instâncias (EC2, Lambda), escalabilidade automática (*Auto Scaling*).       |
| **Otimização de Custos**      | Evitar desperdícios e otimizar o uso de recursos.                              | Instâncias reservadas ou spot, desligar recursos ociosos, monitorar custos com AWS Cost Explorer. |
| **Sustentabilidade**          | Reduzir impacto ambiental e usar recursos de forma consciente e eficiente.    | Escolha de regiões com energia renovável, uso de instâncias spot/serverless, dimensionamento correto para evitar desperdício. |

---

## 🏗️ Princípios de Design

- **Parar de adivinhar suas necessidades de capacidade**  
  Ajuste automaticamente a infraestrutura conforme a demanda muda. Monitore e automatize a adição ou remoção de recursos para manter
  níveis ideais.

- **Testar os sistemas em escala de produção**  
  Crie ambientes duplicados sob demanda, realize testes e desative os recursos após o uso, pagando apenas pelo período de execução.

- **Automatizar para facilitar experimentos de arquitetura**  
  Permite criar e replicar sistemas com baixo custo e esforço manual, monitorando alterações e revertendo parâmetros quando necessário.

- **Permitir que as arquiteturas evoluam**  
  Automatize e teste sob demanda, reduzindo riscos e permitindo que sistemas evoluam junto com as necessidades do negócio.

- **Impulsionar arquiteturas orientadas por dados**  
  Coletar dados sobre o comportamento das cargas de trabalho permite decisões baseadas em fatos e aprimoramentos contínuos da
  arquitetura.

- **Aprimorar por meio de simulações**  
  Teste operações e processos programando eventos aleatórios em produção, identificando melhorias e desenvolvendo experiências na
  gestão de incidentes.

---

🔗 Referência oficial: [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected)

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
