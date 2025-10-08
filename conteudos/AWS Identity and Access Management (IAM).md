## 🔐 AWS Identity and Access Management (IAM)

O **AWS Identity and Access Management (IAM)** permite que você gerencie o acesso aos serviços e recursos da AWS com segurança.  
O **IAM** oferece a flexibilidade de configurar o acesso com base nas necessidades operacionais e de segurança específicas da sua empresa.  
Você pode fazer isso usando uma combinação dos recursos do IAM:  
- Políticas do IAM  
- Usuários, grupos e perfis do IAM  
- Autenticação multifator (MFA)

### Índice

- [Políticas do IAM](#políticas-do-iam)  
- [Root - Usuário-raiz da conta AWS](#root---usuário-raiz-da-conta-aws)  
- [Usuários do IAM](#usuários-do-iam)  
- [Grupos do IAM](#grupos-do-iam)  
- [Perfis (Roles) do IAM](#perfis-roles-do-iam)  
- [Autenticação multifator (MFA)](#autenticação-multifator-mfa)  

---

## Políticas do IAM

Uma **política do IAM** é um documento JSON que **concede ou nega permissões** para serviços e recursos AWS.  

Elas permitem **personalizar níveis de acesso**.  
Exemplo: permitir que usuários acessem **apenas um bucket S3 específico**, e não todos.

⚠️ **Prática recomendada:**  
- Siga o **princípio do menor privilégio**.  
- Conceda somente as permissões necessárias para cada função.  

**Exemplo de Política**  
Imagine a cafeteria criando um usuário para um operador de caixa.  
Esse operador precisa acessar apenas os recibos em um bucket do Amazon S3 chamado `AWSDOC-EXAMPLE-BUCKET`.

A política permitiria a ação:  
- `s3:ListObject`  
- Somente no bucket `AWSDOC-EXAMPLE-BUCKET`.  

➡️ Assim, o operador pode **listar objetos desse bucket**, mas não tem acesso a mais nada.

---

## Root - Usuário-raiz da conta AWS

Ao criar uma conta AWS pela primeira vez, você começa com uma identidade conhecida como **Root (usuário-raiz)**.  

Esse usuário é acessado ao entrar com o endereço de e-mail e a senha usados para criar a conta AWS.  
➡️ Pense nele como o **proprietário da cafeteria**: ele tem acesso completo a todos os serviços e recursos AWS na conta.

- O usuário-raiz cria o primeiro **usuário do IAM**.  
- Esse usuário do IAM pode criar outros usuários.  

⚠️ **Prática recomendada:**  
- **Não use o usuário-raiz para tarefas cotidianas.**  
- Use-o apenas para tarefas críticas (ex.: alterar o e-mail da conta, modificar plano do AWS Support).  
- Ative **MFA** no usuário-raiz.  

---

## Usuários do IAM

Um **usuário do IAM** é uma identidade que você cria na AWS para representar uma **pessoa ou aplicação**.  
Consiste em um **nome** e **credenciais** (login/senha ou chaves de acesso).  

- Por padrão, novos usuários **não têm permissões**.  
- Para que possam executar ações (ex.: criar um bucket S3 ou iniciar uma instância EC2), você deve **atribuir permissões**.  

⚠️ **Práticad recomendadas:**  
- Crie **usuários individuais** do IAM para cada pessoa.  
- Mesmo que vários precisem do mesmo nível de acesso → cada um deve ter credenciais próprias.  
- **Aplique a regra do menor privilégio:**  
  - Conceda **apenas as permissões necessárias** para que o usuário execute suas tarefas.  
  - Evite dar permissões amplas como `AdministratorAccess` se não forem necessárias.

➡️ Isso aumenta a **segurança** e permite **rastrear atividades individualmente**, além de reduzir riscos caso uma credencial seja comprometida.

---

## Grupos do IAM

Um **grupo do IAM** é um conjunto de usuários.  
Quando você atribui uma política ao grupo, **todos os usuários herdam essas permissões**.

### Exemplo na cafeteria:
- Em vez de configurar permissões para cada operador de caixa individualmente → o proprietário cria o grupo **“Operadores de Caixa”**.  
- Todos os novos operadores são adicionados ao grupo e herdam as permissões automaticamente.  

➡️ Isso simplifica o gerenciamento e permite mover funcionários entre grupos conforme mudam de função.

---

## Perfis (Roles) do IAM

Um **perfil (role) do IAM** é uma identidade que você pode **assumir temporariamente** para obter permissões.  

**Exemplo na cafeteria:**  
Um funcionário pode alternar entre diferentes funções:  
- Operar a caixa registradora  
- Atualizar o inventário  
- Processar pedidos online  

Quando assume uma **role**, ele:  
- **Abandona as permissões antigas**  
- **Recebe as permissões da nova role**  

⚠️ **Prática recomendada:**  
- Use roles quando o acesso precisar ser **temporário**.  
- Evita criar usuários fixos para cada cenário.  
- Muito útil para **aplicações, serviços da AWS ou identidades externas** (ex.: federação de identidade).  

---

## Autenticação multifator (MFA)

A **autenticação multifator (MFA)** é uma camada extra de segurança.  
Além de senha e usuário, exige um **segundo fator** de autenticação (ex.: código aleatório enviado para celular ou app autenticador).  

- Sem MFA → basta a senha para acesso.  
- Com MFA → mesmo que a senha vaze, o acesso fica protegido.  

⚠️ **Prática recomendada:**  
- Ative **MFA para o usuário-raiz** imediatamente.  
- Ative **MFA para usuários IAM com acesso privilegiado**.  

---

🔐 Seguindo as práticas recomendadas, você garante **segurança, organização e escalabilidade** no gerenciamento de acessos da sua conta AWS.

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
