## 🧪 Lab 190 - Automação com o AWS CloudFormation

Implantar uma infraestrutura de maneira consistente e confiável é difícil, 
pois requer que as pessoas sigam procedimentos documentados sem pegar atalhos não documentados.  
Além disso, pode ser difícil implantar a infraestrutura fora do horário comercial quando há menos funcionários disponíveis.  

O **AWS CloudFormation** muda esse cenário ao permitir que a infraestrutura seja definida em um modelo que pode ser implantado automaticamente, 
inclusive em cronogramas automatizados.  

Este laboratório oferece experiência prática na implantação e edição de pilhas do CloudFormation.  
Trata-se de uma experiência interativa, sendo necessário consultar a documentação para descobrir como definir recursos dentro de um modelo do CloudFormation.

## Objetivos do laboratório

- Implantar uma pilha do AWS CloudFormation com uma **VPC** e um **grupo de segurança**.
- Configurar uma pilha do AWS CloudFormation com recursos como:
  - Amazon S3
  - Amazon EC2
- Encerrar uma pilha do AWS CloudFormation e seus respectivos recursos.

## Tarefa 1: Implantar uma pilha do CloudFormation

Nesta tarefa, vamos implantar uma pilha do CloudFormation que cria uma **VPC**, conforme o diagrama apresentado no laboratório.

<img width="2391" height="1142" alt="image" src="https://github.com/user-attachments/assets/c6f67979-2c22-45e0-b576-c25e3a549001" />

### Passos iniciais

1. Clique no link fornecido pelo laboratório e faça o download do modelo: `task1.yaml` 

2. Abra o arquivo em um **editor de texto** (não use um processador de texto).

3. Analise o conteúdo do arquivo. Você encontrará as seguintes seções:

#### Seções do modelo

- **Parameters**  
  Usada para solicitar entradas que podem ser reutilizadas em outras partes do modelo.  
  Neste caso, o modelo solicita dois intervalos de endereços IP (CIDR) para definir a VPC.

<img width="591" height="339" alt="image" src="https://github.com/user-attachments/assets/8c0fc8eb-61c3-4bf3-93fe-be2b1c2b8ccd" />

- **Resources**  
  Define a infraestrutura a ser implantada.  
  O modelo cria:
  - Uma VPC

  <img width="549" height="363" alt="image" src="https://github.com/user-attachments/assets/1067c06c-c49f-4332-983d-7f6c6c0f3084" />

  - Um grupo de segurança

  <img width="594" height="457" alt="image" src="https://github.com/user-attachments/assets/8aafe37c-54e1-4d91-be69-0556eb075c94" />

- **Outputs**  
  Fornece informações seletivas sobre os recursos criados.  
  O modelo retorna o grupo de segurança padrão da VPC.

<img width="656" height="215" alt="image" src="https://github.com/user-attachments/assets/d5ddeaa3-d90f-438c-92da-d42ca61e0dc5" />

> O modelo está escrito em **YAML**, um formato muito utilizado para arquivos de configuração.  
> ⚠️ Atenção aos recuos e hifens, pois eles são essenciais.  
> O CloudFormation também aceita modelos em **JSON**.

---

### Criando a pilha no console da AWS

1. Acesse o **Console de Gerenciamento da AWS**.
2. No menu **Serviços**, clique em **CloudFormation**.
3. Clique em **Criar pilha** e siga os passos:

<img width="1432" height="342" alt="image" src="https://github.com/user-attachments/assets/8eab4948-2903-4f2f-814c-7c2d712e86e0" />

   - Selecione **Fazer upload de um arquivo de modelo**
   - Faça upload do arquivo `task1.yaml`
   - Clique em **Próximo**

<img width="1101" height="319" alt="image" src="https://github.com/user-attachments/assets/ffbc9a00-1565-46db-9cbf-eca2a24f4f76" />

4. Em **Specify Details (Especificar detalhes da pilha)**:
   - **Nome da pilha:** `Lab`
   - Mantenha os valores padrão dos parâmetros CIDR
   - Clique em **Próximo**

<img width="1115" height="503" alt="image" src="https://github.com/user-attachments/assets/8558b8fe-c53a-4400-bdad-8575e29c437c" />

5. Na página **Configurar opções da pilha**, mantenha as configurações padrão e clique em **Próximo**.

6. Na página **Revisar e criar**:
   - Concorde com o uso de nomes personalizados
   - Clique em **Enviar**

---

### Acompanhando a criação

- O status da pilha será **CREATE_IN_PROGRESS**

<img width="982" height="296" alt="image" src="https://github.com/user-attachments/assets/8475bf27-cb21-42b8-b03f-f2b3c2f40987" />

- Na aba **Eventos**, acompanhe as ações executadas
- Na aba **Recursos**, visualize a ordem de criação
- Aguarde até o status mudar para **CREATE_COMPLETE**

> Opcional: acesse o console da **VPC** para visualizar a VPC criada.

<img width="1420" height="227" alt="image" src="https://github.com/user-attachments/assets/96fe2daa-c821-4b40-871a-b6444e3a62f0" />

---

## Tarefa 2: Adicionar um bucket do Amazon S3 à pilha

Nesta tarefa, vamos editar o modelo do **CloudFormation** para adicionar um **bucket do Amazon S3** e atualizar a pilha existente.

### Objetivo

- Adicionar um bucket do Amazon S3 ao modelo
- Atualizar a pilha para implantar o novo recurso

### Dicas importantes

- Edite o arquivo `task1.yaml`
- Consulte a documentação: **Amazon S3 – Model Snippets**
  `https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/quickref-s3.html`
- Veja o exemplo YAML
<img width="735" height="133" alt="image" src="https://github.com/user-attachments/assets/2c760a05-2c7c-4b18-a9e7-2b7ec539ea75" />

- Seu código deve ser adicionado na seção **Resources**
- Nenhuma propriedade é obrigatória para o bucket
- Use **dois espaços** para cada nível de recuo no YAML

> 💡 A solução correta exige apenas duas linhas:
> - Identificador do recurso: MyS3Bucket
> - Tipo do recurso: Type: AWS::S3::Bucket

```
MyS3Bucket:
    Type: AWS::S3::Bucket
```
    
### No CloudFormation, todo recurso fica dentro da seção **Resources:**, no mesmo nível de indentação dos outros recursos (VPC, Subnet, RouteTable etc.).  

👉🏼 O bucket S3 não depende da VPC, então ele pode ficar em qualquer lugar dentro de **Resources:**.  
Por organização, o mais comum é colocar no início ou no final do arquivo, em uma seção separada.  
Exemplo recomendado:    
Logo após a linha **Resources:**, antes da VPC, ficaria assim:  

<img width="695" height="295" alt="image" src="https://github.com/user-attachments/assets/2ce5138d-30f4-4a7f-827a-2220b2279cd0" />

---

### Atualizando a pilha

1. No console do CloudFormation, selecione a pilha **Lab**
2. Clique em **Atualizar**

<img width="1390" height="338" alt="image" src="https://github.com/user-attachments/assets/756393b7-8f28-4f79-9cca-b40e4bcd4d5e" />

3. Escolha:
   - **Substituir modelo atual/existente**

<img width="1104" height="258" alt="image" src="https://github.com/user-attachments/assets/c337f1c6-1b35-45d8-9d93-059df6ec83e6" />

   - **Fazer upload de um arquivo de modelo**

<img width="1099" height="165" alt="image" src="https://github.com/user-attachments/assets/33dd3fb0-61c2-4035-8839-8b6b864ba5f6" />

5. Faça upload do `task1.yaml` modificado

<img width="1109" height="130" alt="image" src="https://github.com/user-attachments/assets/7a62c081-c02d-454d-a97c-ba89e842c647" />

6. Clique em **Próximo** até chegar à visualização das alterações

Você verá uma **pré-visualização** indicando a adição do bucket S3, enquanto os demais recursos permanecem inalterados.

<img width="408" height="120" alt="image" src="https://github.com/user-attachments/assets/78ba7d8e-ddc6-45b0-8265-1774226e5480" />

6. Clique em **Atualizar pilha**
7. Aguarde até o status mudar para **UPDATE_COMPLETE**

<img width="413" height="112" alt="image" src="https://github.com/user-attachments/assets/24cc96a5-eaa9-44d7-aaf9-b2af40729375" />

> ⚠️ O CloudFormation atribui um **nome aleatório** ao bucket para evitar conflitos.

- Solução de exemplo: `task2.yaml`
- Opcional: acesse o console do **S3** para visualizar o bucket criado

<img width="739" height="62" alt="image" src="https://github.com/user-attachments/assets/09a56723-8df7-433c-86bc-2dcc192f8dd3" />

---

## Tarefa 3: Adicionar uma instância do Amazon EC2 à pilha

Agora vamos adicionar uma **instância EC2** ao modelo e atualizar a pilha.

### Etapa 1: Adicionar um parâmetro de AMI

Inclua o seguinte parâmetro na seção **Parameters**:

```yaml
AmazonLinuxAMIID:
  Type: AWS::SSM::Parameter::Value<AWS::EC2::Image::Id>
  Default: /aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2
```

<img width="847" height="135" alt="image" src="https://github.com/user-attachments/assets/a5da4e8f-7757-4242-877c-a89622550076" />

> Esse parâmetro utiliza o **AWS Systems Manager Parameter Store** para obter automaticamente a AMI mais recente do Amazon Linux 2 na região da pilha.  

> 📘 Referência: *Query for the latest Amazon Linux AMI IDs using AWS Systems Manager Parameter Store*  
> `https://aws.amazon.com/pt/blogs/compute/query-for-the-latest-amazon-linux-ami-ids-using-aws-systems-manager-parameter-store/`  

👉🏼 Referenciando recursos no CloudFormation  
O CloudFormation permite referenciar outros recursos do modelo usando `!Ref.`  
Exemplo: `VpcId: !Ref VPC`  

<img width="940" height="228" alt="image" src="https://github.com/user-attachments/assets/1c28c4c1-8a40-41e1-8714-b56d062608ad" />
> Note que ele usa !Ref VPC para se referir ao recurso da VPC. Você usará essa técnica ao definir a instância do EC2.

### Etapa 2: Adicionar a instância EC2

<img width="1288" height="213" alt="image" src="https://github.com/user-attachments/assets/ca06617b-2c92-4ba1-b88e-0555ccb4c5d4" />

Adicione um recurso AWS::EC2::Instance na seção **Resources** com estas 05 propriedades:  

- ImageId: !Ref AmazonLinuxAMIID / consulte `AmazonLinuxAMIID`, que é o parâmetro adicionado na etapa anterior.  
- InstanceType: `t3.micro`
- SecurityGroupIds: - !Ref AppSecurityGroup / consulte `AppSecurityGroup`, que é definido em outro lugar no modelo.
- SubnetId: !Ref PublicSubnet / consulte `AppSecurityGroup`, que é definido em outro lugar no modelo.
- Tags - use esse bloco YAML:
  ```
  Tags:
    - Key: Name
      Value: App Server
  ```

<img width="516" height="364" alt="image" src="https://github.com/user-attachments/assets/ab690552-f3a3-4ab8-9447-5e891eb7c7c4" />

📘 Documentação de apoio: AWS::EC2::Instance
`https://docs.aws.amazon.com/AWSCloudFormation/latest/TemplateReference/aws-resource-ec2-instance.html`  

### Atualizando a pilha
- Faça o upload do modelo atualizado conforme já atualizamos na etapa anterior  

<img width="1106" height="574" alt="image" src="https://github.com/user-attachments/assets/dd063a81-c139-4e71-b60b-ec83f9432416" />

- Verifique a pré-visualização das alterações
- Atualize a pilha
- Solução de exemplo: task3.yaml
- Opcional: acesse o console do EC2 para visualizar o **App Server**  

<img width="1420" height="204" alt="image" src="https://github.com/user-attachments/assets/c14ae3ba-c152-4920-8d7a-1a477ece77a7" />

### Tarefa 4: Excluir a pilha

Quando uma pilha do CloudFormation é excluída, todos os recursos criados por ela também são removidos automaticamente.

- No console do CloudFormation, selecione a pilha `Lab`  
- Clique em Excluir
- Confirme em Excluir pilha

<img width="610" height="237" alt="image" src="https://github.com/user-attachments/assets/2380b224-fe15-4a93-b208-9261477efb36" />

O status será **DELETE_IN_PROGRESS** e, após alguns minutos, a pilha desaparecerá.

<img width="405" height="121" alt="image" src="https://github.com/user-attachments/assets/0366a8bf-cf68-4041-80da-9cd2cc3ff6da" />

> ⚠️ Opcional: confirme que o bucket S3, a instância EC2 e a VPC foram excluídos.

### Laboratório concluído 🎉
Parabéns! Você concluiu com sucesso o laboratório de Automação com o AWS CloudFormation.

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Solutions-Architect-Associate/blob/main/README.md) 📒
