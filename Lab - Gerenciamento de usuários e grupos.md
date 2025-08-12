## Lab - Gerenciamento de Usuários e Grupos no Linux

## Objetivos
Neste laboratório, você vai:
- Criar novos usuários com uma senha padrão.
- Criar grupos e atribuir os usuários apropriados.
- Fazer login como diferentes usuários.

---

## Tarefa 1: Usar SSH para se conectar a uma instância do Amazon Linux EC2
Primeiro devemos nos conectar a uma instância do EC2 do Amazon Linux usando um utilitário SSH para realizar as operações.

---

## Tarefa 2: Criar usuários
Nesta seção, você criará usuários com base na tabela a seguir:  
**Importante:** fique atento à ortografia correta das IDs de usuário para que possam usar as credenciais padrão para fazer o login.  
<img width="1309" height="575" alt="image" src="https://github.com/user-attachments/assets/aeda7e23-bd78-4631-9c20-3709f275b321" />  

1. Confirme que você está na pasta inicial do usuário atual digitando **pwd** e pressionando ENTER.
<img width="293" height="73" alt="image" src="https://github.com/user-attachments/assets/3c5cab70-8e34-4a16-91cb-077a2ae63977" />

2. Para adicionar o primeiro usuário da lista anterior, Alejandro Rosalez, digite **sudo useradd arosalez** e pressione Enter.  
<img width="450" height="41" alt="image" src="https://github.com/user-attachments/assets/22c923f6-144e-4a27-b749-ed58c14bcc3d" />

3. Para adicionar a senha, digite **sudo passwd arosalez** e pressione Enter. 
A senha deverá ser digitada duas vezes. Você pode usar a senha P@ssword1234!  
- Observação: Ao digitar a senha, nada aparecerá na tela, portanto digite sua senha e pressione Enter.  
<img width="498" height="120" alt="image" src="https://github.com/user-attachments/assets/502fea69-28ce-4385-884f-cc948cba33f5" />

4. Para validar se os usuários foram criados, digite *sudo cat /etc/passwd | cut -d: -f1* e pressione Enter.  
<img width="568" height="23" alt="image" src="https://github.com/user-attachments/assets/34b02de6-df97-4932-9653-4deae09a8979" />  
<img width="271" height="43" alt="image" src="https://github.com/user-attachments/assets/9682aa1d-9fe5-4900-9e6e-def5b6623331" />  
- Observação: Este comando ajuda a visualizar os usuários criados.  
- **cat** é um dos comandos mais populares. Uma de suas finalidades é exibir arquivos.  

5. Use os comandos **sudo useradd <User ID>** e **sudo passwd <User ID>** para incluir os usuários restantes da tabela.  
Para verificar se todos os usuários foram criados, digite **sudo cat /etc/passwd | cut -d: -f1** e pressione Enter.  
<img width="565" height="20" alt="image" src="https://github.com/user-attachments/assets/ce7982b3-62e6-45e5-a7c6-c5dc5fbff518" />  
<img width="268" height="231" alt="image" src="https://github.com/user-attachments/assets/f56a5558-b221-439d-a20f-33efca982682" />  

---

## Tarefa 3: Criar grupos  
Nesta seção, você criará grupos de usuários e adicionará usuários aos grupos: 
- Sales  
- HR  
- Finance  
- Personnel
- CEO
- Shipping  
- Managers

Quando tiver criado esses grupos, você incluirá os usuários aos devidos grupos com base nas informações fornecidas na tabela da Tarefa 2.  

1. Confirme que você está na pasta inicial do usuário atual digitando **pwd** e pressionando Enter.  
Para verificar se o grupo foi adicionado, digite **cat /etc/group** e pressione Enter.  
<img width="429" height="42" alt="image" src="https://github.com/user-attachments/assets/6835bb32-fa6f-46d5-9718-4ec0c200665c" />  
<img width="250" height="41" alt="image" src="https://github.com/user-attachments/assets/6de82a9b-739c-421d-b0aa-bd8af03a0189" />  

2. Use o comando **sudo groupadd <Group>** para incluir os grupos restantes.  
<img width="465" height="117" alt="image" src="https://github.com/user-attachments/assets/c1fbcc74-5a69-4a4e-b535-faa1aac3ff72" />  

3. Para verificar se todos os grupos foram adicionados, digite **cat /etc/group** e pressione Enter.  
<img width="385" height="21" alt="image" src="https://github.com/user-attachments/assets/af788db3-b0e3-4f9e-89cb-de26021a3060" />
<img width="256" height="153" alt="image" src="https://github.com/user-attachments/assets/ab61b4da-0399-4916-95dc-5468c6b19a00" />

4. Para adicionar o usuário arosalez ao grupo Sales, digite **sudo usermod -a -G Sales arosalez** no terminal e pressione Enter.  
<img width="551" height="42" alt="image" src="https://github.com/user-attachments/assets/87825b98-f237-4538-8a0a-1ce87075d25d" />  

5. Para verificar se o usuário foi adicionado, digite **cat /etc/group e pressione** Enter.  
<img width="383" height="23" alt="image" src="https://github.com/user-attachments/assets/0c6d54b9-78a2-4a0e-8302-106dd478387e" />  
<img width="202" height="18" alt="image" src="https://github.com/user-attachments/assets/8eaef09c-5f03-454a-8f51-61539c98448a" />

6. Use o comando **sudo usermod -a -G <Group Name> <User ID>** para adicionar os usuários restantes aos grupos adequados.  

7. Para verificar as participações no grupo, digite **sudo cat /etc/group** no terminal e pressione Enter.  
<img width="426" height="21" alt="image" src="https://github.com/user-attachments/assets/3c77fe80-5fe9-4678-aeee-038c432120a1" />  
<img width="324" height="156" alt="image" src="https://github.com/user-attachments/assets/e9b08fac-81f6-4b3d-8092-6cfd30a10a3d" />  

8. Acrescente **ec2-user** a todos os grupos.  
<img width="591" height="134" alt="image" src="https://github.com/user-attachments/assets/13872402-c4e8-4671-9e2b-e4c55eac9bd7" />  

9. Para verificar as participações no grupo, digite **sudo cat /etc/group** no terminal e pressione Enter.  
<img width="387" height="22" alt="image" src="https://github.com/user-attachments/assets/763e1109-db92-4634-ad58-1918d1a5088d" />  
<img width="407" height="156" alt="image" src="https://github.com/user-attachments/assets/f355fda1-846c-4b43-a420-85ce3dbc5bed" />  

---

## Tarefa 4: Faça o login usando os novos usuários  
Agora que você tem alguns usuários em sua máquina, pode fazer o login como um novo usuário.  
Você também pode ver o que é um **sudoer**, o que isso ativa e como os comandos emitidos usando **sudo** são registrados em log no arquivo /var/log/secure.  

1. Digite **su arosalez**  
Para a senha, digite **P@ssword1234!** e pressione Enter.  
Agora você está conectado como **arosalez**.  
<img width="358" height="66" alt="image" src="https://github.com/user-attachments/assets/3eaf7799-609b-4572-a100-6061e56b7051" />  
O **ec2-user** final indica que você está localizado no diretório inicial do ec2-user, /home/ec2-user.  

2. Digite **pwd** e pressione Enter para garantir que você esteja no diretório /home/ec2-user.  
<img width="356" height="62" alt="image" src="https://github.com/user-attachments/assets/7224b375-0163-4fb9-8e08-4f074a989e6e" />  

3. Digite **touch myFile.txt** e pressione Enter.  
<img width="464" height="39" alt="image" src="https://github.com/user-attachments/assets/1f3c9570-2181-4fb2-89fb-9a1cc4de235c" />  
Esta mensagem aparece porque o usuário arosalez não tem permissão para gravar arquivos na pasta inicial ec2-user.  

4. Agora você tenta como administrador usando o comando **sudo**.  
Digite **sudo touch myFile.txt** e pressione Enter.  
Digite a senha **P@ssword1234!** e pressione Enter.  
<img width="630" height="230" alt="image" src="https://github.com/user-attachments/assets/c68baebd-5fdd-4211-a9c6-2df9fce6ee1d" />  
Esta mensagem aparece porque o usuário arosalez não se encontra na lista do arquivo de sudoers.  
Sudoers são usuários que possuem direitos especiais para executar comandos que exigem direitos de root.  
Somente alguns usuários deverão receber essa permissão.  

5. Digite **exit** e pressione Enter para voltar ao usuário anterior, ec2-user.  
<img width="360" height="60" alt="image" src="https://github.com/user-attachments/assets/aa4dd89f-159a-4ccf-bdc1-226708a6fbe3" />  

6. Agora você visualiza o conteúdo do arquivo **/var/log/secure**.  
Digite **sudo cat /var/log/secure** e pressione Enter para exibir o conteúdo do arquivo seguro.  
Desça até o final do arquivo usando a seta para baixo.  
<img width="1326" height="36" alt="image" src="https://github.com/user-attachments/assets/4250c184-2b19-4bb9-81fb-a6b306e3f7df" />  

---
