## Elastic Load Balancing (ELB)

O **Elastic Load Balancing (ELB)** é um serviço da AWS que distribui automaticamente o tráfego de rede ou aplicação entre múltiplos 
destinos, como instâncias EC2, contêineres e endereços IP.  
Seu objetivo é melhorar:

- ✅ Disponibilidade
- ✅ Escalabilidade
- ✅ Tolerância a falhas

O ELB também verifica o **status dos recursos** e envia tráfego apenas para instâncias **saudáveis**, ou seja, instâncias 
que passaram nos testes de verificação de integridade (health checks).  

Ele dimensiona sua capacidade automaticamente conforme a demanda cresce ou diminui.

---

### Instâncias "saudáveis" no contexto do ELB

No **Elastic Load Balancing (ELB)**, o termo *saudável (healthy)* é usado para indicar que uma instância ou destino está funcionando 
corretamente e apto a receber tráfego.  
Isso é verificado por meio de **health checks** configurados no balanceador.  

#### Como funciona?

O ELB realiza verificações periódicas, como:

- Checar uma rota específica (ex.: `/health`)
- Verificar a porta usada pela aplicação (ex.: porta 80)
- Testar via HTTP, HTTPS ou TCP

Comportamento esperado:

| Situação | Status | Ação do ELB |
|--------|--------|-------------|
Instância responde corretamente aos health checks | **Healthy (Saudável)** ✅ | Recebe tráfego |
Instância falha repetidamente nos health checks | **Unhealthy (Não saudável)** ❌ | Tráfego é redirecionado para outras instâncias |

#### Sinônimos úteis para "saudáveis"

Se o termo parecer estranho em português, você pode usar também:

- Instâncias **ativas**
- Instâncias **disponíveis**
- Instâncias **em bom estado**
- Destinos **aprovados nos health checks**

> 💡 Observação: O termo *Healthy* é amplamente usado na documentação AWS e em provas, então vale se acostumar com ele.

---

### Tipos de Load Balancers

Cada aplicação tem necessidades diferentes, e um único tipo de balanceador não atenderia bem a todos os cenários.  
Por isso, a AWS oferece tipos de **Load Balancers** para otimizar:  

| Tipo | Camada OSI | Protocolos | Caso de Uso | Benefícios |
|------|-----------|------------|-------------|------------|
| **Application Load Balancer (ALB)** | Layer 7 | HTTP / HTTPS | Aplicações Web, APIs, microserviços | Roteamento avançado por URL, host, path; integração com containers |
| **Network Load Balancer (NLB)** | Layer 4 | TCP / UDP | Tráfego de alta performance e baixa latência | Suporta milhões de requisições por segundo, IP estático |
| **Classic Load Balancer (CLB)** *(legado)* | Layer 4 & 7 (básico) | HTTP / HTTPS / TCP | Sistemas antigos | Recomendado apenas para workloads existentes |

---

### Resumo rápido para provas

- **ALB → HTTP/HTTPS, roteamento Layer 7**
- **NLB → TCP/UDP, alta performance, baixa latência, milhões de conexões**
- **CLB → legado (não usar em novos projetos)**

---

### Dica para memorizar

> **A**pplication = **A**plicações Web (HTTP/HTTPS)  
> **N**etwork = **N**ível de rede (TCP/UDP, alta performance)

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
