## 🧪 Laboratório: Criar uma Amazon Virtual Private Cloud (Amazon VPC) e entender como criar sub-redes e alocar endereços IP;

Neste laboratório, investigamos as necessidades do cliente e montamos um ambiente **VPC** com base nos requisitos.  
Em seguida, criamos um passo a passo simples para o cliente seguir.

## 📌 Cenário

O cliente está iniciando o uso da **AWS** e pediu ajuda para criar sua primeira **VPC**.  
Ele possui algum conhecimento de redes, mas é iniciante na AWS.

**Requisitos do cliente:**
- Cerca de **15.000 endereços IP** no intervalo privado dentro da VPC.  
- Uma **sub-rede pública**.  
- Pelo menos **50 endereços IP** para a sub-rede pública.  

---

## 🚀 Passo a Passo para Criar a VPC

### 1. Abrir o console da AWS e buscar por **VPC**
![Buscar VPC](https://github.com/user-attachments/assets/841f4a1c-066b-4db7-bfd5-c52a4dcb3df0)

### 2. Clicar em **Criar VPC** e configurar as opções

**VPC settings**: `VPC and more`  
![Configuração VPC](https://github.com/user-attachments/assets/38aa0ebb-5464-4627-8885-0ae2e1806ee7)

**Name tag auto-generation**: Defina um nome para a VPC  
![Nome da VPC](https://github.com/user-attachments/assets/8a8edb01-fb14-4a33-af14-a77a712305fc)

**IPv4 CIDR**: `192.168.0.0/18`  
![IPv4 CIDR](https://github.com/user-attachments/assets/3662fe1b-66bb-4ea0-8325-01e9b937de6c)  
> 💡 Use a [calculadora de sub-rede](https://www.subnet-calculator.com/) para confirmar os intervalos.  

**IPv6 CIDR block**: `No IPv6 CIDR block`  
![IPv6 Configuração](https://github.com/user-attachments/assets/fde143ca-8a7a-4ae0-a5ab-793b437342b8)

**Tenancy**: `Default`  
![Tenancy](https://github.com/user-attachments/assets/3d2c956f-1c12-4b32-acc0-804d312758e1)

**Number of Availability Zones (AZs)**: `1`  
![AZs](https://github.com/user-attachments/assets/8bc42d58-4a78-4cdf-abf7-752ceac9d7b0)

**Subnets**:
- Public: `1`  
- Private: `0`  
![Sub-redes](https://github.com/user-attachments/assets/cf71cae1-fb7c-4151-8635-329a108971cb)

**Customize subnets CIDR blocks**: Expandir e revisar  
![Custom CIDR](https://github.com/user-attachments/assets/404e04e4-a061-4128-8fad-51307ce83b4a)

**NAT gateways**: `None`  
![NAT](https://github.com/user-attachments/assets/8aabfaf0-36d7-4bd6-a6e1-68a5da47125b)

### 3. Criar a VPC
Clique em **Criar VPC**.  

**VPC criada:**  
![VPC criada](https://github.com/user-attachments/assets/12542637-6099-4a5c-952a-d9276d56b0b7)

**Resource map:**  
![Mapa de recursos](https://github.com/user-attachments/assets/8400f9d8-d398-4fd7-a239-a63991efcae6)

---

## ❓ Perguntas de Reflexão

### 🔹 Por que existem sub-redes privadas e públicas?
As **sub-redes públicas** são para instâncias que precisam ser acessadas pela Internet, utilizando **IP público** e um **Internet Gateway**.  
Já as **sub-redes privadas** mantêm instâncias isoladas, sem acesso direto à Internet.  
Para que instâncias privadas se conectem à Internet, é necessário um **NAT Gateway**, que será detalhado posteriormente.

### 🔹 Por que usar endereços IP privados em uma VPC?
Os **endereços IP privados** não são acessíveis pela Internet, garantindo que os recursos e a comunicação **permaneçam internos** à VPC.  
Isso aumenta a **segurança** e o **controle** da rede.

---
