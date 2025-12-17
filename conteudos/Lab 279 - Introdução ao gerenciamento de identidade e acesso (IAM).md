## 🧪 Lab - Introdução ao AWS Identity and Access Management (IAM)

Em muitos ambientes de negócios, o acesso envolve um único login em um computador ou rede de sistemas que fornece ao usuário acesso a todos os recursos da rede — 
como pastas pessoais e compartilhadas, intranets corporativas, impressoras e outros dispositivos.  
Sem controles adequados de autenticação e autorização, usuários não autorizados podem explorar rapidamente esses recursos.  

Neste laboratório, vamos explorar **usuários**, **grupos de usuários** e **políticas** no serviço **AWS Identity and Access Management (IAM)**.  

## Objetivos

Após concluir este laboratório, seremos capazes de:

- Criar e aplicar uma política de senhas do IAM  
- Explorar os usuários e grupos de usuários pré-criados do IAM  
- Inspecionar as políticas do IAM aplicadas aos grupos de usuários pré-criados  
- Adicionar usuários a grupos com capacidades específicas ativas  
- Localizar e usar o URL de login do IAM  
- Testar os efeitos das políticas sobre o acesso a serviços da AWS  

> **Visão geral do laboratório**  
> Abaixo está um diagrama do ambiente atual com os usuários e grupos do IAM listados.

<img width="2200" height="1100" alt="image" src="https://github.com/user-attachments/assets/8e8e7e56-6fb8-43a6-8733-d13f093aaa5c" />

## IAM – O que é e para que serve?

O **AWS Identity and Access Management (IAM)** permite:

- **Gerenciar usuários do IAM e o acesso**: crie usuários, atribua credenciais (senhas, chaves de acesso, MFA) e controle suas permissões.
- **Gerenciar funções e permissões do IAM**: as *roles* (funções) são identidades que podem ser assumidas temporariamente por usuários, serviços ou aplicações — ideal para delegação segura de permissões.
- **Gerenciar usuários federados**: permita que identidades corporativas existentes (ex: via Active Directory) acessem a AWS sem criar contas IAM separadas.

## Tarefa 1: Criar uma política de senhas para a conta

Vamos criar uma política de senhas rígida para todos os usuários da conta.  

1. No console AWS, na barra de pesquisa, digite **IAM** e selecione-o.
2. No painel esquerdo, vá em **Account settings**.
3. Observe a política de senha padrão. Sua empresa exige requisitos mais fortes.
4. Clique em **Change password policy** e configure:
   - **Minimum password length**: `10` (em vez de 8)  
   - Marque **todas as caixas**, exceto:  
     - ☐ *Password expiration requires administrator reset*  
   - **Password expiration**: `90 days` (padrão)  
   - **Prevent password reuse**: `5` senhas anteriores (padrão)

<img width="1408" height="670" alt="image" src="https://github.com/user-attachments/assets/3fe164ec-525e-4d82-9333-4db33bdd339b" />

5. Clique em **Save changes**.

> ✅ **Resumo da Tarefa 1**  
> Fortalecemos a segurança da conta com uma política de senhas personalizada, tornando-as mais resistentes a ataques.

## Tarefa 2: Explorar usuários e grupos de usuários

### Explorando usuários

1. No menu esquerdo, clique em **Users**.
   - Usuários disponíveis: `user-1`, `user-2`, `user-3`
2. Clique em **user-1**:
   - Guia **Permissions**: está sem permissões  
   - Guia **Groups**: não pertence a nenhum grupo  
   - Guia **Security credentials**: possui senha de console

> 💡 **Dica**: Grupos permitem aplicar permissões a múltiplos usuários de forma eficiente — melhor que gerenciar permissões individualmente.

### Explorando grupos

1. No menu esquerdo, clique em **User groups**.
   - Grupos pré-criados: `EC2-Admin`, `EC2-Support`, `S3-Support`

#### Grupo EC2-Support
- Na guia Permissions -> Política anexada: **AmazonEC2ReadOnlyAccess** (gerenciada pela AWS)
- Permite: listar e descrever recursos do EC2, ELB, CloudWatch e Auto Scaling — **sem modificar**
- Estrutura da política:
  - **Effect**: `Allow`
  - **Action**: ex: `ec2:DescribeInstances`
  - **Resource**: `*` (todos os recursos)

<img width="1059" height="467" alt="image" src="https://github.com/user-attachments/assets/ef64f3ec-7d71-4f03-aaa6-d1a0ac13087e" />
<img width="1058" height="205" alt="image" src="https://github.com/user-attachments/assets/46c724cf-1f26-4e33-9aee-bebc70a73bb4" />

#### Grupo S3-Support
- Na guia Permissions -> Política anexada: **AmazonS3ReadOnlyAccess**
- Permite: `s3:Get*`, `s3:List*` — somente leitura no S3

<img width="1064" height="405" alt="image" src="https://github.com/user-attachments/assets/a3c3c6e4-cbab-48df-836c-a03b12b8ed4a" />

#### Grupo EC2-Admin
- Na guia Permissions -> Política **inline** (personalizada): **EC2-Admin-Policy**
- Permite: `ec2:Describe*`, `ec2:StartInstances`, `ec2:StopInstances`

<img width="1065" height="405" alt="image" src="https://github.com/user-attachments/assets/4fc50997-8c91-419b-8ecf-23e4ad400843" />

> ✅ **Resumo da Tarefa 2**  
> Exploramos usuários e grupos, identificamos políticas **gerenciadas** vs. **inline**, e compreendemos como as permissões são atribuídas.

## Cenário de negócios

Sua empresa usa intensivamente **EC2** e **S3**. Novos colaboradores precisam de acesso baseado em função:

| Usuário   | Grupo        | Permissões                          |
|-----------|--------------|-------------------------------------|
| `user-1`  | S3-Support   | Leitura no Amazon S3                |
| `user-2`  | EC2-Support  | Leitura no Amazon EC2               |
| `user-3`  | EC2-Admin    | Visualizar, iniciar e parar instâncias EC2 |

## Tarefa 3: Adicionar usuários a grupos

### Adicionar `user-1` ao grupo `S3-Support`
1. Em **User groups**, selecione **S3-Support**.
2. Guia **Users** → **Add users**.
3. Marque **user-1** → **Add users**.

### Adicionar `user-2` ao grupo `EC2-Support`
Repita o processo acima para `user-2` no grupo `EC2-Support`.

### Adicionar `user-3` ao grupo `EC2-Admin`
Repita para `user-3` no grupo `EC2-Admin`.

> ✅ Verifique: cada grupo deve ter **1 usuário** na coluna *Users*.  
> ✅ **Resumo da Tarefa 3**: Todos os usuários foram atribuídos corretamente aos seus grupos.

## Tarefa 4: Fazer login e testar permissões

1. No IAM, acesse o **Dashboard**.
2. Copie o *IAM sign-in URL* no **AWS Account** no canto direito da tela (ex: `https://123456789012.signin.aws.amazon.com/console`).
3. Abra uma **janela privada/anônima**:
   - **Chrome**: `Ctrl+Shift+N` → Nova janela anônima  
   - **Firefox**: `Ctrl+Shift+P` → Nova janela privada  
   - **Edge**: `Ctrl+Shift+P` → Nova janela InPrivate

### Teste com `user-1` (S3-Support)
- Credenciais:
  - Usuário: `user-1`
  - Senha: `Lab-Password1`
- Acesse **S3**: consegue listar buckets e objetos ✅  
- Acesse **EC2**: mensagem *"You are not authorized"* ❌

### Teste com `user-2` (EC2-Support)
- Credenciais:
  - Usuário: `user-2`
  - Senha: `Lab-Password2`
- Acesse **EC2**: visualiza instâncias ✅  
- Tente **parar instância**: erro de permissão ❌  
- Acesse **S3**: *"You don't have permissions to list buckets"* ❌

### Teste com `user-3` (EC2-Admin)
- Credenciais:
  - Usuário: `user-3`
  - Senha: `Lab-Password3`
- Acesse **EC2**: visualiza instâncias ✅  
- **Pare a instância**: sucesso! ✅  
  > Certifique-se de estar na **mesma região** usada no início do lab (ex: Oregon)

> ✅ **Resumo da Tarefa 4**  
> - `user-1`: acesso apenas ao S3  
> - `user-2`: leitura no EC2, sem modificação  
> - `user-3`: controle total sobre instâncias EC2  
> As políticas funcionam conforme o princípio do **menor privilégio**.

---

> ✨ **Dica final**:  
> Mantenha políticas **específicas**, evite `*` em `Resource` quando possível, e prefira **grupos** a permissões individuais para escalabilidade e segurança.

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
