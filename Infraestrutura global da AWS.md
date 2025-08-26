## 🌏 Infraestrutura Global da AWS

A **Infraestrutura Global da AWS** é a base física e lógica que sustenta todos os serviços de nuvem da AWS no mundo.  
Ela foi projetada para ser **segura, escalável, resiliente e de alta disponibilidade**, permitindo que aplicações rodem em qualquer lugar do planeta.  

---

## Componentes da Infraestrutura Global

| **Componente**                  | **Descrição / Função**                                                                 |
|---------------------------------|---------------------------------------------------------------------------------------|
| **Regiões (Regions)**           | Conjuntos de data centers em uma área geográfica específica. Isoladas entre si para resiliência. Ex.: `us-east-1`, `sa-east-1`. |
| **Zonas de Disponibilidade (AZs)** | Cada AZ é composta por um ou mais data centers, projetadas para isolamento de falha. Permitem alta disponibilidade, são tolerantes a falhas e são elásticas e escaláveis. |
| **Zonas Locais (Local Zones)**   | Infraestruturas menores próximas a grandes centros urbanos, para reduzir **latência** em aplicações críticas. |
| **AWS Outposts**                | Extensão da infraestrutura AWS instalada **no ambiente do cliente**, gerenciada pela AWS, ideal para workloads híbridos. |
| **Pontos de Presença (PoPs)**   | Incluem **Edge Locations** e **Regional Edge Caches**, usados por serviços como CloudFront para entregar conteúdo rapidamente e reduzir latência. |

---

<img width="1580" height="911" alt="image" src="https://github.com/user-attachments/assets/2f131cd9-17a1-42f5-8efd-dc53ad61dc5a" />

---

## Resumo dos Benefícios

- Implantação de workloads **próximos aos usuários finais**  
- **Alta disponibilidade** com arquitetura multi-AZ*  
- **Baixa latência** usando PoPs e Local Zones  
- Atendimento a requisitos de **residência de dados** escolhendo regiões específicas  

---

## *Arquitetura Multi-AZ

A **arquitetura multi-AZ (Multi Availability Zone)** é um padrão de design na AWS que **distribui recursos e serviços em múltiplas Zonas de Disponibilidade 
(AZs)** dentro de uma mesma região.  
O objetivo principal é **garantir alta disponibilidade e tolerância a falhas**, ou seja, se uma AZ falhar, outra assume, mantendo o sistema funcionando 
sem interrupções.  

### Benefícios da arquitetura Multi-AZ

| **Benefício**                  | **Descrição** |
|--------------------------------|---------------|
| **Alta disponibilidade**        | Se uma AZ ficar indisponível, outra assume automaticamente. |
| **Tolerância a falhas**         | Reduz riscos de downtime por problemas locais em uma AZ. |
| **Redundância de dados**        | Dados podem ser replicados entre AZs para evitar perda. |
| **Desempenho consistente**      | Balanceamento de carga entre AZs mantém performance estável. |

### Exemplo prático

- Um banco de dados **RDS Multi-AZ** replica automaticamente os dados de uma AZ para outra.  
- Se a AZ principal tiver falha, o RDS muda automaticamente para a réplica, sem perda de serviço.  

---

🔗 Referência oficial: [AWS Global Infrastructure](https://aws.amazon.com/about-aws/global-infrastructure/)

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
