# 💳 Créditos de CPU

Você já ouviu falar em **"Créditos de CPU"**? 🤔  

Sabia que as instâncias do **Amazon EC2** do tipo *burstable* (como T2 e T3) utilizam créditos de CPU para gerenciar seu desempenho?  

Se você, assim como eu, não sabia o que era nem uma coisa nem outra, veja abaixo e tire suas dúvidas de uma vez por todas! 😉

---

## 1. Primeiro, o que é uma instância "burstable"?

Uma instância *burstable* é um tipo de instância do Amazon EC2 projetada para ter um **desempenho básico (linha de base) constante**,  
mas que pode **“explodir” (burst)** e oferecer mais CPU temporariamente quando necessário, usando 'créditos de CPU'.  

Uma instância burstable serve para equilibrar **custo e desempenho**, especialmente em cenários onde a aplicação **não usa muita CPU o 
tempo todo**, mas precisa de **picos de desempenho ocasionais**.  

Em outras palavras:  
- Ela garante um nível mínimo de CPU (linha de base).  
- Quando sua aplicação precisa de mais 'poder', ela **estoura a linha de base** e usa créditos acumulados.  
- Quando os créditos acabam, a instância volta ao desempenho de **linha de base**.  

**E para que serve na prática?**  
- Serve para rodar sites pequenos ou blogs com tráfego variável.  
- Para ambientes de teste ou desenvolvimento, que não precisam de CPU constante.  
- Para aplicações internas ou *batch jobs* com picos curtos de processamento.
  
  > 'Batch jobs' são processos ou tarefas de computador que são executados em lote, ou seja, em grupo e de forma programada,
  > sem interação do usuário durante a execução, como por exemplo, gerar relatórios financeiros no final do dia, enviar emails em massa
  > ou newsletters, fazer backup ou sincronização de dados.

**Famílias burstable da AWS:**  
- T2 (mais antiga)  
- T3 e T3a  
- T4g (baseada em Graviton, ARM)  

**Resumo:**  
Uma instância *burstable* é como um carro híbrido 🚗⚡: ela anda tranquilamente em modo econômico (linha de base),  
mas pode **acelerar forte (burst)** por um tempo se tiver “combustível extra”, ou seja, os **créditos de CPU**.

⚠️ **Dica:** Se a aplicação precisar de alto desempenho constante, use **instâncias de CPU dedicada** (como M5, C5 ou R5).

---

## 2. O que são "Créditos de CPU"?

**Créditos de CPU** são como uma **“moeda”** usada pelas instâncias *burstable* para medir e controlar o uso do processador.  

**Como funciona?**  
- Enquanto a instância está ociosa → ela **acumula créditos**.  
- Enquanto a instância processa tarefas → ela **gasta créditos** para aumentar o desempenho além do nível de linha de base.  

👉🏻 **1 crédito de CPU = 1 minuto de uso de 100% de um núcleo vCPU.**

---

## 3. Como conseguir créditos de CPU?

Você **não compra créditos de CPU** (a não ser no **modo ilimitado**).  
Eles são **gerados automaticamente** pela própria instância *burstable*.  

**Funcionamento:**  
- Cada tipo de instância (T2, T3, T4g) tem uma **taxa de geração de créditos por hora**, definida pela AWS.  
- Enquanto a instância está ociosa ou usando menos CPU que a linha de base, ela **acumula créditos**.  
- Esses créditos ficam guardados em um **saldo de créditos de CPU**.  

**Exemplo prático (t2.micro):**  
- Linha de base: ~10% de 1 vCPU  
- Geração de créditos: 6 créditos por hora  
- Se a instância usar só 5% de CPU, acumula créditos  
- Mais tarde, se precisar de 100% da CPU, ela gasta os créditos acumulados  

⚠️ **Atenção:**  
- Os créditos expiram em 7 dias se não forem usados.  
- Se a instância usar mais CPU do que os créditos disponíveis permitem → ela volta ao **nível de linha de base** (mais lenta).  

### 🔹 Modo ilimitado
- Permite que a instância continue **acima da linha de base mesmo sem créditos**  
- Você **paga pelos créditos extras usados**, mas garante desempenho constante durante picos  

---

## 4. Ciclo dos Créditos de CPU

`Linha de base ──> Ociosa: acumula créditos ──> Pico de uso: gasta créditos ──> Créditos esgotados ──> Retorna à linha de base`

💡 Resumo:
Você consegue créditos de CPU automaticamente quando sua instância usa menos CPU que a linha de base → é como acumular energia em uma 
bateria 🔋 para gastar depois.

---

## 5. Dica extra

Você pode monitorar o saldo de créditos da sua instância usando o **Amazon CloudWatch** para saber se está perto de atingir o limite e 
planejar o uso ou ativar o "modo ilimitado".  
Você pode ativar o "modo ilimitado" de duas maneiras: pelo Console AWS ou pela AWS CLI.  

👉🏻 **Lembre-se:**  
Ative o "modo ilimitado" apenas se sua aplicação tiver picos frequentes de CPU ou precisar de desempenho acima da linha de base 
continuamente.  
Caso contrário, a instância padrão (burstable) já é suficiente e evita custos extras. 😉

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
