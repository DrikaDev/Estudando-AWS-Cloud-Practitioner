Tarefa 1: criar uma chave do AWS KMS
Nessa tarefa, vamos criar uma chave simétrica do AWS KMS e conceder propriedade dessa chave para o perfil do IAM voclabs que foi pré-criada para este laboratório.  

1. No console, insira KMS na barra  de pesquisa e escolha Key Management Service.
Escolha Create a key (Criar uma chave).

<img width="1505" height="330" alt="image" src="https://github.com/user-attachments/assets/a8d77b6c-53da-4d96-9861-d11711efe2c9" />

2. Em Key type (tipo de chave), escolha Symmetric (simétrica). Depois, clique em Next (Próximo).
Criptografia  simétrica usa a mesma chave para criptografar e descriptografar dados, tornando seu uso rápido e eficiente. Criptografia assimétrica usa uma chave pública para criptografar dados e uma chave privada para descriptografar informações.  
<img width="1147" height="177" alt="image" src="https://github.com/user-attachments/assets/54a60c69-9f8b-4cbc-ae2b-77a30de8a108" />

3. Na página Add labels (Adicionar rótulos), configure o seguinte:  
Alias: MyKMSKey  
Description: (Descrição:) Key used to encrypt and decrypt data files. Depois, clique em Next (Próximo).  
<img width="1132" height="371" alt="image" src="https://github.com/user-attachments/assets/89ec98c7-7a1a-42ea-a6f1-1f8e22be4947" />

4. Na página Define key administrative permissions (Definir permissões administrativas para a chave), 
na seção Key administrators (administradores da chave), pesquise e selecione a caixa de seleção para **voclabs** e, depois, Next (Próximo).  
<img width="1150" height="237" alt="image" src="https://github.com/user-attachments/assets/bd7d062a-6774-410d-aaaf-a10d9d003520" />

5. Na página Define key administrative permissions (Definir permissões administrativas para a chave), na seção This account (esta conta), 
pesquise e selecione a caixa de seleção para voclabs e, depois, Next (Próximo).
<img width="1149" height="233" alt="image" src="https://github.com/user-attachments/assets/445055f2-b95d-4895-bf46-73716f09400c" />

6. Não altere a política.
<img width="1147" height="503" alt="image" src="https://github.com/user-attachments/assets/b31a6375-eee9-41ab-9cfe-cb8f6cb75cb6" />

7. Revise as configurações e selecione Finish (Concluir).
<img width="1386" height="422" alt="image" src="https://github.com/user-attachments/assets/03218113-1844-4634-ba3f-44bf9f908bf5" />

8. Escolha o link para a MyKMSKey que você acabou de criar e copie o valor do ARN (Amazon Resource Name) para um editor de texto.
<img width="1135" height="261" alt="image" src="https://github.com/user-attachments/assets/50a00571-9909-4b97-b50d-159de013819a" />

Tarefa 2: configurar a instância do servidor de arquivos  
Antes de criptografar ou descriptografar dados, você precisa definir alguns pontos.  
Para usar sua chave AWS KMS, você deve configurar as credenciais da AWS na instância do EC2 no File Server (Servidor de arquivos).  
Depois disso, instale a AWS Encryption CLI (aws-encryption-cli), que você pode usar para executar os comandos de criptografar e descriptografar.  

1. No console, insira EC2 na barra de pesquisa e escolha EC2.

2. Na lista Instances (instâncias), marque a caixa de seleção ao lado da instância File Server e escolha Connect (Conectar).
<img width="1192" height="171" alt="image" src="https://github.com/user-attachments/assets/0d0e24e2-b507-4a6d-a67b-9b72b91937e5" />

3. Escolha a guia Session Manager (Gerenciador de sessão) e Connect (Conectar).
<img width="1400" height="380" alt="image" src="https://github.com/user-attachments/assets/a899a2fd-ec06-4968-a442-b7e9a443a45c" />

4. Para alterar o diretório inicial e criar um arquivo com as credenciais da AWS, execute os seguintes comandos:
```
cd ~
aws configure
```
<img width="601" height="119" alt="image" src="https://github.com/user-attachments/assets/98207b64-78f8-4421-b590-36c97cc232f9" />

5. Quando solicitado, configure o seguinte:

AWS Access Key ID (ID da chave de acesso à AWS): insira 1 e pressione Enter.
AWS Secret Access Key (Chave secreta de acesso à AWS): insira 1 e pressione Enter.
Default region name (Nome da região padrão): copie e cole a região fornecida na página AWS Details (Detalhes da AWS) no Vocareum.
Dica Pode ser necessário pressionar Ctrl+Shift+V para colar no gerenciador de sessão.
Default output format (Formado de saída padrão): pressione Enter.
O arquivo de configuração da AWS será criado e você o atualizará em uma etapa mais adiante. As entradas anteriores de 1 são espaços reservados temporários.
<img width="316" height="118" alt="image" src="https://github.com/user-attachments/assets/85ada3a9-9318-446e-9176-535863d07b00" />



