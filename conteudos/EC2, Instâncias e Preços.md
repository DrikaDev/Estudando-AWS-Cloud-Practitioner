## ☁️ Amazon EC2

## Índice

- [O que é o Amazon EC2?](#o-que-é-o-amazon-ec2)
- [O que é uma instância?](#o-que-é-uma-instância)
  - [Tipos de instâncias](#tipos-de-instâncias)
  - [Séries e casos de uso](#séries-e-casos-de-uso)
- [Definição de preços do EC2](#definição-de-preços-do-ec2)
- [Opções de Pagamento para Instâncias Reservadas (RIs) do Amazon EC2](#opções-de-pagamento-para-instâncias-reservadas-ris-do-amazon-ec2)
- [Como escolher a instância EC2 correta?](#como-escolher-a-instância-ec2-correta)
---

## O que é o Amazon EC2?

O **Amazon Elastic Compute Cloud (EC2)** é um serviço de computação em nuvem da **AWS** que fornece instâncias (máquinas virtuais) com capacidade de computação **segura, escalável e sob demanda**.  
Ele permite que desenvolvedores criem, implantem e executem aplicativos de forma rápida e flexível, sem precisar comprar e manter servidores físicos.  

Uma instância EC2 é um serviço do tipo IaaS, composta por:  
- CPU;
- Memória (RAM);
- Armazenamento/Disco (EBS, Instance Store ou outros volumes anexados);
- Rede (interfaces de rede virtuais, largura de banda configurável);
- Sistema Operacional (Linux, Windows ou outro que você escolher instalar);

> ⚠️ O disco de uma EC2 pode variar — não é fixo como em um PC comum.
> Você pode escolher EBS (Elastic Block Store) para armazenamento persistente,
> ou usar o Instance Store (armazenamento temporário que se perde ao desligar a instância).

Com o EC2, você pode:  
- 🚀 Iniciar quantos **servidores virtuais** precisar  
- 🔒 Configurar **segurança e rede**  
- 💾 Gerenciar o **armazenamento**  
- 📈 **Escalar verticalmente** para lidar com picos de uso  
- 📉 **Reduzir capacidade** quando a demanda diminuir  

Benefícios principais:  
- **Redução de custos de hardware**  
- **Agilidade no desenvolvimento e implantação**  
- **Flexibilidade para escalar conforme a demanda**

[⬆ Voltar ao índice](#índice)

---

## O que é uma instância?

Uma **instância do Amazon EC2** é um **servidor virtual** na Nuvem AWS.  
Ao iniciar uma instância, você precisa definir um **tipo de instância**, que determina o hardware subjacente e os recursos disponíveis.  

### Tipos de instâncias

Os tipos de instância são nomeados com base na **família** de instância e no tamanho da instância, cada uma otimizada para um tipo de recurso:  

| Família Principal | Foco Principal | Casos de Uso |
|-------------------|----------------|--------------|
| ⚙️ **Uso Geral** | Equilíbrio entre CPU, memória e rede | Aplicações web, ambientes de dev/teste |
| 💻 **Otimizada para Computação** | Alto desempenho de CPU | Processamento intensivo, servidores de aplicação |
| 🧠 **Otimizada para Memoria** | Grande capacidade de RAM | Bancos de dados, cache em memória, análises em tempo real |
| 🌐 **Computação Acelerada** | GPU, FPGA, chips especializados | IA, Machine Learning, HPC, renderização |
| 💾 **Otimizada para Armazenamento** | I/O de disco de alta performance | Big Data, data warehouses, bancos NoSQL |

### Séries e casos de uso

Dentro de cada família, existem **séries** (gerações) que trazem combinações específicas de hardware e otimizações.  

| Série        | Família             | Otimização                | Exemplos de Uso |
|--------------|---------------------|---------------------------|-----------------|
| **T2/T3/T4g** | General Purpose     | Econômicas, burstable     | Testes, dev, apps leves |
| **M4/M5/M6**  | General Purpose     | Balanceadas (CPU+RAM)     | Aplicações web, servidores de aplicação |
| **C4/C5/C6**  | Compute Optimized   | CPU otimizada             | Processamento intensivo, análises de dados |
| **R4/R5/R6**  | Memory Optimized    | Memória otimizada         | Bancos de dados, cache, in-memory apps |
| **P3/P4/G4**  | Accelerated Comp.   | GPU                       | IA, Machine Learning, renderização |
| **I3/I4/D2**  | Storage Optimized   | Armazenamento e I/O       | Big Data, data warehouses, alta taxa de leitura/escrita |

[⬆ Voltar ao índice](#índice)

---

## Definição de preços do EC2

Com o **Amazon EC2**, você paga apenas pelo tempo de computação que usar.  
A AWS oferece diversas opções de preço para diferentes necessidades:

- **Instâncias Sob Demanda**:  
  O pagamento é efetuado estritamente pela capacidade computacional consumida, eliminando a necessidade de investimentos iniciais ou contratos de longo prazo.
  
- **Instâncias Reservadas**:  
  Para cargas de trabalho estáveis e previsíveis, é possível obter uma redução de custos de até 75% ao assumir um compromisso de uso por um período de 1 ou 3 anos, vinculado a famílias de instâncias e Regiões específicas da AWS.
  
- **Instâncias Spot**:  
  Permitem ofertar lances em capacidade computacional excedente da AWS, alcançando descontos de até 90% em relação ao preço Sob Demanda. Em contrapartida, a instância pode ser interrompida caso a AWS precise reaver a capacidade.
  
- **Savings Plans**:  
  Garantem economia de até 72% em uma variedade de tipos de instâncias e serviços, mediante o compromisso com um nível de uso consistente por 1 ou 3 anos.  

- **Hosts Dedicados**:  
  Reservam um servidor físico inteiro para uso exclusivo.
  Oferecem controle total e são ideais para workloads com requisitos estritos de conformidade, segurança ou licenciamento **BYOL** (Bring Your Own License).
  Permite licenciamento de software por socket/CPU/host (como Windows Server, SQL Server, Oracle) porque você sabe onde as instâncias estão fisicamente.  
    
- **Instâncias Dedicadas**:  
  Pagamento por instâncias que operam em hardware fisicamente isolado, dedicado exclusivamente para você, reservado unicamente para a sua conta.
  O principal benefício é o isolamento em relação às operações de outros clientes da AWS.  

<p align="center">
  <img width="370" height="283" alt="image" src="https://github.com/user-attachments/assets/2ce8ecfe-0cde-460c-96dd-bb33d75f0345" />
</p>

[⬆ Voltar ao índice](#índice)

---

## Opções de Pagamento para Instâncias Reservadas (RIs) do Amazon EC2

As opções de pagamento para instâncias reservadas (RIs) do Amazon EC2 são **Tudo Adiantado**, **Parcialmente Adiantado** e **Sem Adiantamento**. A escolha da opção afeta o valor do desconto, sendo que o **Tudo Adiantado** oferece a maior economia.

### Modelos de Pagamento

#### AURI - Tudo Adiantado (All Upfront Payment)
- **Descrição:** Você faz um único pagamento inicial que cobre todo o período da instância reservada.  
- **Vantagem:** Oferece o maior desconto disponível.

#### PURI - Parcialmente Adiantado (Partial Upfront Payment)
- **Descrição:** Você paga um valor inicial baixo e, em seguida, realiza pagamentos por hora com desconto ao longo do período da reserva.  
- **Vantagem:** Equilíbrio entre pagamento inicial e economia, com desconto menor que o "Tudo Adiantado".

#### NURI - Sem Adiantamento (No Upfront Payment)
- **Descrição:** Não é necessário nenhum pagamento inicial. Você é cobrado uma taxa por hora com desconto durante todo o período da reserva, independentemente de a instância estar em uso ou não.  
- **Vantagem:** Sem custo inicial, mas o desconto é o menor entre as opções.

<img width="1210" height="385" alt="image" src="https://github.com/user-attachments/assets/657136d9-5711-4419-b2f8-8e8ec4bb2261" />

### Fatores a Considerar
- **Redução de Custos:** Quanto mais você paga adiantado, maior é o desconto que você obtém.  
- **Planejamento:** Planeje o uso contínuo dos recursos, pois o compromisso de um período mais longo (1 ou 3 anos) influencia a economia oferecida.  
- **Flexibilidade:** Avalie se você precisa da flexibilidade das instâncias reservadas **conversíveis** (que permitem alterar atributos da instância) ou se as instâncias reservadas **padrão** (maior desconto) são mais adequadas para uso constante.

---

## Como escolher a instância EC2 correta?

Escolher a **instância EC2 correta** não se trata apenas de selecionar um tipo aleatório, mas sim de **entender as 
necessidades da aplicação** e utilizar os recursos da nuvem de forma inteligente para alcançar eficiência 
operacional e econômica.  

Ou seja, depende de equilibrar **custo, desempenho e requisitos técnicos** da sua aplicação.  

**Passos para escolher a EC2 correta:**

1. Defina o tipo de carga de trabalho
- **Teste/Desenvolvimento** → instâncias menores e mais baratas (ex: `t2`, `t3` – burstable)  
- **Aplicações Web** → balanceie CPU e memória (ex: `m5`, `m6` – general purpose)  
- **Banco de Dados/Cache** → muita memória (ex: `r5`, `r6` – memory optimized)  
- **Machine Learning, IA, HPC, Renderização** → precisa de GPU (ex: `p3`, `p4`, `g4` – GPU instances)  
- **Big Data/Processamento pesado** → otimizado para computação (ex: `c5`, `c6` – compute optimized)  
- **Armazenamento intenso** → alta taxa de I/O (ex: `i3`, `i4`, `d2` – storage optimized)  

2. Analise os requisitos técnicos da aplicação
- **CPU** → número de vCPUs necessárias  
- **Memória (RAM)** → essencial para bancos de dados, cache e apps em tempo real  
- **Armazenamento** → escolha entre **EBS (persistente)** ou **Instance Store (temporário)**  
- **Rede** → largura de banda necessária (até 100 Gbps em algumas instâncias)  

3. Pense no crescimento (escalabilidade)
- **Tráfego imprevisível** → use **Auto Scaling**  
- **Tráfego estável** → escolha uma instância fixa de tamanho adequado  

4. Compare preços e descontos
- **On-Demand** → flexível, mas mais caro  
- **Reserved Instances** → até 72% mais barato (1 ou 3 anos)  
- **Spot Instances** → até 90% de desconto, mas podem ser interrompidas  

5. Teste antes de decidir
- Rode **benchmarks** da aplicação em diferentes instâncias  
- Use o **AWS Compute Optimizer** para recomendações automáticas  

[⬆ Voltar ao índice](#índice)

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
