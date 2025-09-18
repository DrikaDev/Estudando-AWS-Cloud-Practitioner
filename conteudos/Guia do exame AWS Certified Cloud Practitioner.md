## 📘 Guia do Exame AWS Certified Cloud Practitioner (CLF-C02)  

### Índice

0. [Sobre o Guia do Exame](#sobre-o-guia-do-exame)

1. [Conceitos de Nuvem](#1-conceitos-de-nuvem)  
   1.1 [Benefícios da Nuvem](#11-benefícios-da-nuvem)  
   1.2 [Princípios de Design da Nuvem](#12-princípios-de-design-da-nuvem)  
   1.3 [Migração para a Nuvem](#13-migração-para-a-nuvem)  
   1.4 [Conceitos dos Aspectos Econômicos](#14-conceitos-dos-aspectos-econômicos)  

2. [Segurança e Conformidade](#2-segurança-e-conformidade)  
   2.1 [Modelo de Responsabilidade Compartilhada](#21-modelo-de-responsabilidade-compartilhada)  
   2.2 [Conceitos de Segurança, Governança e Conformidade](#22-conceitos-de-segurança-governança-e-conformidade)  
   2.3 [Gerenciamento de Acesso](#23-gerenciamento-de-acesso)  
   2.4 [Recursos de Segurança](#24-recursos-de-segurança)  

3. [Tecnologia e Serviços da Nuvem](#3-tecnologia-e-serviços-da-nuvem)  
   3.1 [Métodos de Implantação e Operação na Nuvem](#31-métodos-de-implantação-e-operação-na-nuvem)  
   3.2 [Definir a Infraestrutura Global](#32-definir-a-infraestrutura-global)  
   3.3 [Serviços Computacionais](#33-serviços-computacionais)  
   3.4 [Banco de Dados, Serviços e Migração](#34-banco-de-dados-serviços-e-migração)  
   3.5 [Serviços de Rede](#35-serviços-de-rede)  
   3.6 [Serviços de Armazenamento](#36-serviços-de-armazenamento)  
   3.7 [Serviços de Inteligência Artificial, Machine Learning e Analytics](#37-serviços-de-inteligência-artificial-machine-learning-e-analytics)  
   3.8 [Outros Serviços – Categorias](#38-outros-serviços-–-categorias)  

4. [Cobrança, Preços e Suporte](#4-cobrança-preços-e-suporte)  
   4.1 [Comparar Modelos de Preços](#41-comparar-modelos-de-preços)  
   4.2 [Gerenciamento de Cobrança](#42-gerenciamento-de-cobrança)  
   4.3 [Recursos Técnicos e Opções de Suporte](#43-recursos-técnicos-e-opções-de-suporte)  

---

## Sobre o Guia do Exame

Antes de iniciarmos a preparação para o exame **AWS Certified Cloud Practitioner (CLF-C02)**, é essencial ler o guia oficial 
disponibilizado pela AWS 👉🏻 [veja aqui o guia oficial do exame](https://d1.awsstatic.com/pt_BR/training-and-certification/docs-cloud-practitioner/AWS-Certified-Cloud-Practitioner_Exam-Guide.pdf)

O guia funciona como um **mapa de estudos**, mostrando com clareza:  
- Os **tópicos cobrados** no exame.  
- A **distribuição do peso** de cada domínio.  
- O **nível de conhecimento esperado** do candidato.  
- O **formato e estilo das questões** da prova.  
- Recursos e recomendações de **materiais oficiais de apoio**.  

👉🏻 Ler o guia nos ajuda a **direcionar os estudos**, evitando conteúdos irrelevantes e priorizando os pontos mais importantes.  
Em outras palavras, é como **conhecer as regras do jogo antes de jogar** - o que aumenta muito as chances de sucesso na certificação!🎉   

[⬆ Voltar ao índice](#índice)

---

## 1. Conceitos de Nuvem  

### 1.1 Benefícios da Nuvem  
- **Alta disponibilidade** → manter os sistemas acessíveis mesmo diante de falhas.  
- **Elasticidade** → ajustar automaticamente a capacidade de acordo com a demanda.  
- **Agilidade** → provisionar e implementar recursos de forma rápida e eficiente.  

---

### 1.2 Princípios de Design da Nuvem  

**Well-Architected Framework**  
**Pilares:**  
- **Excelência operacional** → executar e monitorar sistemas para agregar valor ao negócio e melhorar continuamente processos e procedimentos.  
- **Segurança** → proteger dados, sistemas e ativos por meio de controles de segurança e gerenciamento de riscos.  
- **Confiabilidade** → garantir que a carga de trabalho execute corretamente e se recupere rapidamente de falhas.  
- **Eficiência de desempenho** → usar os recursos de forma eficiente para atender às necessidades do negócio.  
- **Otimização de custos** → evitar gastos desnecessários e otimizar o uso de recursos.  
- **Sustentabilidade** → reduzir impactos ambientais ao executar cargas de trabalho na nuvem.  

---

### 1.3 Migração para a Nuvem  

**Cloud Adoption Framework (AWS CAF)**  
O **AWS CAF** agrupa os recursos em **seis perspectivas** que orientam a migração para a nuvem:  
- **Negócios** → alinhamento da estratégia de TI aos objetivos corporativos.  
- **Pessoas** → desenvolvimento de competências e gestão da mudança organizacional.  
- **Governança** → definição de políticas, compliance e gerenciamento financeiro.  
- **Plataforma** → seleção e arquitetura dos serviços de nuvem.  
- **Segurança** → proteção de dados, aplicações e infraestrutura.  
- **Operações** → gerenciamento contínuo e eficiente do ambiente em nuvem.  

**Benefícios e estratégias de adoção:**  
- **Redução do risco comercial** → migração planejada reduz riscos operacionais e financeiros.  
- **Melhoria do desempenho ESG** (*Environmental, Social and Governance*) → práticas sustentáveis, responsabilidade social e melhor governança corporativa. 
- **Aumento da receita** → possibilita inovação mais rápida, criação de novos produtos e maior alcance de clientes.  
- **Aumento da eficiência operacional** → processos otimizados e redução de custos com infraestrutura física.  

**Estratégias de migração adequadas:**
  - **Réplicas de banco de dados** → permitem migração contínua e com menos downtime.  
  - **Uso do AWS Snowball** → facilita a transferência segura e rápida de grandes volumes de dados para a nuvem.  

---

### 1.4 Conceitos dos Aspectos Econômicos  

**Benefícios da automação:**  
- **Redução de custos operacionais** → menos necessidade de intervenção manual e menor gasto com processos repetitivos.  
- **Otimização do uso de recursos** → utilização eficiente de servidores, armazenamento e rede.  
- **Maior eficiência e escalabilidade** → capacidade de aumentar ou reduzir recursos automaticamente conforme a demanda.  
- **Diminuição de erros manuais** → processos automatizados reduzem falhas humanas.  
- **Aumento da velocidade na entrega de soluções** → provisionamento mais rápido e ágil de novos serviços e aplicações.  

[⬆ Voltar ao índice](#índice)

---

## 2. Segurança e Conformidade  

### 2.1 Modelo de Responsabilidade Compartilhada  
A **segurança da/na nuvem** é uma responsabilidade dividida entre a **AWS** e o **cliente**:  

- **Responsabilidade da AWS** (*Security of the Cloud*) - **DA** nuvem:  
  - Proteger a infraestrutura que executa os serviços da nuvem (hardware, software, redes e instalações).  

- **Responsabilidade do Cliente** (*Security in the Cloud*) - **NA** nuvem:  
  - Configuração segura dos serviços utilizados.  
  - Gerenciamento de dados, aplicações e sistemas operacionais.  
  - Controle de identidades, permissões e criptografia.  

---

### 2.2 Conceitos de Segurança, Governança e Conformidade  

- **Criptografia em trânsito e em repouso** → proteger dados enquanto são transferidos ou armazenados.  
- **AWS Artifact** → fornece acesso a relatórios de conformidade e documentos de auditoria.  
- **Amazon Inspector** → analisa vulnerabilidades e desvios em aplicações e sistemas.  
- **AWS Security Hub** → centraliza alertas de segurança de múltiplos serviços.  
- **Amazon GuardDuty** → serviço de detecção de ameaças e atividades maliciosas.  
- **AWS Shield** → proteção contra ataques DDoS.  
- **AWS CloudWatch** → monitoramento de logs, métricas e eventos em tempo real.  
- **AWS CloudTrail** → auditoria e registro das ações realizadas na conta AWS.  
- **AWS Audit Manager** → auxilia na automação de auditorias ao coletar de forma contínua evidências de uso dos serviços AWS.  
- **AWS Config** → monitora as configurações e mudanças dos recursos gerenciáveis pela AWS para garantir conformidade e segurança.  

---

### 2.3 Gerenciamento de Acesso  

- **IAM (Identity and Access Management)** → serviço que permite criar e gerenciar usuários, grupos, funções e permissões para controlar o acesso aos
recursos da AWS.  
- **Proteção da conta root** → a conta root tem acesso total à conta AWS; deve ser usada apenas para tarefas administrativas críticas e protegida com MFA
(autenticação multifator).  
- **Princípio do menor privilégio** → conceder apenas as permissões estritamente necessárias para que usuários ou serviços executem suas tarefas.  
- **Single Sign-On (SSO)** → permite que usuários acessem múltiplas contas ou aplicações AWS com um único login centralizado, simplificando a gestão de
identidades.  

---

### 2.4 Recursos de Segurança  

- **AWS WAF (Web Application Firewall)** → protege aplicações web contra ataques comuns como SQL injection e cross-site scripting (XSS).  
- **AWS Firewall Manager** → facilita o gerenciamento centralizado de regras de firewall em múltiplas contas e aplicações.  
- **AWS Shield** → serviço de proteção contra ataques DDoS.  
- **Amazon GuardDuty** → detecção de ameaças e atividades maliciosas em contas e workloads AWS.  
- **AWS Trusted Advisor** → recomendações de segurança, performance, economia de custos e tolerância a falhas.  
- **Segurança de terceiros no AWS Marketplace** → soluções de parceiros para proteção adicional, como antivírus, firewalls e monitoramento.  
- **AWS Knowledge Center** → base de conhecimento com artigos e tutoriais sobre segurança e melhores práticas.  
- **AWS Security Center / Blog de Segurança** → atualizações, alertas e boas práticas sobre segurança na nuvem AWS.  

[⬆ Voltar ao índice](#índice)

---

## 3. Tecnologia e Serviços da Nuvem  

### 3.1 Métodos de Implantação e Operação na Nuvem  

- **APIs (Application Programming Interfaces)** → permitem que aplicações interajam programaticamente com os serviços da AWS.  
- **SDKs (Software Development Kits)** → bibliotecas específicas para diferentes linguagens que facilitam o uso das APIs AWS.  
- **CLIs (Command Line Interfaces)** → permitem gerenciar serviços AWS via linha de comando, automatizando tarefas.  
- **Console de Gerenciamento AWS** → interface web para criação, configuração e monitoramento de recursos.  
- **IaC (Infrastructure as Code)** → prática de definir e gerenciar infraestrutura por meio de código, permitindo replicação e automação.  
- **Modelos de implantação:**  
  - **Nuvem (Cloud)** → recursos totalmente gerenciados na AWS.  
  - **Híbrido (Hybrid)** → combinação de recursos on-premises e na nuvem.  
  - **On-Premises** → recursos hospedados localmente, dentro da própria infraestrutura da empresa.  

---

### 3.2 Definir a Infraestrutura Global  

- **Regiões AWS** → localidades geográficas distintas que contêm múltiplos datacenters (AZs).  
- **AZs (Availability Zones)** → zonas de disponibilidade dentro de uma região; isoladas de falhas umas das outras, mas conectadas com alta velocidade.  
- **Locais de borda (Edge Locations)** → pontos de presença para distribuição de conteúdo via Amazon CloudFront e serviços de baixa latência.  
- **Alta disponibilidade** → arquiteturas que garantem que aplicações continuem funcionando mesmo diante de falhas em AZs ou regiões.  
- **Benefícios da infraestrutura global** → redução de latência, resiliência, redundância e escalabilidade para usuários finais.  

---

### 3.3 Serviços Computacionais  

- **Amazon EC2** → instâncias de servidor virtual; disponíveis em tipos otimizados para computação ou para armazenamento, conforme a necessidade da carga
de trabalho.  
- **Containers**:  
  - **ECS (Elastic Container Service)** → serviço gerenciado para executar aplicações em containers.  
  - **EKS (Elastic Kubernetes Service)** → serviço gerenciado baseado em Kubernetes para orquestração de containers.  
- **Serverless**:  
  - **AWS Lambda** → executar código sem gerenciar servidores; paga-se apenas pelo uso efetivo.  
  - **Fargate** → executar containers sem provisionar ou gerenciar servidores, totalmente serverless.  
- **Auto Scaling** → ajusta automaticamente a quantidade de instâncias ou containers de acordo com a demanda, proporcionando elasticidade.  
- **Balanceadores de Carga (Elastic Load Balancer)** → distribuem tráfego entre múltiplas instâncias ou containers, aumentando disponibilidade e desempenho.  

---

### 3.4 Banco de Dados, Serviços e Migração  

**Quando usar bancos de dados na AWS:**  
- **Banco de dados no EC2** → quando você precisa de controle total sobre o banco e o sistema operacional.  
- **Banco de dados gerenciado pela AWS** → quando deseja que a AWS cuide da manutenção, backup, escalabilidade e alta disponibilidade. 

**Tipos de bancos de dados e serviços:**  
- **Relacionais:**  
  - **RDS (Relational Database Service)** → banco de dados relacional gerenciado pela AWS.  
  - **Aurora** → banco de dados relacional compatível com MySQL e PostgreSQL, com alta performance e escalabilidade.  
- **Não Relacionais:**  
  - **DynamoDB** → banco de dados NoSQL totalmente gerenciado e escalável.  
- **Em Memória:**  
  - **ElastiCache** → cache em memória compatível com Redis e Memcached, usado para acelerar aplicações.  

**Ferramentas de migração de banco de dados:**  
- **DMS (Database Migration Service)** → migração de dados para a AWS de forma segura e com mínimo downtime.  
- **AWS SCT (Schema Conversion Tool)** → converte esquemas de banco de dados de diferentes formatos, facilitando migração entre bancos heterogêneos.  

---

### 3.5 Serviços de Rede  

**Componentes da VPC (Virtual Private Cloud):**  
- **Sub-redes (Subnets)** → divisão lógica da VPC, podendo ser públicas ou privadas.  
- **Internet Gateway** → permite comunicação entre a VPC e a internet.  
- **NAT Gateway** → permite que instâncias em sub-redes privadas acessem a internet de forma segura.  

**Segurança na VPC:**  
- **Network ACL (Access Control List)** → regras de entrada e saída aplicadas no nível da sub-rede.  
- **Security Groups** → firewall virtual que controla tráfego de entrada e saída das instâncias.  
- **Amazon Inspector** → analisa vulnerabilidades e desvios de segurança em recursos implantados.  

**Gerenciamento de tráfego e DNS:**  
- **Amazon Route 53** → serviço de DNS gerenciado que fornece roteamento de tráfego e registro de domínios.  

**Conectividade híbrida:**  
- **AWS VPN** → cria conexões seguras entre data centers locais e a VPC.  
- **AWS Direct Connect** → conexão de rede dedicada entre o ambiente local e a AWS, com baixa latência e alta largura de banda.  

---

### 3.6 Serviços de Armazenamento  

- **Amazon S3 (Simple Storage Service)** → armazenamento de objetos altamente escalável, durável e acessível.  
- **S3 Storage Classes** → diferentes classes de armazenamento (Standard, Infrequent Access, Glacier, Deep Archive) que otimizam custo conforme frequência de acesso e requisitos de retenção.  
- **Amazon EBS (Elastic Block Store)** → armazenamento em blocos 'persistente' para instâncias EC2.  
- **Instance Store** → armazenamento em blocos 'temporário' (efêmero), vinculado ao ciclo de vida da instância EC2.  

**Serviços de Arquivos:**  
- **Amazon EFS (Elastic File System)** → serviço de arquivo elástico e escalável para instâncias 'Linux' - protocolo **NFS**.  
- **Amazon FSx** → serviço de arquivo gerenciado compatível com 'Windows File Server' - protocolo **SMB**.  
- **AWS Storage Gateway** → serviço que integra ambientes **on-premisse com a nuvem**, permitindo cache de arquivo local, backup e arquivamento em nuvem em um modelo 'híbrido'.  

**Gerenciamento de dados e backup:**  
- **Políticas de ciclo de vida do S3** → automatizam a transição de objetos entre classes de armazenamento ou exclusão após determinado tempo.  
- **AWS Backup** → serviço centralizado para automação e gerenciamento de backups em múltiplos serviços AWS (RDS, EFS, DynamoDB, etc.).  

---

### 3.7 Serviços de Inteligência Artificial, Machine Learning e Analytics  

**Machine Learning e IA:**  
- **Amazon SageMaker** → serviço para construir, treinar e implantar modelos de machine learning em escala.  
- **Amazon Lex** → criação de chatbots e interfaces conversacionais com reconhecimento de fala e linguagem natural.  
- **Amazon Kendra** → mecanismo de busca inteligente baseado em machine learning para documentos corporativos.  

**Data Analytics:**  
- **Amazon Athena** → consulta de dados diretamente no Amazon S3 usando SQL, sem necessidade de infraestrutura.  
- **Amazon Kinesis** → coleta, processamento e análise de dados em tempo real.  
- **AWS Glue** → serviço de ETL (Extract, Transform, Load => extração, transformação e carga) totalmente gerenciado que ajuda a transformar dados crus em
dados organizados e pesquisáveis.
- **Amazon QuickSight** → serviço de business intelligence para criação de dashboards e visualizações interativas.  

---

### 3.8 Outros Serviços – Categorias  

**Integração de Aplicações:**  
- **Amazon EventBridge** → barramento de eventos para integração entre aplicações e serviços.  
- **Amazon SNS (Simple Notification Service)** → serviço de mensagens e notificações no modelo pub/sub (publish/subscribe - publicar/assinar).  
- **Amazon SQS (Simple Queue Service)** → serviço de filas no esquema FIFO para envio de mensagens assíncronas e desacoplamento de sistemas.  

**Aplicações Empresariais:**  
- **Amazon Connect** → central de atendimento baseada em nuvem.  
- **Amazon SES (Simple Email Service)** → envio de e-mails em escala com alta capacidade de entrega.  

**Capacitação de Cliente:**  
- **AWS Support** → planos de suporte técnico e orientação.  

**Ferramentas para Desenvolvedores:**  
- **AWS CodeBuild** → compila código, executa testes e produz artefatos de software.  
- **AWS CodePipeline** → automação de pipelines de CI/CD.  
- **AWS X-Ray** → rastreamento e depuração de aplicações distribuídas.  

**Computação para Usuário Final:**  
- **Amazon AppStream 2.0** → streaming de aplicativos para usuários finais sem necessidade de instalação local.  
- **Amazon WorkSpaces** → desktops virtuais gerenciados na nuvem.  

**Frontend para Web e Mobile:**  
- **AWS Amplify** → criação e hospedagem de aplicações web e mobile escaláveis.  
- **AWS AppSync** → criação de APIs flexíveis com GraphQL, integrando múltiplas fontes de dados.  

**Internet das Coisas (IoT):**  
- **AWS IoT Core** → conecta dispositivos IoT à nuvem com segurança e escalabilidade.  

[⬆ Voltar ao índice](#índice)

---

## 4. Cobrança, Preços e Suporte  

### 4.1 Comparar Modelos de Preços  

**Principais modelos de preços da AWS:**  
- **Instâncias sob demanda** → paga-se por hora ou segundo de uso, sem compromisso de longo prazo.  
- **Instâncias reservadas** → compromisso de 1 ou 3 anos com desconto significativo sobre o preço sob demanda.  
- **Savings Plans** → compromisso de uso por 1 ou 3 anos que oferece desconto flexível em múltiplos serviços de computação.  
- **Instâncias dedicadas** → servidores físicos dedicados a uma conta, útil por requisitos de conformidade ou licença.  
- **Capacidade reservada** → reserva de capacidade de instâncias EC2 para garantir disponibilidade em períodos críticos.  
- **Instâncias Spot** → aproveita capacidade ociosa da AWS com até 90% de desconto, mas podem ser interrompidas.  

**Custos de dados:**  
- Dados recebidos ou enviados **entre regiões** → podem gerar cobrança de transferência de dados.  
- Dados recebidos ou enviados **na mesma região** → geralmente gratuitos entre serviços AWS, mas depende do serviço específico.  

---

### 4.2 Gerenciamento de Cobrança  

- **AWS Organizations** → gerenciamento centralizado de múltiplas contas AWS, permitindo aplicar políticas e consolidar faturamento.  
- **AWS Budgets** → define limites de gastos e envia alertas quando os custos ou uso ultrapassam os valores planejados.  
- **AWS Cost Explorer** → ferramenta para visualizar, analisar e detalhar os custos e padrões de uso ao longo do tempo.  
- **Tags de alocação de custos** → permitem categorizar recursos para rastrear custos por projeto, departamento ou equipe.  
- **AWS Pricing Calculator** → ajuda a estimar o custo de serviços antes da implementação, considerando diferentes cenários e modelos de preços.  

---

### 4.3 Recursos Técnicos e Opções de Suporte  

**Recursos gerais de suporte e aprendizado:**  
- **AWS Support** → fornece ajuda técnica e orientação para resolução de problemas.  
- **Rede de Parceiros AWS** → parceiros certificados que oferecem consultoria e serviços especializados.  
- **Whitepapers** → documentos oficiais com melhores práticas e guias técnicos.  
- **Blogs e Documentação AWS** → tutoriais, atualizações e informações detalhadas sobre serviços.  
- **AWS Knowledge Center** → base de conhecimento com perguntas frequentes e soluções.  
- **AWS re:Post** → comunidade de perguntas e respostas para desenvolvedores e usuários AWS.  

**Planos de suporte pagos:**  
- **Developer Support** → suporte básico para desenvolvedores.  
- **Business Support** → suporte avançado com respostas rápidas e orientação técnica.  
- **Enterprise Support** → suporte completo para grandes organizações, com consultoria dedicada e arquitetura de referência.  

**Ferramentas de monitoramento:**  
- **AWS Health Dashboard** → monitoramento do status dos serviços e eventos que impactam seus ambientes, ajudando na otimização de custos e mitigação de
riscos.  

[⬆ Voltar ao índice](#índice)

---

✅ Agora é com você!  
Espero que esse guia te ajude a revisar melhor os conteúdos.  
Boa prova e sucesso na sua jornada para se tornar AWS Certified Cloud Practitioner!🚀  

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
