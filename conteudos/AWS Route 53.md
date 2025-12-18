## AWS Route 53

O **AWS Route 53** é um serviço de DNS escalável e altamente disponível, que permite a desenvolvedores e empresas direcionar solicitações de usuários para 
os aplicativos corretos na internet de forma **confiável e econômica**, garantindo que o acesso aos serviços seja **rápido e estável**.  

### O que é um DNS?
O DNS (Domain Name System) é um **Sistema de Nomes de Domínio** que traduz "nomes amigáveis" como www.google.com.br em endereços IP como 216.239.38.120 
que é como os computadores usam para se comunicar.  

### Funcionalidades principais

- **Roteamento de usuários**  
  Conecta as solicitações dos usuários à infraestrutura AWS, como:  
  - Instâncias do **Amazon EC2**  
  - **Amazon Elastic Load Balancers**  
  - **Buckets do Amazon S3**  
  Também pode rotear usuários para infraestrutura **fora da AWS**.

- **Serviço de registro de domínio**  
  Facilita a **compra, transferência e gerenciamento de domínios** sem lidar com a complexidade do protocolo DNS.

- **Failover de DNS e verificações de integridade**  
  Permite criar **rotas seguras e confiáveis**, garantindo que o tráfego seja redirecionado apenas para recursos saudáveis.

- **Configuração de TTL (Time To Live) personalizada**  
  Permite ajustar o **tempo de vida dos registros DNS**, controlando a frequência com que os clientes consultam o DNS.

💡 **Resumo:**  
O Route 53 funciona como um **diretor de tráfego da Internet**, garantindo que os usuários sejam conectados de forma confiável à aplicação certa, 
seja dentro ou fora da AWS, enquanto simplifica o gerenciamento de domínios e registros DNS.

👉🏻 O nome **"Route 53"** faz referência à porta 53, que é a porta padrão usada para comunicação DNS em redes.

## Políticas de Roteamento do Route 53

O **AWS Route 53** oferece diferentes **políticas de roteamento** para atender a diversas necessidades de distribuição de tráfego.  
As **políticas de roteamento do Amazon Route 53** definem **como o DNS responde às consultas**, ou seja, **para onde o tráfego será direcionado**.

### 🔹 Simple Routing (Roteamento Simples)
- Retorna **um único recurso** (IP ou nome DNS)
- Não realiza verificação de saúde (Health Check)
- Ideal para arquiteturas simples

**Exemplo:**  
Um único servidor web atendendo o domínio.

### 🔹 Weighted Routing (Roteamento Ponderado)
- Distribui o tráfego entre vários recursos com base em **pesos**
- Muito usado para **testes A/B** ou **migrações graduais**

**Exemplo:**  
- 80% do tráfego para a versão antiga  
- 20% para a nova versão

### 🔹 Latency-based Routing (Baseado em Latência)
- Direciona o usuário para a região AWS com **menor latência**
- Focado em **melhorar a performance**
- Garante que o usuário receba a resposta mais rápida possível de qualquer região

**Exemplo:**  
Usuários do Brasil → `sa-east-1`  
Usuários da Europa → `eu-west-1`

### 🔹 Failover Routing
- Configuração **ativa/passiva**  
- Usado para **alta disponibilidade**
- Define um recurso **primário** e outro **secundário**
- Funciona em conjunto com **Health Checks**
- O recurso principal atende a todo o tráfego, mas se falhar, o Route 53 redireciona para um **recurso de backup**

**Exemplo:**  
Se o recurso primário falhar, o tráfego é direcionado automaticamente para o secundário.

### 🔹 Geolocation Routing (Geolocalização)
- Direciona o tráfego com base na **localização do usuário** (país ou continente)
- Permite customizar conteúdo ou serviços por região
- Muito usado para cumprir **requisitos legais** ou **conteúdo regional**

**Exemplo:**  
Usuários do Brasil acessam uma versão específica do site.

### 🔹 Geoproximity Routing
- Direciona o tráfego com base na **distância geográfica** entre usuário e recurso
- Permite ajustar o alcance usando **bias**
- Requer o **Route 53 Traffic Flow**
- Pode transferir tráfego de recursos em um local para recursos em outro.  
- Usada **somente para fluxo de tráfego**.

**Exemplo:**  
Aumentar ou reduzir artificialmente a área de influência de uma região.

### 🔹 Multivalue Answer Routing
- Retorna até **oito registros saudáveis selecionados aleatoriamente**  
- Realiza um balanceamento simples no lado do cliente
- Suporta **Health Checks**
- Distribui o tráfego de forma balanceada e aumenta a resiliência

**Exemplo:**  
Vários servidores web respondendo ao mesmo domínio.

💡 **Resumo:**  
Essas políticas permitem que você **controle com precisão como o tráfego DNS é distribuído**, garantindo **desempenho, resiliência e experiência otimizada 
para os usuários**.

## Verificações de Saúde no Route 53

As **verificações de integridade (health checks)** do **Route 53** permitem monitorar a saúde e o desempenho de aplicativos, redes e servidores.  

### Principais Funcionalidades
- **Monitoramento de recursos específicos**  
  Ex.: servidores web, servidores de e-mail ou endpoints personalizados.  

- **Redirecionamento automático de tráfego**  
  Caso a verificação de integridade falhe, o Route 53 **redireciona o tráfego para recursos saudáveis**.  

- **Execução periódica**  
  As verificações são realizadas em intervalos configuráveis para **detectar problemas rapidamente**.  

- **Integração com CloudWatch**  
  Fornece **métricas detalhadas, gráficos e alarmes** para acompanhar a saúde dos recursos monitorados.  

### Benefícios
- Detecção de falhas antes dos usuários finais.  
- Possibilidade de configurar **alarmes** para notificações imediatas.  
- Maior **resiliência e disponibilidade** das aplicações.  

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
