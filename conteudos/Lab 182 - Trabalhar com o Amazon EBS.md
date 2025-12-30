## 🧪 Lab 182 - Trabalhar com o Amazon EBS

O Amazon Elastic Block Store (Amazon EBS) é um serviço de armazenamento em bloco dimensionável e de alto desempenho projetado para o Amazon Elastic Compute 
Cloud (Amazon EC2).  
Neste laboratório, vamos aprender a criar um volume do EBS e a executar operações nele, como anexá-lo a uma instância, criar um sistema de arquivos e fazer 
um backup de snapshot.  

<img width="1626" height="470" alt="image" src="https://github.com/user-attachments/assets/10cf76b7-197c-4507-ac76-a9abeb4815c9" />

## 🎯 Objetivo
Criar, anexar e configurar um volume do **Amazon EBS** em uma instância **EC2 Linux**, adicionando um novo sistema de arquivos persistente.

## Tarefa 1: Criar um volume do EBS

1. No **Console de Gerenciamento da AWS**, na barra de pesquisa, digite **EC2** e abra o console do EC2.
2. No menu à esquerda, selecione **Instâncias**.
3. Uma instância chamada **Lab** já estará em execução.
4. **Anote a Zona de Disponibilidade** da instância Lab (exemplo: `us-west-2a`).  
   > 💡 Pode ser necessário rolar a tela para a direita para visualizar essa coluna.

<img width="1122" height="157" alt="image" src="https://github.com/user-attachments/assets/de085477-0adf-421d-a663-ea9f1ea5e20c" />

5. No menu à esquerda, em **Elastic Block Store**, selecione **Volumes**.
6. Você verá um volume existente de **8 GiB** em uso pela instância.

<img width="1182" height="175" alt="image" src="https://github.com/user-attachments/assets/17de4ebe-6f66-4719-b1ac-435fd8dcb29b" />

7. Clique em **Criar volume** e configure:
   - **Tipo de volume**: General Purpose SSD (**gp2**)
   - **Tamanho (GiB)**: `1`
   - **Zona de Disponibilidade**: a mesma da instância Lab (ex: `us-west-2a`)

<img width="1384" height="443" alt="image" src="https://github.com/user-attachments/assets/9a16f7a9-406d-4191-bd4f-42c119ab1f56" />

8. Em **Tags (opcional)** - clique em "Adicionar tag":
   - **Chave**: `Name`
   - **Valor**: `My Volume`
9. Clique em **Criar volume**.

<img width="1385" height="229" alt="image" src="https://github.com/user-attachments/assets/6f85d704-d194-4c43-95dc-9dbe96c18f70" />

🔄 O volume será criado com status **Criando**, que em seguida mudará para **Disponível**.  
> 💡 Use o botão **Atualizar** se necessário.

## Tarefa 2: Anexar o volume à instância EC2

1. Selecione o volume **My Volume**.
2. No menu **Ações**, clique em **Anexar volume**.
3. Em **Instância**, selecione a instância **Lab**.
4. O **Nome do dispositivo** será `/dev/sdf`.
5. Clique em **Associar volume**.

<img width="1397" height="450" alt="image" src="https://github.com/user-attachments/assets/521bab3c-4d36-48ff-9458-5ffbcadf5428" />

✅ O estado do volume mudará para **Em uso**.

## Tarefa 3: Conectar-se à instância EC2 (Lab)

1. No Console da AWS, abra **EC2**.
2. Selecione **Instâncias**.
3. Marque a instância **Lab**.
4. Clique em **Conectar-se**.
5. Na aba **EC2 Instance Connect**, clique em **Conectar-se** novamente.

🔗 Uma nova aba do navegador será aberta com o **terminal da instância**.

> 💡 Caso o terminal trave, atualize a página ou repita o processo de conexão.

## Tarefa 4: Criar e configurar o sistema de arquivos

- Verificar os volumes existentes: `df -h`
<img width="672" height="181" alt="image" src="https://github.com/user-attachments/assets/5dfbf727-fb50-46ca-8521-5c97930b039f" />
👉 O novo volume ainda não aparece porque não foi formatado nem montado.

- Criar o sistema de arquivos (ext3): `sudo mkfs -t ext3 /dev/sdf`
<img width="594" height="393" alt="image" src="https://github.com/user-attachments/assets/d6f2a81c-f671-4fcf-ac91-6bcf02fa681c" />

- Criar o diretório de montagem: `sudo mkdir /mnt/data-store`

- Montar o volume: `sudo mount /dev/sdf /mnt/data-store`

- Garantir montagem automática após reboot: `echo "/dev/sdf   /mnt/data-store ext3 defaults,noatime 1 2" | sudo tee -a /etc/fstab`

- Verifique o arquivo: `cat /etc/fstab`

- Verificar novamente o armazenamento: `df -h`
<img width="576" height="188" alt="image" src="https://github.com/user-attachments/assets/f3364226-ca4a-4074-a2b6-f557550db71a" />

- Criar e testar um arquivo no novo volume: `sudo sh -c "echo some text has been written > /mnt/data-store/file.txt"`

- Verificar o conteúdo: `cat /mnt/data-store/file.txt`
✅ O texto gravado será exibido, confirmando que o volume EBS está funcionando corretamente.
<img width="534" height="50" alt="image" src="https://github.com/user-attachments/assets/ca7ad496-c3e6-45d7-9b0e-988a54989fd2" />

## Tarefa 5: Criar um snapshot do Amazon EBS

Os snapshots do Amazon EBS são armazenados no **Amazon Simple Storage Service (Amazon S3)** para maior durabilidade.  
Eles podem ser usados para:  

- Backup e restauração de dados  
- Criação de novos volumes (clonagem)  
- Compartilhamento entre contas AWS  
- Cópia entre Regiões AWS  

### Passo a passo

1. No Console de Gerenciamento do EC2, selecione **Volumes**.
2. Escolha o volume **My Volume**.
3. No menu **Ações**, selecione **Criar snapshot**.

<img width="1174" height="198" alt="image" src="https://github.com/user-attachments/assets/9f180f78-4745-4062-a21f-2ff1b34cfecf" />

4. Na seção **Tags**, escolha **Adicionar tag** e configure:
   - **Chave**: `Name`
   - **Valor**: `My Snapshot`
5. Selecione **Criar snapshot**.

<img width="1381" height="279" alt="image" src="https://github.com/user-attachments/assets/4d1f5f9e-128d-4b70-8b3d-b69446a6fb84" />

No painel de navegação à esquerda, selecione **Snapshots**.
O **Status** do snapshot inicialmente será **Pendente**
<img width="1192" height="161" alt="image" src="https://github.com/user-attachments/assets/822d8da4-07e9-481c-9556-d8339df4f300" />

e, após a conclusão, mudará para **Concluído**.
<img width="1193" height="159" alt="image" src="https://github.com/user-attachments/assets/ddf30aa2-7f02-45b4-9fea-7ceaaaa4ed27" />

> Observação: apenas os blocos de armazenamento usados são copiados para o snapshot.  
> Blocos vazios não ocupam espaço de armazenamento.  

### Excluir o arquivo do volume

- No terminal do EC2 Instance Connect, execute: `sudo rm /mnt/data-store/file.txt`

- Para verificar se o arquivo foi excluído: `ls /mnt/data-store/file.txt`
<img width="697" height="51" alt="image" src="https://github.com/user-attachments/assets/2df85714-7ffd-4457-b047-bc5df33b170b" />

> Saída esperada:  
> ls: não é possível acessar /mnt/data-store/file.txt: Arquivo ou diretório inexistente  

## Tarefa 6: Restaurar o snapshot do Amazon EBS

Se for necessário recuperar os dados armazenados em um snapshot, é possível restaurá-lo criando um novo volume do Amazon EBS a partir desse snapshot.  

### Tarefa 6.1: Criar um volume usando o snapshot

1. No Console de Gerenciamento do EC2, selecione o snapshot **My Snapshot**.
2. No menu **Ações**, escolha **Criar volume com o snapshot**.

<img width="1179" height="163" alt="image" src="https://github.com/user-attachments/assets/588757f6-eae8-4b7a-be94-ce3b7f9fac9b" />

3. Em **Zona de Disponibilidade**, selecione a mesma Zona de Disponibilidade usada anteriormente.
4. Na seção **Tags (opcional)**, escolha **Adicionar tag** e configure:
   - **Chave**: `Name`
   - **Valor**: `Restored Volume`
5. Selecione **Criar volume**.

Para visualizar o novo volume, no painel de navegação à esquerda, selecione **Volumes**.

<img width="1420" height="237" alt="image" src="https://github.com/user-attachments/assets/310c51c6-f66b-4325-ae3d-5e2df7734ef2" />

O **Status** do volume será **Disponível**.

> Ao restaurar um snapshot para um novo volume, é possível alterar configurações como tipo de volume, tamanho e Zona de Disponibilidade.

### Tarefa 6.2: Anexar o volume restaurado à instância do EC2

1. Selecione o volume **Restored Volume**.
2. No menu **Ações**, escolha **Anexar volume**.
3. Na lista suspensa **Instância**, selecione a instância **Lab**.
4. O campo **Nome do dispositivo** estará definido como `/dev/sdg`.
5. Selecione **Associar volume**.

<img width="1383" height="358" alt="image" src="https://github.com/user-attachments/assets/76f956c5-8bc9-4d4c-8adf-a49741a2ceea" />

> O **Status** do volume agora será **Em uso**.

### Tarefa 6.3: Montar o volume restaurado

No terminal do **EC2 Instance Connect**, execute os comandos abaixo.

- Criar o diretório para montagem do volume: `sudo mkdir /mnt/data-store2`  

- Montar o volume restaurado: `sudo mount /dev/sdg /mnt/data-store2`

- Verificar se o arquivo restaurado existe no volume: `ls /mnt/data-store2/file.txt`  

<img width="643" height="51" alt="image" src="https://github.com/user-attachments/assets/1ef70d2b-7ebe-48f2-8fe9-70e0b99dccdd" />

> Você deverá visualizar o arquivo file.txt, confirmando que os dados foram restaurados com sucesso.

## Conclusão

Neste laboratório, realizamos as seguintes atividades:  

- Restauramos um snapshot do Amazon EBS  
- Criamos um novo volume a partir de um snapshot  
- Anexamos e montamos o volume restaurado em uma instância EC2  

🎉 Laboratório concluído com sucesso!

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Solutions-Architect-Associate/blob/main/README.md) 📒
