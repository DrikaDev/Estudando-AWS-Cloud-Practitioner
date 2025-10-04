## 🧪 Lab 263 - Criar Sub-redes e Alocar Endereços IP em uma Amazon VPC

### Índice

[Introdução](#introdução)  
[Cenário](#cenário)  
[Diagrama do Cliente](#diagrama-do-cliente)  
[1. Investigar as necessidades do cliente](#1-investigar-as-necessidades-do-cliente)  
[2. Enviar a resposta ao cliente](#2-enviar-a-resposta-ao-cliente)

---

### Introdução

Neste laboratório, vamos: 
- Resumir o cenário do cliente.
- Criar uma Amazon Virtual Private Cloud (Amazon VPC) e entender como criar sub-redes e alocar endereços IP.
- Familiarizar-se com o Console de Gerenciamento da Amazon Web Services (AWS).
- Desenvolver uma solução para o problema do cliente neste laboratório.

---

### Cenário

Sua função é como **engenheiro de suporte de nuvem na AWS**.  
Durante o seu turno, um cliente de uma startup solicita assistência com relação a um problema de rede dentro de sua infraestrutura da AWS.  
Abaixo está o e-mail referente à arquitetura do cliente:  

<img width="1182" height="514" alt="image" src="https://github.com/user-attachments/assets/6b065040-6784-4b53-92c6-074e695f12a1" />

### Diagrama do Cliente

A arquitetura da VPC do cliente consiste em:

- Uma VPC que requer aproximadamente **15.000 endereços IP** para a sede em Seattle.
- Um **gateway de internet**.
- Uma **sub-rede pública** que requer cerca de **50 endereços IP** para o departamento de operações.

<img width="867" height="569" alt="image" src="https://github.com/user-attachments/assets/38b776f3-e28b-465b-9e7d-9d6c5e55fa8f" />  

> **Figura:** na arquitetura da VPC do cliente, ele precisa de aproximadamente 15.000 endereços IP para a sede em Seattle e 50 endereços IP para o departamento de operações, que estarão na sub-rede pública.

---

### 1. Investigar as necessidades do cliente

**Revisão rápida: O que é uma VPC?**  

Uma **VPC (Virtual Private Cloud)** é como um **data center na nuvem**, isolado logicamente de outras redes virtuais.  
Ela permite ativar e iniciar recursos da AWS em minutos.  

- Recursos dentro de uma VPC se comunicam através de **endereços IP privados**.  
- Uma instância precisa de **endereço IP público** para se comunicar fora da VPC.  
- A VPC precisará de **recursos de rede**, como **gateway de internet** e **tabela de rotas**, para acessar a internet.  
- Um **bloco CIDR** é um intervalo de **endereços IP privados** usado dentro da VPC (por exemplo, `/16` ao lado de um endereço IP).  
- Uma **sub-rede** é um intervalo de endereços IP dentro da VPC.  

Para calcular intervalos CIDR: [Subnet Calculator](https://www.subnet-calculator.com/)  
Para referência de endereços IP privados: [RFC 1918](https://datatracker.ietf.org/doc/html/rfc1918)

---

### Cenário da tarefa

O cliente **Paulo**, iniciante na AWS, precisa de:

- Cerca de **15.000 endereços IP privados** na VPC.  
- Uma **sub-rede pública** com pelo menos **50 endereços IP**.  

O objetivo é montar a VPC e criar um **guia passo a passo** para ele iniciar os recursos.

---

### Passo a Passo para criar a VPC no Console da AWS

1. Abrir o console da AWS e buscar por **VPC**  
![Buscar VPC](https://github.com/user-attachments/assets/841f4a1c-066b-4db7-bfd5-c52a4dcb3df0)

2. Clicar em **Criar VPC** e configurar as opções:  
- **VPC settings**: `VPC and more`  
![Configuração VPC](https://github.com/user-attachments/assets/38aa0ebb-5464-4627-8885-0ae2e1806ee7)

- **Name tag auto-generation**: Defina um nome para a VPC  
![Nome da VPC](https://github.com/user-attachments/assets/8a8edb01-fb14-4a33-af14-a77a712305fc)

- **IPv4 CIDR**: `192.168.0.0/18`  
![IPv4 CIDR](https://github.com/user-attachments/assets/3662fe1b-66bb-4ea0-8325-01e9b937de6c)  
> 💡 Use a [calculadora de sub-rede](https://www.subnet-calculator.com/) para confirmar os intervalos.  

- **IPv6 CIDR block**: `No IPv6 CIDR block`  
![IPv6 Configuração](https://github.com/user-attachments/assets/fde143ca-8a7a-4ae0-a5ab-793b437342b8)

- **Tenancy**: `Default`  
![Tenancy](https://github.com/user-attachments/assets/3d2c956f-1c12-4b32-acc0-804d312758e1)

- **Number of Availability Zones (AZs)**: `1`  
![AZs](https://github.com/user-attachments/assets/8bc42d58-4a78-4cdf-abf7-752ceac9d7b0)

- **Subnets**:  
  - **Número de sub-redes públicas:** 1  
  - **Número de sub-redes privadas:** 0  
![Sub-redes](https://github.com/user-attachments/assets/cf71cae1-fb7c-4151-8635-329a108971cb)

- **Customize subnets CIDR blocks**: Expandir e revisar
  - **Public subnet CIDR block in us-west-2a:** `192.168.1.0/26`

![Custom CIDR](https://github.com/user-attachments/assets/404e04e4-a061-4128-8fad-51307ce83b4a)

- **NAT gateways**: `None`  
![NAT](https://github.com/user-attachments/assets/8aabfaf0-36d7-4bd6-a6e1-68a5da47125b)

- Clique em **Criar VPC**.  

**VPC criada:**  
![VPC criada](https://github.com/user-attachments/assets/12542637-6099-4a5c-952a-d9276d56b0b7)

**Resource map:**  
![Mapa de recursos](https://github.com/user-attachments/assets/8400f9d8-d398-4fd7-a239-a63991efcae6)

---

### Perguntas de Reflexão

### 🔹 Por que existem sub-redes privadas e públicas?
As **sub-redes públicas** são para instâncias que precisam ser acessadas pela Internet, utilizando **IP público** e um **Internet Gateway**.  
Já as **sub-redes privadas** mantêm instâncias isoladas, sem acesso direto à Internet.  
Para que instâncias privadas se conectem à Internet, é necessário um **NAT Gateway**, que será detalhado posteriormente.

### 🔹 Por que usar endereços IP privados em uma VPC?
Os **endereços IP privados** não são acessíveis pela Internet, garantindo que os recursos e a comunicação **permaneçam internos** à VPC.  
Isso aumenta a **segurança** e o **controle** da rede.

---

### 2. Enviar a resposta ao cliente  

Olá **Paulo**,

Conforme solicitado, montamos sua **VPC** e sua **sub-rede pública** de acordo com suas necessidades.  
Abaixo estão os detalhes e instruções:  

#### 1. VPC criada
- **Nome da VPC:** First-vpc  
- **Bloco CIDR IPv4:** 192.168.0.0/18  
- **Tenancy:** Padrão  
- **Número de Zonas de Disponibilidade:** 1  

#### 2. Sub-rede pública
- **Nome da Sub-rede:** First-public-subnet  
- **Bloco CIDR:** 192.168.1.0/26  
- **Zona de Disponibilidade:** us-west-2a  
- **Número de endereços IP disponíveis:** 50  

#### 3. Recursos adicionais
- **Gateway de internet:** Criado e associado à VPC  
- **Tabela de rotas:** Configurada para permitir acesso à Internet  

#### 4. Próximos passos
1. Acesse o **Console da AWS**.  
2. Navegue até **VPC → Suas VPCs** e confira a VPC `First-vpc`.  
3. Verifique a sub-rede pública e os endereços IP disponíveis.  
4. Inicie instâncias na sub-rede conforme necessário.  

Se precisar de ajuda para adicionar **sub-redes privadas** ou configurar **NAT**, podemos orientar nas próximas etapas.

Atenciosamente,  
**[Seu Nome]**  
Engenheiro de Suporte de Nuvem

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
