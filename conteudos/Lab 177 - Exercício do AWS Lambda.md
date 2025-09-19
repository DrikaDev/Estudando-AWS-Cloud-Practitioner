## 🧪 Lab 177 - Exercício AWS Lambda (Desafio)

## 🎯 Objetivos

- Criar uma função do **AWS Lambda** para contar o número de palavras em um arquivo de texto.
- Configurar um **bucket do Amazon S3** para invocar a função Lambda automaticamente quando um arquivo de texto for enviado.
- Criar um **tópico do Amazon SNS** para enviar um e-mail com a contagem de palavras.

---

## 🔄 Fluxo do Laboratório
1. O usuário faz upload de um arquivo de texto no **bucket do S3**.
2. O **S3** aciona a função **Lambda**.
3. A função **Lambda** conta o número de palavras no arquivo.
4. O resultado da contagem é enviado via **SNS** para o e-mail configurado.

---

## 1️⃣ Criar o Bucket no S3
👉🏻 É no bucket que ficam os arquivos enviado e também o **ponto de partida do fluxo**, porque é ele quem dispara a Lambda sempre que recebe um novo arquivo.  
⚠️ Por isso, antes de configurar o **trigger no Lambda**, precisamos criar o bucket que acionará a função.

1. No console da AWS, vá para **S3**.
2. Clique em **Criar bucket**.
3. Defina um nome único e escolha a região.
4. Configure permissões de acordo com a política da sua empresa ou laboratório.
5. Clique em **Criar bucket**.

<img width="935" height="196" alt="image" src="https://github.com/user-attachments/assets/138aaea2-ffb6-4ed7-a6bd-05663b47c630" />

---

## 2️⃣ Criar o Tópico SNS
Antes de configurar a Lambda, crie um **tópico SNS** para enviar notificações:

1. No console da AWS, vá para **SNS**.
2. Clique em **Criar tópico** → **Standard**.
3. Nome do tópico: `ContadorDePalavras`.
<img width="1386" height="502" alt="image" src="https://github.com/user-attachments/assets/274bb904-d6e7-4258-9dc3-0b690bf2722b" />

4. Abaixo, na guia "Assinaturas" clique em "Criar assintaura" para receber a notificação:
   - **Protocolo**: `Email`.
   - **Endpoint**: digite seu **e-mail** (ex.: `meuemail@exemplo.com`).
5. Clique em **Criar assinatura**.
<img width="1393" height="485" alt="image" src="https://github.com/user-attachments/assets/95cee283-f223-4cde-b346-eef17a52aa6a" />

6. Abra seu e-mail e confirme a assinatura clicando no link de confirmação.
7. Copie o **ARN** do tópico para usar no código da Lambda.

---

## 3️⃣ Criar a Função Lambda
Agora crie a função Lambda que fará a contagem de palavras.

**Passos:**
1. Acesse o console do **AWS Lambda**.
2. Clique em **Criar função** → **Autor do zero**.
3. Configure:
   - **Nome**: `ContarNumerosPalavras`.
   - **Runtime**: **Python 3.11** (ou mais recente).
   - **Permissões**: selecione **Usar uma função existente** e escolha `LambdaAccessRole`.
4. Clique em **Criar função**.

<img width="1387" height="406" alt="image" src="https://github.com/user-attachments/assets/de84c316-bc5e-4daa-9525-e3feaa287c9f" />

---

## 4️⃣ Escrever o Código Python
- Na seção **Código da função**, selecione **Editor integrado**.
- Insira o código abaixo, substituindo o ARN do SNS pelo que você criou:

```
import boto3

def lambda_handler(event, context):
    s3 = boto3.client('s3')
    sns = boto3.client('sns')
    
    # Recupera informações do arquivo enviado
    bucket = event['Records'][0]['s3']['bucket']['name']
    key = event['Records'][0]['s3']['object']['key']
    
    # Baixa o arquivo
    obj = s3.get_object(Bucket=bucket, Key=key)
    text = obj['Body'].read().decode('utf-8')
    
    # Conta palavras
    word_count = len(text.split())
    
    # Mensagem a ser enviada
    message = f"A contagem de palavras no arquivo {key} é {word_count}."
    
    # Envia para o tópico SNS
    sns.publish(
        TopicArn='arn:aws:sns:REGIAO:ID_TOPICO:NomeDoTopico',
        Message=message,
        Subject='Word Count Result'
    )
    
    return {
        'statusCode': 200,
        'body': message
    }
```

> **Dica:** Substitua `arn:aws:sns:REGIAO:ID_TOPICO:NomeDoTopico` pelo ARN do tópico SNS que você criou.

---

## 5️⃣ Configurar o Trigger do S3

Agora que o bucket existe, configure a Lambda para ser acionada automaticamente:

1. Na função Lambda, clique em **Adicionar gatilho** (*Add trigger*).
2. Selecione **S3**.
3. Escolha o **bucket** que você criou.
5. Em **Evento do bucket**, selecione **PUT** (upload de arquivos).
6. Clique em Adicionar.

<img width="1393" height="421" alt="image" src="https://github.com/user-attachments/assets/cc578299-bb7f-4401-b6a9-98de0141d907" />

<img width="1387" height="64" alt="image" src="https://github.com/user-attachments/assets/257db97e-29ec-4279-b313-7cc6ac99bde7" />

<img width="596" height="313" alt="image" src="https://github.com/user-attachments/assets/ef28a1f0-a5bf-4b5c-bbb2-b7905807a744" />


---

## 6️⃣ Testar a Função

1. Faça upload de arquivos `.txt` no bucket S3.

<img width="1385" height="171" alt="image" src="https://github.com/user-attachments/assets/cb8b2b69-d1c1-4add-8f12-482aa0feb51e" />

<img width="1390" height="185" alt="image" src="https://github.com/user-attachments/assets/52619efa-346e-492c-98d4-83a2529da43c" />

3. Verifique os logs da função no **CloudWatch Logs**.
4. Confirme se o e-mail ou SMS foi enviado pelo **SNS** com a contagem correta.

---

## 🔄 Fluxo Final

1. Upload de arquivo no **S3**.
2. A função **Lambda** é acionada automaticamente.
3. A função conta as palavras e envia via **SNS**.
4. Notificação recebida por e-mail ou SMS.

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
