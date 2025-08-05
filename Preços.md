## 💵 Definição de Preços do Amazon EC2

Com o **Amazon EC2**, você paga apenas pelo tempo de computação que usar.  
A AWS oferece diversas opções de preço para diferentes necessidades:

## 🔹 Instâncias Sob Demanda
- **Características**  
  - Ideais para cargas de trabalho irregulares e de curto prazo que **não podem ser interrompidas**.  
  - Não exigem custos antecipados nem contratos mínimos.  
  - Executam continuamente até que sejam interrompidas.  
- **Casos de uso comuns**  
  - Desenvolvimento e teste de aplicações  
  - Execução de aplicações com padrões de uso imprevisíveis  
- **Observação**  
  - Não são recomendadas para cargas de trabalho de **longo prazo (≥ 1 ano)**, pois podem sair mais caras que as Reservadas.

## 🔹 Instâncias Reservadas
Oferecem **descontos significativos** em comparação às instâncias sob demanda, em troca de um **compromisso de 1 ou 3 anos**.  

### Tipos:
- **Standard Reserved Instances**
  - Maior economia com período de 3 anos.  
  - Indicadas quando você já sabe:
    - Tipo e tamanho da instância (ex.: `m5.xlarge`)  
    - Sistema operacional (ex.: Windows Server, Red Hat Enterprise Linux)  
    - Tenancy (padrão ou dedicado)  
  - Pode especificar **Zona de Disponibilidade** para reserva de capacidade garantida.  

- **Instâncias Reservadas Conversíveis**
  - Permitem **flexibilidade** de mudar tipo de instância ou Zona de Disponibilidade.  
  - Desconto um pouco menor que as Standard.  

### No final do período de vigência
Se não renovar:  
- A instância continua executando como **sob demanda**, ou  
- Você pode adquirir uma nova instância reservada compatível.  

## 🔹 Savings Plans
Oferecem até **72% de desconto** em relação às tarifas sob demanda.  

- **Como funcionam**
  - Você se compromete com um **gasto por hora** (ex.: USD 10/h) por 1 ou 3 anos.  
  - Qualquer uso até o compromisso é cobrado com desconto.  
  - Uso além do compromisso é cobrado como sob demanda.  

- **Vantagens**
  - Mais flexíveis que as Instâncias Reservadas  
  - Não exigem especificar previamente:
    - Tipo e tamanho da instância  
    - Sistema operacional  
    - Tenancy  
  - Abrangem qualquer instância de uma **mesma família em uma Região**.  

- **Ferramenta útil**
  - O [AWS Cost Explorer](https://aws.amazon.com/aws-cost-management/aws-cost-explorer/) ajuda a analisar:
    - Seu uso dos últimos 7, 30 ou 60 dias  
    - Recomendações personalizadas de Savings Plans  

## 🔹 Instâncias Spot
- **Características**
  - Utilizam capacidade não usada do EC2  
  - Até **90% mais baratas** que as sob demanda  
  - Podem ser interrompidas se a capacidade for necessária para outros clientes  
- **Importante**
  - A AWS pode **pedir de volta a instância a qualquer momento**, avisando com **2 minutos de antecedência** para que você finalize e salve seu trabalho.  
  - Você pode retomar mais tarde, se necessário.  
  - Portanto, as cargas de trabalho **precisam tolerar interrupções**.  
- **Casos de uso comuns**
  - Processamento em segundo plano  
  - Tarefas em **batch**  
  - Trabalhos com horários de início e término flexíveis  
- **Exemplo**
  - Processar dados de uma pesquisa em horários de menor demanda.   

⚠️ *Não são ideais para aplicações que exigem disponibilidade contínua (como desenvolvimento e testes críticos).*

## 🔹 Hosts Dedicados
- **Descrição**
  - Servidores físicos exclusivos para o cliente  
  - Permitem uso de licenças próprias (por soquete, núcleo ou VM)  
- **Aquisição**
  - Disponíveis sob demanda ou por reserva  
- **Custo**
  - São a opção **mais cara** do Amazon EC2  

---

## Comparativo Resumido

| Tipo de Instância         | Vantagem Principal                       | Casos de Uso Comuns                                    |
|----------------------------|------------------------------------------|-------------------------------------------------------|
| **Sob Demanda**           | Flexibilidade máxima                     | Dev/teste, cargas imprevisíveis                       |
| **Reservadas (Standard)** | Maior economia (até 3 anos)              | Aplicações estáveis, workloads previsíveis            |
| **Reservadas (Conversíveis)** | Flexibilidade com bom desconto        | Workloads que podem mudar tipo de instância ou região |
| **Savings Plans**         | Desconto alto sem especificar instância  | Diversos workloads dentro de uma família de instância |
| **Spot**                  | Economia extrema (até 90%)               | Processamento flexível, tarefas tolerantes a falhas   |
| **Hosts Dedicados**       | Controle total + licenciamento próprio   | Workloads com requisitos de conformidade/licenciamento |

---
