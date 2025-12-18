## Elastic Load Balancing (ELB)

## Índice

- [Instâncias **saudáveis** no contexto do ELB](#instâncias-saudáveis-no-contexto-do-elb)
- [Tipos de Load Balancers](#tipos-de-load-balancers)
- [Visibilidade da integridade das instâncias no ELB](#visibilidade-da-integridade-das-instâncias-no-elb)
- [Componentes do Application Load Balancer (ALB)](#componentes-do-application-load-balancer-alb)
  - [Load Balancer](#load-balancer)
  - [Listener](#listener)
  - [Rules (Regras)](#rules-regras)
  - [Target Group (Grupo de Destino)](#target-group-grupo-de-destino)
  - [Targets (Destinos)](#targets-destinos)
  - [Health Checks](#health-checks)
- [Fluxo de funcionamento do ALB](#fluxo-de-funcionamento-do-alb)
- [Resumo geral (nível prova AWS)](#resumo-geral-nível-prova-aws)
- [Dica para memorizar](#dica-para-memorizar)


O **Elastic Load Balancing (ELB)** é um serviço da AWS que distribui automaticamente o tráfego de rede ou aplicação entre múltiplos
destinos, como instâncias EC2, contêineres e endereços IP.

Seu objetivo é melhorar:
- ✅ Disponibilidade
- ✅ Escalabilidade
- ✅ Tolerância a falhas

O ELB também verifica o **status dos recursos** e envia tráfego apenas para instâncias **saudáveis**, ou seja, instâncias
que passaram nos testes de verificação de integridade (*health checks*).

Ele dimensiona sua capacidade automaticamente conforme a demanda cresce ou diminui,
em conjunto com serviços como o **Auto Scaling**.

## Instâncias **saudáveis** no contexto do ELB

No **Elastic Load Balancing (ELB)**, o termo *saudável (healthy)* indica que uma instância ou destino
está funcionando corretamente e apto a receber tráfego.

Isso é verificado por meio de **health checks** configurados no balanceador.

### Como funciona?

O ELB realiza verificações periódicas, como:

- Checar uma rota específica (ex.: `/health`)
- Verificar a porta usada pela aplicação (ex.: porta 80)
- Testar via HTTP, HTTPS ou TCP

### Comportamento esperado

| Situação | Status | Ação do ELB |
|--------|--------|-------------|
| Instância responde corretamente aos health checks | **Healthy (Saudável)** ✅ | Recebe tráfego |
| Instância falha repetidamente nos health checks | **Unhealthy (Não saudável)** ❌ | Tráfego é redirecionado |

### Sinônimos úteis para "saudável"

- Instâncias **ativas**
- Instâncias **disponíveis**
- Instâncias **em bom estado**
- Destinos **aprovados nos health checks**

> 💡 Observação: o termo *Healthy* é amplamente usado na documentação da AWS e em provas,
então vale se acostumar com ele.

## Tipos de Load Balancers

Cada aplicação tem necessidades diferentes, e um único tipo de balanceador não atenderia bem a todos os cenários.
Por isso, a AWS oferece diferentes tipos de **Load Balancers**.

| Tipo | Camada OSI | Protocolos | Caso de uso | Benefícios |
|------|-----------|------------|-------------|------------|
| **Application Load Balancer (ALB)** | Layer 7 | HTTP / HTTPS | Aplicações Web, APIs, microserviços | Roteamento avançado por URL, host e path |
| **Network Load Balancer (NLB)** | Layer 4 | TCP / UDP | Alta performance e baixa latência | Milhões de requisições por segundo, IP estático |
| **Classic Load Balancer (CLB)** *(legado)* | Layer 4 & 7 (básico) | HTTP / HTTPS / TCP | Sistemas antigos | Apenas para workloads existentes |

## Visibilidade da integridade das instâncias no ELB

Entre os tipos de Load Balancer, o **Application Load Balancer (ALB)** é o que oferece
**maior visibilidade sobre a integridade das instâncias de destino**.

Isso ocorre porque o ALB utiliza **Target Groups**, onde é possível visualizar claramente
o status de cada destino.

- Exibe o status de saúde **por Target Group**
- Permite identificar exatamente **quais instâncias ou destinos estão falhando**
- Estados possíveis:
  - **Healthy** ✅
  - **Unhealthy** ❌
- Health checks configuráveis:
  - Path (ex.: `/health`)
  - Porta
  - Protocolo (HTTP/HTTPS)
  - Códigos de resposta HTTP

Essa visibilidade facilita:
- Diagnóstico de falhas
- Integração com Auto Scaling
- Operação e *troubleshooting* da aplicação

## Componentes do Application Load Balancer (ALB)

O **Application Load Balancer (ALB)** é composto por vários componentes que trabalham juntos
para receber, avaliar e encaminhar o tráfego HTTP/HTTPS.

### Load Balancer
- Recurso principal do ALB
- Fornece um **endpoint DNS** público ou interno
- Recebe o tráfego dos clientes

### Listener
- Escuta solicitações de conexão dos clientes
- Associado a uma **porta** e **protocolo** (HTTP ou HTTPS)
- Avalia as solicitações recebidas

**Exemplo:**  
HTTP :80, HTTPS :443

### Rules (Regras)
- Definem **como o tráfego será roteado**
- Avaliam condições como:
  - Host (domínio)
  - Path (URL)
  - Headers
  - Query strings

### Target Group (Grupo de Destino)
- Define **para onde o tráfego será encaminhado**
- Pode conter:
  - Instâncias EC2
  - Endereços IP
  - Funções Lambda
- Executa **health checks** nos destinos

### Targets (Destinos)
- Recursos que efetivamente recebem o tráfego
- Precisam estar **healthy** para receber requisições

### Health Checks
- Verificações periódicas de integridade
- Determinam se um destino está:
  - **Healthy**
  - **Unhealthy**

## Fluxo de funcionamento do ALB

1. O cliente envia uma requisição
2. O **Load Balancer** recebe o tráfego
3. O **Listener** escuta a conexão
4. As **Rules** avaliam a requisição
5. O **Target Group** encaminha para um **Target saudável**

## Resumo geral (nível prova AWS)

- **Listener** → recebe conexões dos clientes
- **Rules** → decidem o roteamento
- **Target Group** → define os destinos
- **Health Checks** → garantem integridade

### Comparação rápida

| Load Balancer | Visibilidade da saúde |
|--------------|-----------------------|
| **ALB** | ✅ Detalhada (Target Groups) |
| NLB | ⚠️ Básica (nível de conexão) |
| CLB | ❌ Limitada |

**Resumo final:**  
Se a necessidade for **monitorar claramente a integridade das instâncias de destino**,
o balanceador indicado é o **Application Load Balancer (ALB)**.

### Dica para memorizar

> **A**pplication = **A**plicações Web (HTTP/HTTPS)  
> **N**etwork = **N**ível de rede (TCP/UDP, alta performance)

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
