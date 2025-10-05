## 🧪 Lab 278 - Proteção de dados usando criptografia - AWS KMS (Key Management Service) 

A **criptografia** é a conversão de informações em um **código secreto**, garantindo que os dados permaneçam **confidenciais e privados**.  
Suas funções principais incluem:  

- **Autenticação** – verificar a identidade de remetentes e destinatários.
- **Integridade dos dados** – assegurar que os dados não foram alterados.
- **Não repúdio** – impedir que alguém negue uma ação realizada.

A função central da criptografia é **criptografar**, ou seja, transformar dados em um formato **não legível** para quem não possui autorização.  

A criptografia garante a **privacidade**, mantendo informações ocultas de destinatários não autorizados.  

- **Criptografar**: transformar dados em formato ilegível.  
- **Descriptografar**: reverter os dados criptografados para o formato original, tornando-os legíveis novamente.

---

### Tarefa 1: criar uma chave do AWS KMS (Key Management Service)

Nesta tarefa, vamos criar uma **chave do AWS KMS** que será usada posteriormente para **criptografar e descriptografar dados**.  

O **AWS Key Management Service (KMS)** permite criar e gerenciar **chaves criptográficas** e controlar seu uso em vários serviços da AWS e aplicativos.  
O KMS é um serviço seguro e resiliente, utilizando **Hardware Security Modules (HSMs)** validados conforme o padrão FIPS 140-2 para proteger suas chaves.  

1. No console, digite **KMS** na barra de pesquisa e escolha **Key Management Service**.  

2. Escolha Create a key (Criar uma chave).  

<img width="1505" height="330" alt="image" src="https://github.com/user-attachments/assets/a8d77b6c-53da-4d96-9861-d11711efe2c9" />

3. Em **Key type (Tipo de chave)**, selecione **Symmetric (Simétrica)** e clique em **Next (Próximo)**.  
   > **Observação:**  
   > - Criptografia **simétrica**: mesma chave para criptografar e descriptografar, rápida e eficiente.  
   > - Criptografia **assimétrica**: chave pública para criptografar e chave privada para descriptografar.

<img width="1147" height="177" alt="image" src="https://github.com/user-attachments/assets/54a60c69-9f8b-4cbc-ae2b-77a30de8a108" />

4. Na página **Add labels (Adicionar rótulos)**, configure:
   - **Alias:** `MyKMSKey`  
   - **Description (Descrição):** `Key used to encrypt and decrypt data files.`  
   - Clique em **Next (Próximo)**.

<img width="1132" height="371" alt="image" src="https://github.com/user-attachments/assets/89ec98c7-7a1a-42ea-a6f1-1f8e22be4947" />

5. Na página **Define key administrative permissions (Definir permissões administrativas para a chave)**:  
   - Em **Key administrators (Administradores da chave)**, selecione a caixa de **voclabs** e clique em **Next (Próximo)**.  
   - Em **This account (Esta conta)**, selecione novamente **voclabs** e clique em **Next (Próximo)**.

<img width="1150" height="237" alt="image" src="https://github.com/user-attachments/assets/bd7d062a-6774-410d-aaaf-a10d9d003520" />

6. Na página **Define key administrative permissions** (Definir permissões administrativas para a chave), na seção **This account** (esta conta),  
pesquise e selecione a caixa de seleção para 'voclabs' e, depois, Next.  

<img width="1149" height="233" alt="image" src="https://github.com/user-attachments/assets/445055f2-b95d-4895-bf46-73716f09400c" />

7. Não altere a política.  

<img width="1147" height="503" alt="image" src="https://github.com/user-attachments/assets/b31a6375-eee9-41ab-9cfe-cb8f6cb75cb6" />

8. Revise as configurações e clique em **Finish**.  

<img width="1386" height="422" alt="image" src="https://github.com/user-attachments/assets/03218113-1844-4634-ba3f-44bf9f908bf5" />

9. Escolha o link para a **MyKMSKey** que você acabou de criar e copie o valor do ARN (Amazon Resource Name) para um editor de texto.  

<img width="1135" height="261" alt="image" src="https://github.com/user-attachments/assets/50a00571-9909-4b97-b50d-159de013819a" />

---

### Tarefa 2: configurar a instância do servidor de arquivos  

Nesta tarefa, vamos configurar o arquivo de credenciais da AWS que habilita o uso da chave do AWS KMS que você criou em uma etapa anterior.  
Vamos instalar a AWS Encryption CLI para conseguir executar comandos de criptografia.  

Mas, antes de criptografar ou descriptografar dados, é necessário configurar alguns pontos na **instância EC2 do File Server**:
- Configurar as credenciais da AWS para usar a **chave AWS KMS**.
- Instalar a **AWS Encryption CLI** (`aws-encryption-cli`) para executar comandos de criptografia e descriptografia.

1. No console, insira EC2 na barra de pesquisa e escolha EC2.  

2. Na lista **Instances**, marque a caixa de seleção da instância **File Server** e escolha Connect.  

<img width="1192" height="171" alt="image" src="https://github.com/user-attachments/assets/0d0e24e2-b507-4a6d-a67b-9b72b91937e5" />

3. Na guia **Session Manager** (Gerenciador de sessão) clique em Connect.  

<img width="1400" height="380" alt="image" src="https://github.com/user-attachments/assets/a899a2fd-ec06-4968-a442-b7e9a443a45c" />

4. Para alterar o diretório inicial e criar um arquivo com as credenciais da AWS, execute os seguintes comandos:  

```
cd ~
aws configure
```

<img width="601" height="119" alt="image" src="https://github.com/user-attachments/assets/98207b64-78f8-4421-b590-36c97cc232f9" />

5. Quando solicitado, configure o seguinte:  

  - AWS Access Key ID (ID da chave de acesso à AWS): insira 1 e pressione Enter.
  - AWS Secret Access Key (Chave secreta de acesso à AWS): insira 1 e pressione Enter.
  - Default region name (Nome da região padrão): copie e cole a região fornecida na página AWS Details (Detalhes da AWS) no Vocareum.
  - Dica Pode ser necessário pressionar Ctrl+Shift+V para colar no gerenciador de sessão.
  - Default output format (Formado de saída padrão): pressione Enter.

> O arquivo de configuração da AWS será criado e você o atualizará em uma etapa mais adiante.  
> As entradas anteriores de 1 são espaços reservados temporários.  

<img width="316" height="118" alt="image" src="https://github.com/user-attachments/assets/85ada3a9-9318-446e-9176-535863d07b00" />

6. Navegue até à página do console Vocareum e selecione o botão  AWS Details (Detalhes da AWS).  
Ao lado de AWS CLI, selecione Show (Exibir).  
Copie e cole o bloco de código, que começa com [default], em um editor de texto.  
Retorne à guia do navegador em que você está logado no servidor de arquivos.  
Para abrir o arquivo de credenciais da AWS, execute o seguinte comando: ` vi ~/.aws/credentials `  

7. No arquivo `~/.aws/credentials`, digite **dd** várias vezes para excluir o conteúdo do arquivo.  
Cole o bloco de código que você copiou do Vocareum.  

<img width="727" height="135" alt="image" src="https://github.com/user-attachments/assets/95a63897-ced2-42d5-9d09-65ff3ebd6366" />

8. Para salvar e fechar o arquivo, pressione `Esc`, digite `:wq` e pressione `Enter`.  
Para visualizar o conteúdo atualizado do arquivo, execute o seguinte comando: ` cat ~/.aws/credentials `  

Agora você instalará a AWS Encryption CLI e exportar o caminho.  
Fazendo isso, você poderá executar os comandos para criptografar e descriptografar dados.  

9. Para instalar a **AWS Encryption CLI** e definir o caminho, execute os seguintes comandos:  
```
pip3 install aws-encryption-sdk-cli
export PATH=$PATH:/home/ssm-user/.local/bin
```

> Com isso, a instância está pronta para criptografar e descriptografar dados usando a chave AWS KMS.  

---

### Tarefa 3: Criptografar e Descriptografar Dados  

Nesta tarefa, vamos criar um arquivo de texto com uma simulação de dados sigilosos.  
Depois, vamos usar a criptografia para proteger o conteúdo do arquivo.  
A próxima etapa é descriptografar e visualizar o conteúdo do arquivo.  

Vamos aprender a criptografar dados de texto simples em texto cifrado executando o comando `--encrypt`.  
Depois, descriptografar o texto cifrado de volta para o original, obtendo dados de texto simples legíveis.  

1. Para criar o arquivo de texto, execute os seguintes comandos:  
```
touch secret1.txt secret2.txt secret3.txt
echo 'TOP SECRET 1!!!' > secret1.txt
```

2. Para visualizar o conteúdo do arquivo secret1.txt, execute o seguinte comando: ` cat secret1.txt `  

3. Para criar um diretório para a saída do arquivo criptografado, execute o seguinte comando: ` mkdir output `  

4. Copie e cole o seguinte comando em um editor de texto: ` keyArn=(KMS ARN) `  

5. No editor de texto, substitua (KMS ARN) pelo ARN da AWS KMS que você copiou na tarefa 1.  

6. Execute o comando atualizado no terminal do servidor de arquivos.  
Este comando salva o ARN de uma chave AWS KMS na variável $keyArn.
Ao criptografar usando uma chave do AWS KMS, você pode identificá-la usando um ID de chave, ARN de chave, nome de alias ou ARN de alias.  

8. Para criptografar o arquivo secret1.txt, execute o seguinte comando:
```
aws-encryption-cli --encrypt \
                     --input secret1.txt \
                     --wrapping-keys key=$keyArn \
                     --metadata-output ~/metadata \
                     --encryption-context purpose=test \
                     --commitment-policy require-encrypt-require-decrypt \
                     --output ~/output/.
```

8. As informações a seguir descrevem o que esse comando fez:  

Explicação dos parâmetros:  

- A primeira linha criptografa o conteúdo do arquivo.  
`--encrypt` → indica que é operação de criptografia.  
`--input` → arquivo a ser criptografado.  
`--wrapping-keys key=$keyArn` → usa a chave KMS especificada pelo ARN.  
`--metadata-output` → arquivo de texto para armazenar metadados da operação.  
`--encryption-context` → define contexto de criptografia (boa prática).  
`--commitment-policy` → garante que recursos de segurança com confirmação de chave sejam usados.  
`--output` → diretório de saída do arquivo criptografado.  

> Quando o comando de criptografia tiver êxito, ele não retornará nenhuma saída.  

9. Para determinar se o comando teve êxito, execute o seguinte comando: ` echo $? `  

> Se o comando teve êxito, o valor de $? é 0 = sucesso.
> Se houve falha no comando, o valor será diferente de zero.  

<img width="122" height="55" alt="image" src="https://github.com/user-attachments/assets/92652d11-dd0e-442a-9544-91344c58b1f5" />

10. Para visualizar o arquivo recém-criptografado, execute o seguinte comando: ` ls output `  
A saída deverá ser como a seguinte: `secret1.txt.encrypted`  

11. Para visualizar o conteúdo do arquivo recém-criptografado, execute o seguinte comando:  
```
cd output
cat secret1.txt.encrypted
```
> O arquivo agora contém texto cifrado, ilegível até a descriptografia.

> O processo de criptografia e descriptografia leva os dados em forma de texto simples, legível e compreensível, e manipula sua forma para criar o texto cifrado, que é o que você está vendo agora.  

> Quando os dados forem transformados em texto cifrado, o texto simples fica inacessível até que tenha sido descriptografado.  

> O diagrama a seguir demonstra como a criptografia funciona com chaves e algoritmos simétricos. Uma chave simétrica e um algoritmo são usados para converter uma mensagem de texto simples em texto cifrado.  

<img width="537" height="267" alt="image" src="https://github.com/user-attachments/assets/ac02194f-bf5e-47c0-9cd1-d7d57156aaa2" />

12. Pressione Enter.  
A seguir, você vai descriptografar o arquivo secret1.txt.encrypted.  

13. Para descriptografar o arquivo, execute os seguintes comandos:  
```
aws-encryption-cli --decrypt \
                     --input secret1.txt.encrypted \
                     --wrapping-keys key=$keyArn \
                     --commitment-policy require-encrypt-require-decrypt \
                     --encryption-context purpose=test \
                     --metadata-output ~/metadata \
                     --max-encrypted-data-keys 1 \
                     --buffer \
                     --output.
```

13. Para visualizar a localização do novo arquivo, execute o seguinte comando: ` ls `  
O arquivo `secret1.txt.encrypted.decrypted` contém o conteúdo descriptografado do arquivo `secret1.txt.encrypted`.  

15. Para visualizar o conteúdo do arquivo criptografado, execute o seguinte comando: ` cat secret1.txt.encrypted.decrypted `  
Após descriptografar com êxito, você verá o conteúdo do texto simples original do secret1.txt.

> O seguinte diagrama demonstra como a mesma chave secreta e algoritmo simétrico do processo de criptografia são usados para descriptografar o texto cifrado de volta para um texto simples.

<img width="375" height="203" alt="image" src="https://github.com/user-attachments/assets/ab13577f-6535-45d6-b4c5-3019c9262bd9" />


### Observações

Criptografia simétrica: mesma chave e algoritmo usados para criptografar e descriptografar.
Texto simples → texto cifrado → texto simples novamente usando a mesma chave.
A prática assegura confidencialidade e integridade dos dados.

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
