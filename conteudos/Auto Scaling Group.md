## 🚀 Auto Scaling Group (ASG) 

Os **Auto Scaling Groups (ASGs)** na AWS são os principais componentes utilizados para **escalar recursos automaticamente** conforme a necessidade da 
aplicação.  

Eles agrupam diversas instâncias do **Amazon EC2** e tratam esse conjunto como uma unidade lógica para fins de **dimensionamento automático** e 
**gerenciamento**.

---

### ✅ Como funciona?

- Um ASG contém um conjunto de instâncias EC2
- Ele ajusta automaticamente o número de instâncias com base em demanda
- As instâncias são distribuídas entre **múltiplas zonas de disponibilidade (AZs)**, aumentando a **tolerância a falhas**
- Garante que sua aplicação permaneça disponível e performática mesmo com variações de tráfego

---

### ⚙️ Parâmetros principais

Ao criar um Auto Scaling Group, você define:

| Configuração | Descrição |
|-------------|-----------|
| **Min** | Número mínimo de instâncias que devem existir |
| **Max** | Número máximo permitido de instâncias |
| **Desired Capacity** | Número desejado que o ASG tenta manter |

---

### 🧠 Configuração de Inicialização

Para iniciar as instâncias, você precisa definir uma:

- **Launch Configuration** *(legado)*  
ou
- **Launch Template** ✅ *(recomendado)*

Nela, você especifica:

- Tipo da instância EC2
- **AMI** (Amazon Machine Image)
- Security groups
- Key pair
- Configurações de rede
- User data (scripts de inicialização)

---

### 🎯 Resumo

| Conceito | Explicação |
|--------|-----------|
ASG | Grupo lógico de instâncias EC2 com escala automática |
Objetivo | Garantir disponibilidade, performance e economia |
Distribuição | Instâncias espalhadas por múltiplas AZs |
Escalonamento | Aumenta ou reduz EC2s conforme a demanda |

---

### 📝 Dica

> **Auto Scaling = Escalar automaticamente + Alta disponibilidade com custo otimizado**

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
