## 🪣 Amazon S3 - Simple Storage Service

## Índice
- [O que é o Amazon S3?](#o-que-é-o-amazon-s3)
- [Características do Amazon S3](#características-do-amazon-s3)
- [Abordagens para controle de acesso ao Amazon S3](#abordagens-para-controle-de-acesso-ao-amazon-s3)
- [Classes de Armazenamento do Amazon S3](#classes-de-armazenamento-do-amazon-s3)
- [Ciclo de vida no Amazon S3 (Lifecycle Policy)](#ciclo-de-vida-no-amazon-s3-lifecycle-policy)
- [Criar notificação de evento](#criar-notificação-de-evento)
- [Versionamento de bucket](#versionamento-de-bucket)
- [Modelo de consistência de dados no Amazon S3](#modelo-de-consistência-de-dados-no-amazon-s3)
- [Informações adicionais relevantes do Amazon S3](#informações-adicionais-relevantes-do-amazon-s3)

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
- O tamanho máximo de um arquivo / objeto é de até **5 TB**.
- **Não há limite para o "volume total de dados"**, o S3 foi projetado para armazenar quantidades praticamente ilimitadas de dados.  
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
- Algumas aplicações usam o **S3 como back-end**.  
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
  - **AWS KMS (Key Management Service)** para gerenciar chaves.  
  - **No lado do servidor**: o S3 criptografa os objetos ao salvar e descriptografa ao baixar.  
  - **No lado do cliente**: você gerencia a criptografia antes de enviar os dados.  

[⬆ Voltar ao índice](#índice)

---

## Classes de Armazenamento do Amazon S3

O Amazon S3 oferece várias classes de armazenamento para equilibrar **custo x desempenho x durabilidade**, dependendo da forma como os dados serão acessados.  

<img width="710" height="420" alt="image" src="https://github.com/user-attachments/assets/5171ad2c-5eeb-4e8c-a404-243fcf9b3f75" />  

### Aqui estão as principais classes de armazenamento do S3:  

- **S3 Standard**  
  - Uso: acesso frequente (sites, apps, big data, analytics)  
  - Replicação: Multi-AZ  
  - Disponibilidade: 99,99%  
  - Custo: mais alto  
  - Recuperação: milissegundos  

- **S3 Standard-IA (Infrequent Access)**  
  - Uso: dados raramente acessados, mas críticos (backups, logs importantes)  
  - Replicação: Multi-AZ  
  - Disponibilidade: 99,9%  
  - Custo: menor que Standard, mas com cobrança por recuperação  
  - Recuperação: milissegundos  

- **S3 Intelligent-Tiering**  
  - Uso: quando não se sabe a frequência de acesso  
  - Replicação: Multi-AZ  
  - Disponibilidade: 99,9%  
  - Custo: varia conforme movimentação automática + taxa por objeto  
  - Recuperação: milissegundos  

- **S3 One Zone-IA**  
  - Uso: dados recriáveis, cópias secundárias de backup  
  - Replicação: 1 AZ  
  - Disponibilidade: 99,5%  
  - Custo: mais baixo que Standard-IA  
  - Recuperação: milissegundos  

- **S3 Glacier Instant Retrieval**  
  - Uso: arquivamento raro com acesso imediato (mídia, compliance, arquivos médicos)  
  - Replicação: Multi-AZ  
  - Disponibilidade: 99,9%  
  - Custo: mais barato que IA  
  - Recuperação: milissegundos  

- **S3 Glacier Flexible Retrieval (antigo Glacier)**  
  - Uso: arquivamento de longo prazo com recuperação flexível  
  - Replicação: Multi-AZ  
  - Disponibilidade: 99,9%  
  - Custo: muito baixo  
  - Recuperação:
    - Expedited (Acelerado) de 1–5 min
    - Standard (Padrão) de 3–5h
    - Bulk (Em massa) de 5–12h  

- **S3 Glacier Deep Archive**  
  - Uso: arquivamento de ultra longo prazo (7–10+ anos, compliance, histórico)  
  - Replicação: Multi-AZ  
  - Disponibilidade: 99,9%  
  - Custo: menor de todos  
  - Recuperação:
    - Standard (Padrão) até 12h
    - Bulk (Em massa) até 48h  

### Custo estimado por classe:

- **S3 Standard** → mais caro, acesso imediato  
- **S3 Standard-IA** → mais barato, cobrança por recuperação  
- **S3 Intelligent-Tiering** → varia conforme movimentação automática + taxa por objeto  
- **S3 One Zone-IA** → mais barato que Standard-IA, somente 1 AZ  
- **S3 Glacier Instant Retrieval** → barato, acesso imediato  
- **S3 Glacier Flexible Retrieval** → muito barato, recuperação de minutos a horas  
- **S3 Glacier Deep Archive** → mais barato de todos, recuperação em horas ou dias  

> 💡 Dica: combinar classes com **Lifecycle Policy** reduz custos automaticamente.

[⬆ Voltar ao índice](#índice)

---

## Ciclo de vida no Amazon S3 (Lifecycle Policy)

O **ciclo de vida** é um conjunto de regras que permite **automatizar** a movimentação ou **exclusão** de objetos ao longo do tempo.  
Você pode 'definir políticas' para personalizar e gerenciar como os objetos serão armazenados.  

- Dados podem migrar entre classes: **Standard → Standard-IA → Glacier → Deep Archive**.  
- Objetos podem ser **excluídos automaticamente** após determinado período.  
- Reduz custos e facilita gerenciamento sem intervenção manual. 

<img width="991" height="267" alt="Ciclo de vida Amazon S3" src="https://github.com/user-attachments/assets/2aafe898-1cae-4b69-a231-cb908dfab150" />   

> PS: Não existe "exclusão" como parte do Intelligent-Tiering. A exclusão só ocorre se você configurar uma **lifecycle policy** separada.  

[⬆ Voltar ao índice](#índice)

---

## Criar notificação de evento

No Amazon S3, podemos configurar **notificações** para serem enviadas quando determinados eventos ocorrerem em um bucket.  
É possível especificar **um ou mais tipos de eventos** para os quais você deseja receber notificações.  
Por exemplo:

- Quando um **novo arquivo** é adicionado ao bucket.
- Quando um **objeto existente é modificado**.
- Quando um **objeto é removido**.
- Quando ocorrer eventos de **transição** ou **expiração** de objetos, com base nas políticas de ciclo de vida configuradas no bucket.

Essas notificações fazem parte de uma **arquitetura orientada a eventos**.  
E, com base no **evento**, podemos acionar um **trigger** com outros serviços integráveis, como por exemplo:

- **AWS Lambda** – Executa um código específico quando o evento é disparado.
- **Amazon SNS** – Publica uma mensagem para múltiplos assinantes.
- **Amazon SQS** – Envia mensagens para filas que podem ser processadas posteriormente.

<img width="1847" height="461" alt="image" src="https://github.com/user-attachments/assets/68b16c1c-e932-427e-a02d-61f1c250b846" />  

<img width="1833" height="620" alt="image" src="https://github.com/user-attachments/assets/491df2de-2bea-428f-b70b-04943f191c44" />  

<img width="1829" height="267" alt="image" src="https://github.com/user-attachments/assets/a8a6de09-07db-4b06-acb5-e8f3593fdc7a" />  

<img width="1832" height="537" alt="image" src="https://github.com/user-attachments/assets/89a74663-6446-48a5-b980-ac5cfd2f3f48" />  

<img width="1837" height="512" alt="image" src="https://github.com/user-attachments/assets/944959c8-200b-4f3b-91e3-ae9a0609dc65" />  

> Dessa forma, conseguimos criar fluxos automatizados, como processar imagens assim que forem enviadas para o bucket, gerar logs, ou atualizar bancos de dados.

[⬆ Voltar ao índice](#índice)

---

## Versionamento de Bucket

O **Amazon S3** oferece uma opção de **versionamento de código** que é uma infraestrutura de armazenamento resiliente, ou seja, ele oferece um nível adicional de proteção extra.  

### Como funciona?

Quando ativado, o **versionamento** mantém **'todas as versões'** de um objeto no mesmo bucket.  
Isso permite recuperar arquivos **excluídos por engano**, ou restaurar versões anteriores.  

Por exemplo:  
- Você envia a **versão 1** de um arquivo.  
- Depois envia a **versão 2** — o S3 mantém **as duas versões**.  
- Se excluir a versão 2, ainda pode recuperar a versão 1.  
- E mesmo que exclua a versão 1, o S3 **não remove o arquivo de imediato** — ele coloca um **marcador de exclusão**.  

<img width="581" height="257" alt="image" src="https://github.com/user-attachments/assets/4da50926-4176-487c-85e5-447046417d86" />

> Assim, o versionamento pode ser usado para **preservar, recuperar e restaurar** arquivos, prevenindo perdas em casos de **'erros humanos'** ou falhas na aplicação.  

### Como ativar o versionamento?  

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

Pronto, o arquivo estará de volta:  

<img width="1866" height="307" alt="image" src="https://github.com/user-attachments/assets/100da4e1-25bc-4e9f-8cd7-431017edcba0" />

[⬆ Voltar ao índice](#índice)

---

## Modelo de consistência de dados no Amazon S3

- **Consistência imediata** para operações **PUT de novos objetos**.  

<img width="435" height="178" alt="Consistência de leitura" src="https://github.com/user-attachments/assets/e066f4c9-8f30-41a9-bc64-4629b9ea6a23" />

- **Consistência eventual** para **substituição ou deleção de arquivos**.  
  Pode ocorrer de retornar uma versão antiga ou levar algum tempo para propagar alterações em todas as cópias.  

<img width="436" height="182" alt="Consistência eventual" src="https://github.com/user-attachments/assets/81099ada-62c7-49fe-8255-c8431d66e0fa" />  

[⬆ Voltar ao índice](#índice)

---

## Informações adicionais relevantes do Amazon S3

### Replicação entre regiões (Cross-Region Replication - CRR)
- Duplica automaticamente objetos em **outra região AWS**.  
- Benefícios:
  - Maior **resiliência e disponibilidade**  
  - Compliance com leis de proteção de dados  
  - Recuperação de desastres (DR)  
- Requisitos:
  - Versionamento habilitado no bucket de origem e destino  
  - Permissões adequadas para replicação  

### Versionamento + MFA Delete
- **Versionamento** mantém todas as versões de um objeto no bucket.  
- **MFA Delete** exige **autenticação multifator** para excluir objetos ou versões antigas, aumentando a segurança.

### Boas práticas de segurança
- Bloquear **acesso público** por padrão.  
- Habilitar **criptografia padrão**:
  - SSE-S3 → gerenciada pelo S3  
  - SSE-KMS → gerenciada pelo KMS  
- Revisar **políticas de bucket** regularmente.  
- Usar **IAM Roles** para conceder acesso mínimo necessário.

### Integração com outros serviços AWS
- **AWS Lambda** → processar arquivos automaticamente  
- **Amazon Athena** → consultas SQL sobre objetos no S3  
- **Amazon CloudFront** → CDN para distribuição global de arquivos  
- **AWS Backup** → políticas centralizadas de backup  

### Notas sobre desempenho
- Evite prefixos sequenciais nos nomes dos objetos para otimizar I/O.  
- Para uploads grandes, usar **Multipart Upload** ou **S3

[⬆ Voltar ao índice](#índice)

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
