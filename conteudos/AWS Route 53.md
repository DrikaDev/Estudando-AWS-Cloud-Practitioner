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

---

## Políticas de Roteamento do Route 53

O **AWS Route 53** oferece diferentes **políticas de roteamento** para atender a diversas necessidades de distribuição de tráfego:

### 1. Política de Roteamento Simples (Simple routing policy)
- Usada para **um único recurso** que executa uma função específica.  
- Direciona todo o tráfego para esse recurso sem lógica adicional.

### 2. Política de Roteamento Ponderado (Weighted routing policy)
- Útil quando há **vários recursos** disponíveis.  
- Permite **direcionar uma certa porcentagem de tráfego** para cada recurso.

### 3. Política de Roteamento de Latência (Latency routing policy)
- Roteia o tráfego com base na **menor latência de rede** para o usuário.  
- Garante que o usuário receba a resposta mais rápida possível de qualquer região.

### 4. Política de Roteamento de Failover (Failover routing policy)
- Configuração **ativa/passiva**.  
- O recurso principal atende a todo o tráfego, mas se falhar, o Route 53 redireciona para um **recurso de backup**.

### 5. Política de Roteamento de Localização Geográfica (Geolocation routing policy)
- Roteia o tráfego com base na **localização geográfica dos usuários**.  
- Permite customizar conteúdo ou serviços por região.
- Excelente para cumprir requisitos legais.

### 6. Política de Roteamento de Proximidade Geográfica (Geoproximity routing policy)
- Roteia o tráfego com base na **localização geográfica dos recursos**.  
- Pode transferir tráfego de recursos em um local para recursos em outro.  
- Usada **somente para fluxo de tráfego**.

### 7. Política de Roteamento de Resposta Multivalor (Multivalue answer routing policy)
- Permite que o Route 53 responda a consultas DNS com até **oito registros saudáveis selecionados aleatoriamente**.  
- Distribui o tráfego de forma balanceada e aumenta a resiliência.

💡 **Resumo:**  
Essas políticas permitem que você **controle com precisão como o tráfego DNS é distribuído**, garantindo **desempenho, resiliência e experiência otimizada 
para os usuários**.

---

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
