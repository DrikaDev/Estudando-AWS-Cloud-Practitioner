## 🪪 Segurança na Nuvem: ROOT (Raiz) & IAM (Identity and Access Management)

## Diferenças entre usuário Root & usuário IAM:

### Usuário ROOT:
- É o **usuário raiz** da conta AWS.  
- Possui **acesso total** a todos os recursos e **não** pode ter seus privilégios limitados.  

> **Prática recomendada**: não use o usuário ROOT no dia a dia. Utilize-o apenas quando **estritamente necessário**, como para:
> - Alterar o plano do AWS Support.  
> - Restaurar permissões de um usuário IAM.  
> - Alterar configurações da conta (informações de contato, regiões permitidas etc.).

### Usuário IAM:
- Integra-se a outros serviços da AWS.  
- Permite federação de identidades, acesso seguro para aplicativos e permissões granulares.  

## 🔐 Mas, o que é o IAM?

O **IAM** é um serviço **gratuito** da AWS para **gerenciar usuários e controlar acesso** a recursos.  

Você pode:
- Criar **usuários**, **grupos** e **funções**.  
- Aplicar **políticas** para definir permissões (ex.: quem pode encerrar instâncias EC2, criar buckets no S3 ou apenas visualizar).  

### Componentes Essenciais
- **Usuário IAM**: pessoa que pode se autenticar na AWS.  
- **Grupo IAM**: conjunto de usuários com as mesmas permissões.  
- **Política IAM**: documento que define quais recursos podem ser acessados e em qual nível.  
- **Função IAM**: conjunto de permissões temporárias usadas por serviços como EC2, Lambda etc.  

### Formas de Acesso
- **Console de Gerenciamento AWS** (Web).  
- **AWS CLI** (linha de comando no Linux ou Windows).  
- **AWS SDKs** (Java, Python, JavaScript e outras linguagens).  

---

## 🛡️ 4 Recomendações Básicas de Segurança - Como aplicar boas práticas de segurança

### 1. Pare de usar o usuário ROOT o quanto antes
a) Ao conectar-se como ROOT, crie um **usuário IAM** para você.  
b) Crie um **grupo IAM** com permissões de administrador e adicione seu usuário.  
c) Desabilite e remova **chaves de acesso** do ROOT (se existirem).  
d) Habilite uma **política de senha** para usuários.  
e) Passe a fazer login com as novas credenciais IAM.  
f) Armazene as credenciais ROOT em local seguro.  

### 2. Habilite MFA (Multi-Factor Authentication)
a) Ative MFA para o usuário ROOT e para todos os usuários IAM.  
b) Utilize MFA também para controlar acesso às **APIs** da AWS.  

### 3. Ative o AWS CloudTrail
- Serviço que **rastreia atividades** dos usuários na conta.  
- Registra todas as solicitações de API para serviços compatíveis.  
- Histórico básico (90 dias) é **gratuito e habilitado por padrão**.  

### 4. Habilite Relatórios de Faturamento
- Fornece informações sobre uso e custos estimados.  
- O relatório de **Custos e Uso da AWS** permite monitorar cobranças **por hora** ou **por dia**.  

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
