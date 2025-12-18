## AWS Systems Manager 

O **AWS Systems Manager (SSM)** é um serviço da AWS que permite **gerenciar, automatizar e manter recursos da infraestrutura** da AWS, 
principalmente **instâncias EC2**, de forma **centralizada e segura**, sem necessidade de acesso via SSH ou RDP.

## 🔧 O que o AWS Systems Manager faz?
O Systems Manager ajuda a:
- Gerenciar instâncias EC2 e servidores on-premises
- Executar comandos remotamente
- Automatizar tarefas operacionais
- Aplicar patches de segurança
- Manter inventário de software e configurações
- Acessar instâncias com segurança
- Monitorar e responder a incidentes operacionais

## 🧩 Principais recursos do AWS Systems Manager

Os recursos são as capacidades do serviço, ou seja, o que o Systems Manager oferece.  

### 🔹 Session Manager
- Acesso às instâncias **sem SSH ou RDP**
- Não exige portas abertas no Security Group
- Registro de sessões no CloudWatch ou S3

**🧠 Exemplo prático**
- Acessar uma EC2 com segurança, sem SSH ou RDP  
  → **Session Manager**

---

### 🔹 Run Command
- Execução de comandos em **uma ou várias instâncias**
- Ideal para manutenção em massa

**🧠 Exemplo prático**
- Executar um comando em uma ou várias instâncias EC2  
  → **Run Command (`AWS-RunShellScript`)**

---

### 🔹 Automation
- Execução de **runbooks automatizados**

**🧠 Exemplos práticos**
- Criar snapshot dos volumes EBS de uma EC2  
  → **Automation (`AWS-CreateSnapshot`)**

- Criar backup completo de uma EC2 (AMI)  
  → **Automation (`AWS-CreateImage`)**

- Reiniciar instâncias automaticamente  
  → **Automation**

---

### 🔹 Patch Manager
- Aplicação automática de patches de segurança
- Mantém servidores em compliance

**🧠 Exemplo prático**
- Aplicar patches de segurança automaticamente nas instâncias  
  → **Patch Manager**

---

### 🔹 Inventory
- Coleta informações sobre:
  - Sistema operacional
  - Aplicações instaladas
  - Configurações do sistema

**🧠 Exemplo prático**
- Coletar informações de sistema operacional e softwares instalados  
  → **Inventory**

---

### 🔹 Parameter Store
- Armazenamento seguro de **dados de configuração**, como:
  - Variáveis de configuração
  - Strings de conexão
  - URLs
  - Segredos (integrado ao AWS KMS)

**🧠 Exemplo prático**
- Armazenar variáveis de configuração ou segredos de forma segura  
  → **Parameter Store**

---

### 🔹 OpsCenter
- Centralização de alertas e incidentes operacionais
- Criação e acompanhamento de OpsItems

**🧠 Exemplo prático**
- Centralizar e gerenciar incidentes operacionais  
  → **OpsCenter**

---

### 🔹 State Manager
- Mantém instâncias em um **estado desejado**

**🧠 Exemplo prático**
- Garantir que um agente ou serviço esteja sempre em execução  
  → **State Manager**

## 📂 Tipos de documentos do AWS Systems Manager

| Tipo de Documento | Finalidade | Exemplo |
|------------------|-----------|---------|
| **Session** | Acesso interativo à instância (Session Manager) | `AWS-StartInteractiveCommand` |
| **Package** | Instalação e gerenciamento de pacotes | `AWS-ConfigureAWSPackage` |
| **Automation** | Execução de fluxos automatizados (runbooks) | `AWS-CreateSnapshot` |
| **Command** | Execução de comandos nas instâncias | `AWS-RunShellScript` |
| **Policy** | Governança, compliance e controles | `AWS-DisablePublicAccessForEC2Instance` |

> São instruções que dizem como o recurso deve agir.
> Escritos em JSON ou YAML.
> Definem passos, comandos e parâmetros.
> São usados pelos recursos.

> ➡️ Pense neles como o manual de uso da ferramenta.

## 🔐 Benefícios

- 🔒 **Segurança**: sem chaves SSH e sem portas abertas
- ⚙️ **Automação**: redução de tarefas manuais
- 📊 **Visibilidade**: visão centralizada do ambiente
- 📈 **Escalabilidade**: gerencia milhares de instâncias
- 💰 **Custo**: não há cobrança adicional pelo serviço

## 🧠 Resumo rápido

- Snapshot de volumes EBS → **Automation (`AWS-CreateSnapshot`)**
- Backup completo (AMI) → **Automation (`AWS-CreateImage`)**
- Acesso remoto sem SSH → **Session**
- Instalação de software → **Package**
- Execução de comandos → **Command**

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
