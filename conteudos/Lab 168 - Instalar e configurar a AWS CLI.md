## 🧪 Lab 168 - Instalar e configurar a AWS CLI

A **AWS Command Line Interface (AWS CLI)** é uma ferramenta de linha de comando que permite interagir diretamente com os serviços da **Amazon Web Services (AWS)**.  

Você pode instalar a AWS CLI em sua máquina local ou em uma instância da nuvem, como uma instância do **Amazon EC2**.  
Alguns sistemas, como o **Amazon Linux**, já vêm com a AWS CLI pré-instalada.  
No entanto, neste laboratório, vamos instalar e configurar a AWS CLI em uma instância **Red Hat Linux**, que **não** inclui a CLI por padrão.  

Durante esta atividade, vamos:  

1. Estabelecer uma conexão **SSH** com a instância EC2.
2. Instalar e configurar a AWS CLI com credenciais válidas.
3. Usar a CLI para interagir com o **AWS Identity and Access Management (IAM)**.

O ambiente final reflete o seguinte:

<img width="915" height="384" alt="image" src="https://github.com/user-attachments/assets/b3255f65-5289-4149-a178-228eb1db99ea" />

> Uma **VPC** na AWS contém uma instância **EC2 (Red Hat)**.  
> A instância está acessível via **SSH** e tem a **AWS CLI instalada e configurada**.  
> A CLI se comunica com o **IAM** usando credenciais fornecidas.

## Tarefa 1: Conectar-se à instância do EC2 (Red Hat) via SSH

### 🪟 Usuários do Windows
1. Clique em **Detalhes** > **Mostrar**.
2. Clique em **Download PEM** e salve `labsuser.pem`.
3. Copie o **Public IP** para uso posterior.
4. Feche o painel de detalhes.

5. Vá em EC2 - clique na instância - Connect - SSH client
6. Copie o comando ssh
7. Abra um terminal e acesse o diretório onde o arquivo foi salvo (ex: `~/Downloads`)
8. Cole o comando para se conectar à instância: ssh -i labsuser.pem ec2-user@<ip-address>

Quando solicitado, digite yes para aceitar a chave do host.
Nenhuma senha será necessária — a autenticação é feita via chave SSH.

## Tarefa 2: Instalar a AWS CLI no Red Hat Linux

Na janela do terminal (dentro da sessão SSH), execute os seguintes comandos:

- Baixe o instalador da AWS CLI v2:
`curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"`

- Descompacte o arquivo:
`unzip -u awscliv2.zip`

- Execute o instalador com privilégios de root:
`sudo ./aws/install`

- Verifique a instalação:
`aws --version`

Saída esperada (versões podem variar):
`aws-cli/2.7.24 Python/3.8.8 Linux/4.14.133-113.105.amzn2.x86_64 botocore/2.4.5`

- Teste a ajuda da CLI:
`aws help`
Pressione q para sair do visualizador.

## Tarefa 3: Observar detalhes do IAM no Console

No Console da AWS, pesquise por IAM e acesse o serviço.
No menu esquerdo, clique em **Users** e selecione o usuário **awsstudent**.
Na guia **Permissions**, expanda a política `lab_policy` e clique em {} JSON para ver seu conteúdo.
Esta política concede acesso específico a serviços da AWS.
Acesse a guia **Security credentials**.
Localize o Access Key ID.
As credenciais completas (Access Key ID e Secret Access Key) estão disponíveis no menu Detalhes no início das instruções.

## Tarefa 4: Configurar a AWS CLI

Na janela do terminal (sessão SSH), execute: 
`aws configure`

Preencha conforme solicitado:

- AWS Access Key ID: cole o valor de AccessKey do menu Detalhes
- AWS Secret Access Key: cole o valor de SecretKey
- Default region name: us-west-2
- Default output format: json
A configuração é salva em ~/.aws/credentials e ~/.aws/config.

<img width="661" height="128" alt="image" src="https://github.com/user-attachments/assets/b3db84ea-43f5-4081-821c-4fdaa85bc786" />

## Tarefa 5: Testar acesso ao IAM via CLI

Verifique se a configuração está funcionando:
`aws iam list-users`

<img width="658" height="251" alt="image" src="https://github.com/user-attachments/assets/c9870cdf-1be9-4699-8bcb-aa6c4247dd63" />

Se bem-sucedido, você verá uma lista de usuários do IAM em formato JSON.

## 🧩 Desafio da Atividade 1

Objetivo: Baixar a política lab_policy em formato JSON usando apenas a AWS CLI.

### Dicas
Use a Referência de comandos da AWS CLI para IAM.
A política `lab_policy` é uma política gerenciada localmente (não gerenciada pela AWS).
Use `--scope Local` para filtrar políticas personalizadas.
Use `get-policy-version` para obter o conteúdo JSON real.
Redirecione a saída para um arquivo com >.
Solução sugerida
Liste as políticas locais:
`aws iam list-policies --scope Local`

<img width="640" height="326" alt="image" src="https://github.com/user-attachments/assets/5ab6481f-b4e3-481a-b39c-4473a6976940" />

Identifique o Arn e o DefaultVersionId da política lab_policy (ex: v1).
Baixe a política para um arquivo chamado lab_policy.json:

```
aws iam get-policy-version \
  --policy-arn arn:aws:iam::038946776283:policy/lab_policy \
  --version-id v1 > lab_policy.json
```

✅ O arquivo lab_policy.json agora contém a política exata vista no console.

### Resumo da Atividade
Você:
- Instalou a AWS CLI em uma instância Red Hat Linux
- Configurou credenciais para acesso à conta da AWS
- Validou o acesso usando comandos IAM
- Baixou uma política do IAM via linha de comando

Principais lições:
A AWS CLI oferece controle programático sobre os serviços da AWS — equivalente ao Console, mas automatizável.

Para autenticar via CLI, você precisa de:
- Access Key ID
- Secret Access Key

Já no Console, usa-se usuário + senha (e, opcionalmente, MFA).

## Conclusão
🎉 Parabéns! Você concluiu com sucesso:
- Instalar e configurar a AWS CLI
- Conectar a AWS CLI a uma conta da AWS
- Acessar o IAM usando a AWS CLI
Agora você está pronto para automatizar e gerenciar sua infraestrutura AWS diretamente pela linha de comando!

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
