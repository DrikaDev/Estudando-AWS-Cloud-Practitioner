## 🧪 Lab: Hospedar um Site Estático no S3

Vamos criar um bucket no Amazon S3 para hospedar um site estático e colocar em prática os conceitos de **permissões e políticas públicas**.  

### 📌 Passo 1 — Criar o Bucket

1. Acesse o **Amazon S3** e clique em **Create bucket**.  
   
   <img width="886" height="200" alt="image" src="https://github.com/user-attachments/assets/97850ab4-2f1f-4884-967b-a3b64b7572dd" />
   
2. Escolha um **nome único** para o bucket.  
   
   <img width="886" height="465" alt="image" src="https://github.com/user-attachments/assets/76cd967e-74d3-49cf-abae-f22f87b23284" />
   
3. **Desmarque** a opção `Bloquear todo o acesso público`.  
   - Marque o checkbox:  
     *"Reconheço que as configurações atuais podem fazer com que esse bucket e os objetos dentro dele se tornem público"*.  
   > Isso permitirá que o site seja acessado por qualquer pessoa via link.  
   
   <img width="886" height="339" alt="image" src="https://github.com/user-attachments/assets/edfeb558-5968-414b-9cfd-6cd8713035d5" />
   
4. Deixe os demais itens como **default** e clique em **Criar Bucket**.  

---

### 📌 Passo 2 — Fazer o Upload dos Arquivos

1. Entre no bucket criado.  
   
   <img width="886" height="351" alt="image" src="https://github.com/user-attachments/assets/039a31c6-b724-40c7-851c-55bd4c31a288" />
   
2. Faça o **upload** dos arquivos do seu site e depois clique em **Carregar**.
   
   <img width="886" height="300" alt="image" src="https://github.com/user-attachments/assets/b339395f-8a82-47d2-91d3-25b62b849935" />
   
3. Após concluído, os arquivos devem aparecer listados no bucket.
   
   <img width="886" height="286" alt="image" src="https://github.com/user-attachments/assets/004f65f2-6a05-4be9-968e-4006962fd788" />

---

### 📌 Passo 3 — Habilitar a Hospedagem de Site Estático

1. Vá até a aba **Properties** do seu bucket.
   
   <img width="886" height="145" alt="image" src="https://github.com/user-attachments/assets/8ed4057b-2e28-45fb-bb76-b88e9361ccca" />
   
2. Em **Static website hosting**, clique em **Editar**.
   
   <img width="886" height="156" alt="image" src="https://github.com/user-attachments/assets/afe38695-0099-4ce0-950a-14c9e73bb333" />
   
3. **Habilite** a hospedagem de site estático.
   
   <img width="576" height="203" alt="image" src="https://github.com/user-attachments/assets/f193c6ea-d659-45eb-8db1-f67bdfd04296" />
   
4. Configure:  
   - **Documento de índice** → `index.html`  
   - **Documento de erro** → `error.html`  
   
5. Salve as alterações.  

---

### 📌 Passo 4 — Corrigir o Erro 403 (Permissões)

Ao tentar acessar o site, temos o **Erro 403 (Access Denied)**.  

<img width="886" height="85" alt="image" src="https://github.com/user-attachments/assets/de206a83-50cc-483c-9537-3b413d01c3d9" />

<img width="886" height="381" alt="image" src="https://github.com/user-attachments/assets/6a44d3f1-fdfa-4313-9d91-4ddba216fba5" />  

👉🏻 Isso acontece porque o bucket ainda **não tem uma política pública** que permita acesso.  

Para corrigir:
1. Vá até a aba **Permissions**.
   
   <img width="886" height="268" alt="image" src="https://github.com/user-attachments/assets/b1e1870e-2db9-4d2d-876b-7fa31a85cf95" />
   
2. Em **Bucket Policy**, insira o seguinte código JSON:  

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::nome-do-seu-bucket/*"
    }
  ]
}
```

> ⚠️ **Importante:**  
> Substitua `nome-do-seu-bucket` pelo nome real do seu bucket.

3. Clique em **Salvar**.
   
4. Dê um **F5** no link do site.
   
### 🎉 Pronto! Seu **site estático** está no ar usando o **Amazon S3** 🚀☁️  

<img width="886" height="515" alt="image" src="https://github.com/user-attachments/assets/7102d3b2-a049-4087-b3e1-98c962d85a62" />

<img width="886" height="522" alt="image" src="https://github.com/user-attachments/assets/fdcf265c-06ec-48a5-8f34-a887ce54d383" />

<img width="886" height="509" alt="image" src="https://github.com/user-attachments/assets/b899fc34-c43f-4ff3-b617-ab99039ee743" />

---

👉🏻 Essa prática não só reforça o aprendizado de **permissões e políticas públicas**,  
mas também mostra como usar a AWS para **publicar um site real** de forma simples e acessível.

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
