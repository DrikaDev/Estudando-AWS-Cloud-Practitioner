## ☁️ Amazon EC2

## Índice

- [O que é o Amazon EC2?](#o-que-é-o-amazon-ec2)
- [O que é uma instância?](#o-que-é-uma-instância)
- [Tipos de instâncias e casos de uso](#tipos-de-instâncias-e-casos-de-uso)
- [Definição de preços do EC2](#definição-de-preços-do-ec2)
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

> PS: O disco de uma EC2 pode variar — não é fixo como em um PC comum.
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

Uma **instância do EC2** é um **servidor virtual** na Nuvem AWS.  
Ao iniciar uma instância do EC2, você precisa definir um **tipo de instância**, que determina o hardware e os recursos disponíveis.  

Cada tipo de instância oferece um equilíbrio diferente de:  
- 💻 **Computação (CPU)**  
- 🧠 **Memória (RAM)**  
- 🌐 **Rede (Networking)**  
- 💾 **Armazenamento (EBS, SSD, etc.)**

[⬆ Voltar ao índice](#índice)

---

## Tipos de instâncias e casos de uso

As instâncias da AWS são categorizadas de acordo com o perfil de desempenho que oferecem.  
Escolher o tipo de instância certo é essencial para otimizar **custo x desempenho** em seus projetos na nuvem.  
A seguir, alguns tipos e seus principais cenários de uso:  

| Família       | Otimização           | Exemplos de uso                                  |
|---------------|----------------------|--------------------------------------------------|
| **T2/T3/T4g** | Econômicas, burstable | Testes, dev, apps leves                          |
| **M4/M5/M6**  | Balanceadas (CPU+RAM) | Aplicações web, servidores de aplicação          |
| **C4/C5/C6**  | CPU otimizada        | Processamento intensivo, análises de dados       |
| **R4/R5/R6**  | Memória otimizada    | Bancos de dados, cache, in-memory apps           |
| **P3/P4/G4**  | Computação acelerada (GPU) | IA, Machine Learning, renderização               |
| **I3/I4/D2**  | Armazenamento e I/O  | Big Data, data warehouses, alta taxa de leitura/escrita |

[⬆ Voltar ao índice](#índice)

---

## Definição de preços do EC2

Com o **Amazon EC2**, você paga apenas pelo tempo de computação que usar.  
A AWS oferece diversas opções de preço para diferentes necessidades:

| Tipo de Instância         | Vantagem Principal                       | Casos de Uso Comuns                                    |
|----------------------------|------------------------------------------|-------------------------------------------------------|
| **Sob Demanda**           | Flexibilidade máxima                     | Dev/teste, cargas imprevisíveis                       |
| **Reservadas (Standard)** | Maior economia (até 3 anos)              | Aplicações estáveis, workloads previsíveis            |
| **Reservadas (Conversíveis)** | Flexibilidade com bom desconto        | Workloads que podem mudar tipo de instância ou região |
| **Savings Plans**         | Desconto alto sem especificar instância  | Diversos workloads dentro de uma família de instância |
| **Spot**                  | Economia extrema (até 90%)               | Processamento flexível, tarefas tolerantes a falhas   |
| **Hosts Dedicados**       | Controle total + licenciamento próprio   | Workloads com requisitos de conformidade/licenciamento |

[⬆ Voltar ao índice](#índice)

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
