## Amazon CloudWatch

- [Sobre o Amazon CloudWatch](#sobre-o-amazon-cloudwatch)  
- [Métricas no Amazon CloudWatch](#métricas-no-amazon-cloudwatch)
- [Políticas de Escalabilidade com Amazon CloudWatch](#políticas-de-escalabilidade-com-amazon-cloudwatch)
- [Eventos no Amazon CloudWatch](#eventos-no-amazon-cloudwatch)  
- [Logs (Registros) no Amazon CloudWatch](#logs-registros-no-amazon-cloudwatch)

## Sobre o Amazon CloudWatch

O **Amazon CloudWatch** é um serviço de **monitoramento de recursos e aplicativos** da AWS.  
Ele fornece visibilidade completa sobre **utilização de recursos, desempenho de aplicativos e integridade operacional**.  

### Funcionalidades Principais
- **Coleta e rastreamento de métricas**  
  Permite monitorar métricas em tempo real de diversos serviços da AWS.  

- **Monitoramento de logs**  
  Coleta, armazena e analisa arquivos de log para identificar erros ou padrões.  

- **Resposta automática a alterações**  
  Usa dados operacionais (logs e métricas) para acionar **ações automáticas** diante de mudanças no sistema.  

### Integração com Serviços AWS
O CloudWatch funciona de forma integrada com vários serviços, como:  
- **Amazon EC2**  
- **Amazon S3**  
- **Amazon DynamoDB**  
- **Amazon ECS**  
- **AWS Lambda**  
- Entre outros.  

### Benefícios
- Visibilidade em tempo real da infraestrutura.  
- Detecção proativa de falhas ou degradação de desempenho.  
- Ações automatizadas para manter a **resiliência e disponibilidade**.  

## Métricas no Amazon CloudWatch

No **Amazon CloudWatch**, **métricas** são o conceito fundamental de monitoramento.  
Elas representam um **conjunto ordenado de pontos de dados** publicados no serviço.  

**Definição:**  
- Uma **métrica** é como uma **variável a ser monitorada**.  
- Os **pontos de dados** representam os valores dessa variável ao longo do tempo.  
- Cada ponto de dado contém um **registro de data e hora**.  

### Estrutura de uma Métrica
- **Nome** – identifica a métrica.  
- **Namespace** – agrupamento lógico que contém a métrica.  
- **Dimensões** – até **30 atributos** que especificam características adicionais da métrica.  

### Estatísticas
Você pode recuperar **estatísticas** sobre os pontos de dados como uma **série temporal ordenada**, permitindo análises de desempenho e comportamento ao longo do tempo.  

### Fontes de Métricas
O CloudWatch fornece métricas para **todos os serviços da AWS**, como:  
- EC2 (ex.: utilização de CPU, memória).  
- S3 (ex.: número de solicitações, bytes armazenados).  
- DynamoDB, ECS, Lambda e muito mais.  

💡 **Em resumo:**  
As métricas são a **base do monitoramento no CloudWatch**, permitindo rastrear, analisar e reagir a mudanças nos recursos da AWS.  

## Políticas de Escalabilidade com Amazon CloudWatch

O **Amazon CloudWatch** é o serviço responsável por **monitorar métricas** e **disparar ações de escalabilidade**.  
Quem executa a escalabilidade de fato é o **Amazon EC2 Auto Scaling**, com base nas métricas e alarmes do CloudWatch.

### 🔁 Principais políticas de escalabilidade (Auto Scaling + CloudWatch)

### 🔹 Target Tracking Scaling
- Mantém uma **métrica em um valor-alvo**
- A AWS ajusta automaticamente a capacidade
- **Mais simples e mais usada**

**Exemplo:**
- Manter CPU média em **50%**
- Se subir → adiciona instâncias  
- Se cair → remove instâncias

📌 Métricas comuns:
- CPUUtilization
- RequestCountPerTarget (ALB)
- Utilização de memória (custom metric)

### 🔹 Step Scaling
- Ajusta a capacidade em **etapas**, com base na intensidade do alarme
- Usa **CloudWatch Alarms**
- Permite maior controle

**Exemplo:**
- CPU > 60% → +1 instância  
- CPU > 80% → +3 instâncias  

📌 Ideal quando você quer respostas diferentes para cargas diferentes.

### 🔹 Simple Scaling (legado)
- Baseado em **um único alarme**
- Executa **uma única ação**
- Usa cooldown obrigatório
- ⚠️ Considerado **obsoleto** (não recomendado)

**Exemplo:**
- CPU > 70% → +1 instância

### 🔹 Scheduled Scaling
- Escalabilidade baseada em **horários definidos**
- Não depende de métricas
- Ótimo para cargas previsíveis

**Exemplo:**
- Aumentar instâncias às 8h
- Reduzir instâncias às 20h

### 🔹 Predictive Scaling
- Usa **Machine Learning**
- Analisa padrões históricos
- Escala **antes** da demanda aumentar

**Exemplo:**
- E-commerce que sempre cresce em datas específicas

📌 Funciona junto com Target Tracking.

### 🧠 Relação CloudWatch x Auto Scaling
- **CloudWatch**
  - Coleta métricas
  - Cria alarmes
- **Auto Scaling**
  - Executa as ações de escala
  - Adiciona ou remove instâncias

👉 CloudWatch **decide quando**, Auto Scaling **executa**.

### 🧠 Resumo rápido

| Política | Uso principal |
|--------|---------------|
| Target Tracking | Manter métrica em valor fixo |
| Step Scaling | Controle detalhado por níveis |
| Simple Scaling | Legado (evitar) |
| Scheduled Scaling | Escala por horário |
| Predictive Scaling | Escala antecipada com ML |

## Eventos no Amazon CloudWatch

O **AWS CloudWatch Events** é um serviço que fornece um método simplificado e sistemático para **responder a alterações** em todo o ambiente AWS.  

Essas alterações podem variar desde uma **mudança de estado simples** (como uma instância EC2 sendo iniciada ou interrompida) até condições mais **complexas**.  

### 🔎 Padrões de Evento
Você pode:  
- Definir **padrões de eventos** para monitorar recursos da AWS em busca de alterações específicas.  
- Agendar tarefas no estilo **cron jobs** diretamente no serviço.  

### ⚡ Ações de Resposta
Quando um evento ocorre, você pode acionar diferentes tipos de ações, como:  
- Executar uma **função Lambda**.  
- Enviar uma **notificação via SNS**.  
- Iniciar uma **política de Auto Scaling**.  
- Outras opções de integração com serviços AWS.  

### 🎯 Benefício
O **CloudWatch Events** ajuda a:  
- **Automatizar** tarefas.  
- **Responder rapidamente** a alterações.  
- Aumentar a **eficiência operacional** do ambiente AWS.  

💡 Em resumo: o CloudWatch Events é essencial para criar sistemas **reativos e automatizados** na AWS.  

## Logs (Registros) no Amazon CloudWatch

O **AWS CloudWatch Logs** permite **monitorar, armazenar e acessar** arquivos de log de diferentes fontes, como:  
- Instâncias do **Amazon EC2**  
- **AWS CloudTrail**  
- Outras aplicações e serviços da AWS  

### 🔎 Centralização
- Consolida os **logs de todos os sistemas, aplicativos e serviços** em um único local.  
- Oferece um serviço altamente **escalável e confiável** para análise.  

### ⚡ Funcionalidades
- **Visualizar e pesquisar** logs em tempo real.  
- **Definir alarmes** baseados em métricas extraídas dos logs.  
- **Correlacionar** logs com outros dados operacionais no CloudWatch.  
- **Integrar com AWS Lambda**, permitindo respostas rápidas a eventos críticos.  

### 🎯 Benefícios
- Melhora o **monitoramento contínuo** de recursos.  
- Facilita a **detecção e solução de problemas**.  
- Fornece **visibilidade unificada** sobre a operação da sua infraestrutura.  

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
