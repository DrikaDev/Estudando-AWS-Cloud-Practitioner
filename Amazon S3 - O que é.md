## 🪣 Amazon S3 - Simple Storage Service

## Índice
- [O que é o Amazon S3?](#o-que-é-o-amazon-s3)
- [Características do Amazon S3](#características-do-amazon-s3)
- [Abordagens para controle de acesso ao Amazon S3](#abordagens-para-controle-de-acesso-ao-amazon-s3)
- [Modelo de consistência de dados no Amazon S3](#modelo-de-consistência-de-dados-no-amazon-s3)
- [Amazon S3 Intelligent-Tiering](#amazon-s3-intelligent-tiering)
- [Ciclo de vida no Amazon S3](#ciclo-de-vida-no-amazon-s3)
- [Criar notificação de evento](#criar-notificação-de-evento)
- [Versionamento de bucket](#versionamento-de-bucket)
  
---

## O que é o Amazon S3?

O **Amazon S3 (Simple Storage Service)** é um serviço da AWS de armazenamento de **objetos** na nuvem de forma **segura, escalável e acessível**.  

Ele é ideal para:

- Guardar **imagens, vídeos, documentos** e outros arquivos.  
- Hospedar **sites estáticos** compostos apenas por HTML, CSS e JavaScript.  
  👉 Exemplos: **portfólios pessoais, currículos online, landing pages**.

[⬆ Voltar ao índice](#índice)

---

## Características do Amazon S3

- Cada **Bucket** tem um nome único / exclusivo.  
- O tamanho máximo de um arquivo / objeto é de **5 TB**.  
- Todos os objetos possuem:
  - **Chave**  
  - **ID de versão**  
  - **Valor**  
  - **Metadados**  
  - **Sub-recursos**
- Oferece **99,999999999% de durabilidade**, garantindo que os dados não sejam perdidos.  
  - Exemplo: Se armazenar **10.000 objetos**, pode ocorrer a perda média de **um único objeto a cada 10.000.000 anos**.  
- Não fica dentro de uma **VPC**, mas a nível de **Região** que você designar.  
- Foi criado para suportar falhas simultâneas de dispositivos, detectando rapidamente e reparando redundâncias perdidas.  
- Algumas aplicações utilizam até mesmo o **S3 como back-end**.  
- Disponibilidade de **99,99%** na classe **S3 Standard**.  

[⬆ Voltar ao índice](#índice)

---

## Abordagens para controle de acesso ao Amazon S3

- **Configuração Default**: somente o dono da conta consegue acessar.  
- **Acesso público**: usado para hospedagem de sites.  
- **Políticas de acesso**: controlam quais usuários terão acesso ao bucket.  
  - **Políticas do IAM**: adote o princípio do **privilégio mínimo**, concedendo apenas permissões necessárias.  
  - **URLs pré-assinadas**: concedem acesso temporário a outras pessoas.  

<img width="991" height="342" alt="Controle de acesso Amazon S3" src="https://github.com/user-attachments/assets/5f2a6178-b9c6-4270-ab27-c0151ba55726" />

- **Criptografia**: somente quem possui a chave secreta pode decodificar os dados.  
  - Use o **AWS KMS (Key Management Service)** para gerenciar chaves.  
  - **No lado do servidor**: habilite a criptografia padrão — o S3 criptografa os objetos ao salvar e descriptografa ao baixar.  
  - **No lado do cliente**: você mesmo gerencia a criptografia, enviando dados já criptografados.  

[⬆ Voltar ao índice](#índice)

---

## Modelo de consistência de dados no Amazon S3

- **Consistência imediata** para operações **PUT de novos objetos**.  

<img width="435" height="178" alt="Consistência de leitura" src="https://github.com/user-attachments/assets/e066f4c9-8f30-41a9-bc64-4629b9ea6a23" />

- **Consistência eventual** para **substituição ou deleção de arquivos**.  
  - Pode ocorrer de retornar uma versão antiga ou levar tempo para propagar em todas as cópias.  

<img width="436" height="182" alt="Consistência eventual" src="https://github.com/user-attachments/assets/81099ada-62c7-49fe-8255-c8431d66e0fa" />  

[⬆ Voltar ao índice](#índice)

---

## Amazon S3 Intelligent-Tiering

O **S3 Intelligent-Tiering** (Camadas de Classes de Armazenamento Inteligente) foi projetado para otimizar os custos movendo automaticamente os objetos para o nível de acesso mais econômico com base no padrão de uso/acesso dos dados.

- **S3 Standard**: dados acessados com frequência.  
- **S3 Standard-IA (Infrequent Access)**: dados duradouros, acessados ocasionalmente.  
- **S3 One Zone-IA (Infrequent Access)**: dados duradouros, **não críticos** e acessados com pouca frequência.  
- **S3 Glacier / Deep Archive**: arquivamento de dados raramente acessados.  

[⬆ Voltar ao índice](#índice)

---

## Ciclo de vida no Amazon S3

Você pode definir políticas de **ciclo de vida** para personalizar e gerenciar como os objetos serão armazenados ao longo do tempo.  

- Os dados podem migrar automaticamente entre classes de armazenamento **sem impactar as aplicações**.  
- Exemplos de políticas:
  - **Mover** objetos antigos para classes de menor custo.  
  - **Excluir** objetos após determinado período.  

<img width="991" height="267" alt="Ciclo de vida Amazon S3" src="https://github.com/user-attachments/assets/2aafe898-1cae-4b69-a231-cb908dfab150" />  

[⬆ Voltar ao índice](#índice)

---

## Criar notificação de evento

No Amazon S3, podemos configurar notificações para serem enviadas quando determinados eventos ocorrerem em um bucket.  
É possível especificar **um ou mais tipos de eventos** para os quais você deseja receber notificações.

### Exemplos de eventos
- Quando um **novo arquivo** é adicionado ao bucket.
- Quando um **objeto existente é modificado**.
- Quando um **objeto é removido**.
- Quando ocorrer eventos de **transição** ou **expiração** de objetos, com base nas políticas de ciclo de vida configuradas no bucket.

Essas notificações fazem parte de uma **arquitetura orientada a eventos**.  
Com base no evento, podemos acionar (**trigger**) outros serviços, como por exemplo:

- **AWS Lambda** – Executa um código específico quando o evento é disparado.
- **Amazon SNS** – Publica uma mensagem para múltiplos assinantes.
- **Amazon SQS** – Envia mensagens para filas que podem ser processadas posteriormente.

<img width="1847" height="461" alt="image" src="https://github.com/user-attachments/assets/68b16c1c-e932-427e-a02d-61f1c250b846" />  

<img width="1833" height="620" alt="image" src="https://github.com/user-attachments/assets/491df2de-2bea-428f-b70b-04943f191c44" />  

<img width="1829" height="267" alt="image" src="https://github.com/user-attachments/assets/a8a6de09-07db-4b06-acb5-e8f3593fdc7a" />  

<img width="1832" height="537" alt="image" src="https://github.com/user-attachments/assets/89a74663-6446-48a5-b980-ac5cfd2f3f48" />  

<img width="1837" height="512" alt="image" src="https://github.com/user-attachments/assets/944959c8-200b-4f3b-91e3-ae9a0609dc65" />  


Dessa forma, conseguimos criar fluxos automatizados, como processar imagens assim que forem enviadas para o bucket, gerar logs, ou atualizar bancos de dados.

[⬆ Voltar ao índice](#índice)

---

## Versionamento de Bucket

O **Amazon S3** oferece uma opção de **versionamento de código** que é uma infraestrutura de armazenamento resiliente, ou seja, ele oferece um nível adicional de proteção extra.  

**Como funciona?**  

Quando ativado, o **versionamento** mantém **'todas as versões'** de um objeto no mesmo bucket. Isso permite recuperar arquivos **excluídos por engano**, ou restaurar versões anteriores.  

Por exemplo:  
- Você envia a **versão 1** de um arquivo.  
- Depois envia a **versão 2** — o S3 mantém **as duas versões**.  
- Se excluir a versão 2, ainda pode recuperar a versão 1.  
- E mesmo que exclua a versão 1, o S3 **não remove o arquivo de imediato** — ele coloca um **marcador de exclusão**.  

<img width="581" height="257" alt="image" src="https://github.com/user-attachments/assets/4da50926-4176-487c-85e5-447046417d86" />

Assim, o versionamento pode ser usado para **preservar, recuperar e restaurar** arquivos, prevenindo perdas em casos de **'erros humanos'** ou falhas na aplicação.  

---

### Como ativar o versionamento  

1. **Ative a função** no bucket:  
<img width="1393" height="174" alt="image" src="https://github.com/user-attachments/assets/169417c4-5e39-48ac-8972-9f4aa302d102" />

2. Depois de ativar, o botão **"Mostrar versões"** ficará disponível:

<img width="214" height="52" alt="image" src="https://github.com/user-attachments/assets/b3d8e49a-4c7d-4195-b20b-de31ac0ea8f8" />

<img width="1869" height="419" alt="image" src="https://github.com/user-attachments/assets/92f02168-01a5-4840-90d7-b8b26ce6d65a" />

4. Ao excluir um arquivo:  
<img width="1840" height="492" alt="image" src="https://github.com/user-attachments/assets/d8a807db-5aff-4149-8e8e-23b130df618b" />  

<img width="1850" height="267" alt="image" src="https://github.com/user-attachments/assets/0d85ace2-2e9d-47a5-b29b-c8183f6a9cf2" />  

4. Clique em **"Mostrar versões"** para visualizar:  
<img width="1866" height="377" alt="image" src="https://github.com/user-attachments/assets/4c4e2a2c-04b8-4dd4-b4bc-356ae28b21b3" />  

5. Você verá o **marcador de exclusão**:  
<img width="1242" height="192" alt="image" src="https://github.com/user-attachments/assets/13f013d5-626c-4f36-8b9c-bee7caa2853a" />

6. Para restaurar o arquivo, basta **remover o marcador**:  
<img width="1845" height="329" alt="image" src="https://github.com/user-attachments/assets/add52dd5-1233-4234-8156-eb6371225d93" />

E pronto, o arquivo estará de volta:  
<img width="1866" height="307" alt="image" src="https://github.com/user-attachments/assets/100da4e1-25bc-4e9f-8cd7-431017edcba0" />

[⬆ Voltar ao índice](#índice)

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
