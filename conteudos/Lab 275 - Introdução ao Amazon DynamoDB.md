## 🧪 Lab 275 - Introdução ao Amazon DynamoDB

### Índice

- [0. Introdução](#0-introdução)
- [1. Criar uma tabela](#1-criar-uma-tabela)
- [2. Adicionar dados na tabela](#2-adicionar-dados-na-tabela)
- [3. Modificar um item existente](#3-modificar-um-item-existente)
- [4. Consultar a tabela](#4-consultar-a-tabela)
- [5. Excluir a tabela](#5-excluir-a-tabela)
- [6. Conclusão](#6-conclusão)

---

### 0. Introdução

O **Amazon DynamoDB** é um serviço de banco de dados **NoSQL** rápido e flexível para aplicativos que precisam de **latência consistente de milissegundos de dígito único em qualquer escala**.  

Ele é um banco de dados totalmente gerenciado, compatível com os modelos de dados **documento** e **chave-valor**.  

Neste laboratório vamos criar uma tabela no DynamoDB para armazenar informações sobre uma **biblioteca de música**.  
Em seguida, vamos **consultar** a biblioteca de música e, por último, **excluir** a tabela do DynamoDB.  

---

### 1. Criar uma tabela 

Nesta tarefa, vamos criar uma nova tabela no **Amazon DynamoDB** chamada `Music`.  
Cada tabela requer uma **chave de partição**, que é usada para particionar dados entre os servidores do DynamoDB.  
Opcionalmente, uma tabela também pode ter uma **chave de classificação**.  
A combinação de **chave de partição** e **chave de classificação** identifica exclusivamente cada item em uma tabela do DynamoDB.  

1. No **Console de Gerenciamento da AWS**, escolha o menu **Services (Serviços)**.  

2. Em **Database (Banco de dados)**, selecione **DynamoDB**.  

3. Clique em **Create table (Criar tabela)**.  

4. Configure os seguintes campos:  
   - **Table name (Nome da tabela):** `Music`  
   - **Partition key (Chave de partição):** `Artist` (mantenha `String` selecionado)  
   - **Sort key - optional (Chave de classificação – opcional):** `Song` (mantenha `String` selecionado)  

<img width="1383" height="473" alt="image" src="https://github.com/user-attachments/assets/eaca2c17-809c-4229-a82c-792bf80453f3" />

5. A tabela usará a **configuração padrão** para índices e capacidade provisionada.  

<img width="1381" height="151" alt="image" src="https://github.com/user-attachments/assets/09de6c56-4607-4904-9f80-f44227f6f69e" />

6. Role a tela para baixo e selecione **Create table (Criar tabela)**.  
A tabela será criada em menos de um minuto.  
Aguarde até que a tabela 'Music' esteja 'Active' antes de prosseguir para a próxima tarefa.  

<img width="1118" height="247" alt="image" src="https://github.com/user-attachments/assets/42348e75-45ef-4c2e-8dea-6c1b08544b1a" />

---

### 2. Adicionar dados na tabela

Nesta tarefa, vamos adicionar dados à tabela **Music**.  

Uma tabela é uma coleção de dados sobre um tópico específico.  
Cada tabela contém vários **itens**, que são grupos de atributos identificáveis de forma única entre todos os outros itens.  
No DynamoDB, os itens são semelhantes às linhas em outros bancos de dados, e não há limite para o número de itens em uma tabela.  

Cada item é composto por um ou mais **atributos**, que são os elementos de dados fundamentais, semelhantes às colunas de outros bancos de dados.  
Por exemplo, um item na tabela **Music** contém atributos como `song` e `artist`.  

Ao gravar um item, apenas as **chaves de partição e de classificação** são obrigatórias.  
O DynamoDB permite adicionar atributos adicionais sem que seja necessário definir previamente um esquema, oferecendo flexibilidade para cada item ter diferentes atributos.  

1. Escolha a tabela **Music**.  

2. Selecione **Actions (Ações)** → **Create item (Criar item)**.  

<img width="1401" height="187" alt="image" src="https://github.com/user-attachments/assets/c4ad3f54-1e32-47fc-9f8b-26f198212753" />

3. Preencha os seguintes atributos:  
   - **Artist:** `Pink Floyd`  
   - **Song:** `Money`  

4. Para criar um atributo adicional, selecione **Add new attribute**.  
- Na lista suspensa, selecione **String**.  

<img width="1364" height="385" alt="image" src="https://github.com/user-attachments/assets/4bef7063-bae6-48ea-b38f-c46fd8da18f0" />

Uma nova linha de atributo será adicionada.  

5. Para o novo atributo, insira:  
- **Attribute name**: `Album`  
- **Empty value**: `The Dark Side of the Moon`  

6. Adicione outro atributo, selecione **Add new attribute**.  
Na lista suspensa, escolha **Number**.  
Um novo atributo de número será adicionado.  

Para o novo atributo, insira:  
- **Attribute name**: `Year`  
- **Empty value**: `1973`  
- Selecione **Create Item**.  

<img width="1286" height="363" alt="image" src="https://github.com/user-attachments/assets/0f61e5d5-3558-4eac-bf3b-ca4532f3fb8f" />

O item foi adicionado à tabela Music.  

<img width="789" height="281" alt="image" src="https://github.com/user-attachments/assets/fcfefac6-d6f3-47de-9b7b-12b8714e8357" />

7. Para criar um segundo item, use os seguintes atributos:  

<img width="847" height="216" alt="image" src="https://github.com/user-attachments/assets/d5326ee2-f9d6-42e2-827a-a1172db61812" />

> Este item tem um atributo adicional chamado **Genre** (Gênero).  
> Esse é um exemplo de como cada item pode ter atributos diferentes sem que seja preciso predefinir um esquema de tabela.  

8. Para criar um terceiro item, use os seguintes atributos:  

<img width="847" height="212" alt="image" src="https://github.com/user-attachments/assets/46486aba-0e95-4ab4-8664-5afc4afb4400" />

> Este item tem um novo atributo chamado **LengthSeconds** (Duração em segundos) identificando o quanto dura a música.  
> Isso demonstra a flexibilidade de um banco de dados NoSQL.  

<img width="1284" height="415" alt="image" src="https://github.com/user-attachments/assets/bc48b8c8-14dd-411c-aec3-e0978bf76adb" />

---

### 3. Modificar um item existente  

Nesta tarefa, vamos corrigir um item existente na tabela **Music**.  

1. No painel do **DynamoDB**, vá em **Tables (Tabelas)** → **Explore Items (Explorar itens)**.  

<img width="1397" height="187" alt="image" src="https://github.com/user-attachments/assets/abe6dcd5-5707-4381-bda4-617eac6b3b37" />

2. Selecione o botão **Music**.  

<img width="275" height="53" alt="image" src="https://github.com/user-attachments/assets/8aa22080-c144-4f19-8952-d30167ffa721" />

3. Clique no item **Psy**.  
Selecione Actions, Edit item.  

<img width="789" height="204" alt="image" src="https://github.com/user-attachments/assets/b588809a-7831-43b7-90a0-eb360f0ccbf8" />

4. Altere o atributo **Year (Ano)** de `2011` para `2012`.  

5. Clique em **Save Changes (Salvar alterações)**.  

> O item agora está atualizado com a informação correta.

---

### 4. Consultar a tabela

Há duas maneiras de consultar uma tabela do **DynamoDB**: **consulta (Query)** e **verificação (Scan)**.  

- **Consulta (Query):** localiza itens com base na **chave primária** e, opcionalmente, na **chave de classificação**.  
  - Totalmente indexado, portanto é muito rápido.  
- **Verificação (Scan):** examina todos os itens da tabela, sendo menos eficiente e mais demorado em tabelas grandes.  

**1. Consulta por chave primária:**  

- Expanda **Scan/Query items (Verificar/Consultar itens)** e selecione **Query (Consultar)**.  

<img width="789" height="115" alt="image" src="https://github.com/user-attachments/assets/d9ebc9dd-a239-482d-b1f5-ad83cbda11f8" />

- Serão exibidos campos: 
  - **Partition key: Artist**: coloque 'Psy'  
  - **Sort key: Song**: coloque 'Gangnam Style'
- Clique em **Run**.  

<img width="784" height="313" alt="image" src="https://github.com/user-attachments/assets/06b13b13-1707-4ece-944f-5bc6c40db8b5" />

> A música aparecerá rapidamente na lista (pode ser necessário rolar para baixo).  

<img width="787" height="287" alt="image" src="https://github.com/user-attachments/assets/e5c3f87d-daa6-4648-9b28-6ca65a015bfa" />

> A **consulta** é a forma mais eficiente de recuperar dados de uma tabela do DynamoDB.  

**2. Verificação por filtro:**  

- Role para cima e selecione **Scan**.  
- Expanda **Filters** e insira os seguintes valores:  
  - Em **Attribute name**, insira 'Year'  
  - Em **Type**, selecione 'Number'  
  - Em **Value**, insira '1971'  
-Clique em Run  

<img width="785" height="448" alt="image" src="https://github.com/user-attachments/assets/d8083ca3-4538-40da-ba63-aaa4c5f6b814" />

> Apenas a música lançada em 1971 é exibida.  

<img width="789" height="290" alt="image" src="https://github.com/user-attachments/assets/7a6c0aba-9565-48e3-a0cc-6224945f5187" />

---

### 5. Excluir a tabela  

Nesta tarefa, vamos excluir a tabela **Music**. Todos os dados da tabela também serão excluídos.  

- No painel do DynamoDB, em **Tables**, selecione a tabela Music.  
- Selecione **Delete**.  

<img width="1412" height="593" alt="image" src="https://github.com/user-attachments/assets/6288147c-bb59-4c05-bba3-af20dc1f42bf" />

- No painel de confirmação, digite `confirm` e clique em **Delete (Excluir)**.  

<img width="831" height="133" alt="image" src="https://github.com/user-attachments/assets/f181246d-5529-4b2e-a3d7-59e4d374b90f" />

A tabela será excluída permanentemente do **DynamoDB**.  

---

### 6. Conclusão

Parabéns! Você concluiu com êxito as seguintes tarefas:

- ✅ Criou uma tabela no **Amazon DynamoDB**  
- ✅ Inseriu dados em uma tabela do **Amazon DynamoDB**  
- ✅ Consultou uma tabela do **Amazon DynamoDB**  
- ✅ Excluiu uma tabela do **Amazon DynamoDB**

Para aprofundar seus conhecimentos sobre o DynamoDB, consulte a **documentação oficial da AWS**.

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
