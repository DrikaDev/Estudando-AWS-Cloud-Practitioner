Lab - Proteção de dados usando criptografia

Criptografia é o processo de transformar informações em código secreto para garantir confidencialidade, autenticação, integridade e não repúdio.  
O processo inverso é a descriptografia, que torna os dados legíveis novamente.

Neste laboratório seremos capazes:
Criar uma chave de criptografia do AWS KMS
Instalar a AWS Encryption CLI
Criptografar texto simples
Descriptografar texto cifrado

---

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

---

Tarefa 2: configurar a instância do servidor de arquivos  
Antes de criptografar ou descriptografar dados, você precisa definir alguns pontos.  
Para usar sua chave AWS KMS, você deve configurar as credenciais da AWS na instância do EC2 no File Server (Servidor de arquivos).  
Depois disso, instale a AWS Encryption CLI (aws-encryption-cli), que você pode usar para executar os comandos de criptografar e descriptografar.  

Nesta tarefa, vamos configurou o arquivo de credenciais da AWS que habilita o uso da chave do AWS KMS que você criou em uma etapa anterior.  
Vamos instalar a AWS Encryption CLI para conseguir executar comandos de criptografia.  

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

6. Navegue até à página do console Vocareum e selecione o botão  AWS Details (Detalhes da AWS).
Ao lado de AWS CLI, selecione Show (Exibir).
Copie e cole o bloco de código, que começa com [default], em um editor de texto.
Retorne à guia do navegador em que você está logado no servidor de arquivos.
Para abrir o arquivo de credenciais da AWS, execute o seguinte comando:
```
vi ~/.aws/credentials
```

7. No arquivo ~/.aws/credentials file, digite dd várias vezes para excluir o conteúdo do arquivo.
Cole o bloco de código que você copiou do Vocareum.
<img width="727" height="135" alt="image" src="https://github.com/user-attachments/assets/95a63897-ced2-42d5-9d09-65ff3ebd6366" />

8. Para salvar e fechar o arquivo, pressione Esc, digite :wq e pressione Enter.
Para visualizar o conteúdo atualizado do arquivo, execute o seguinte comando:
```
cat ~/.aws/credentials
```
Agora você instalará a AWS Encryption CLI e exportar o caminho. Fazendo isso, você poderá executar os comandos para criptografar e descriptografar dados.  

9. Para instalar a AWS Encryption CLI e definir o caminho, execute os seguintes comandos:
```
pip3 install aws-encryption-sdk-cli
export PATH=$PATH:/home/ssm-user/.local/bin
```

---

Tarefa 3: criptografar e descriptografar dados  

Nesta tarefa, você criará um arquivo de texto com uma simulação de dados sigilosos. Depois, você usará a criptografia para proteger o conteúdo do arquivo. A próxima etapa é descriptografar e visualizar o conteúdo do arquivo.

1. Para criar o arquivo de texto, execute os seguintes comandos:
```
touch secret1.txt secret2.txt secret3.txt
echo 'TOP SECRET 1!!!' > secret1.txt
```


