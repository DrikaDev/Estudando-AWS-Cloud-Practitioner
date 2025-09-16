## 🧪 Lab - Introdução ao Amazon DynamoDB

Neste laboratório vamos criar uma tabela no DynamoDB para armazenar informações sobre uma biblioteca de música.  
Em seguida, vamos consultar a biblioteca de música e, por último, excluir a tabela do DynamoDB.  

---

## Tarefa 1: Criar a tabela 

Criar uma tabela no DynamoDB chamada Music, definindo uma chave de partição (obrigatória) e, opcionalmente, uma chave de classificação.  
Juntas, essas chaves identificam de forma única cada item na tabela.

No Console de Gerenciamento da AWS, escolha DynamoDB.
Selecione Create table.

Em **Table name**, insira 'Music'  
Em **Partition key**, insira 'Artist' e deixe String selecionada na lista suspensa.  
Em **Sort key - optional**, insira 'Song' e deixe String selecionada.  

<img width="1383" height="473" alt="image" src="https://github.com/user-attachments/assets/eaca2c17-809c-4229-a82c-792bf80453f3" />

A tabela usará a configuração padrão para índices e capacidade provisionada.  

<img width="1381" height="151" alt="image" src="https://github.com/user-attachments/assets/09de6c56-4607-4904-9f80-f44227f6f69e" />

Role a tela para baixo e selecione **Create alarm**.  

A tabela será criada em menos de um minuto.  
Aguarde até que a tabela 'Music' esteja 'Active' antes de prosseguir para a próxima tarefa.  

<img width="1118" height="247" alt="image" src="https://github.com/user-attachments/assets/42348e75-45ef-4c2e-8dea-6c1b08544b1a" />

---

## Tarefa 2: Adicionar dados na tabela

A tabela armazena itens (como linhas em outros bancos).
Cada item é único e formado por atributos (como colunas).
Exemplo: na tabela Music, um item pode ter os atributos song e artist.
O DynamoDB não exige esquema fixo: só é obrigatória a chave de partição (e de classificação, se houver).
Cada item pode ter atributos diferentes, e não há limite de quantidade de itens.

Escolha a tabela Music.  
Selecione Actions e, depois, Create item.  

<img width="1401" height="187" alt="image" src="https://github.com/user-attachments/assets/c4ad3f54-1e32-47fc-9f8b-26f198212753" />

Para o valor **Artist**, insira 'Pink Floyd'  
Para o valor **Song**, insira Money'  

Para criar um atributo adicional, selecione Add new attribute.
Na lista suspensa, selecione String.  

<img width="1364" height="385" alt="image" src="https://github.com/user-attachments/assets/4bef7063-bae6-48ea-b38f-c46fd8da18f0" />

Uma nova linha de atributo será adicionada.  
Para o novo atributo, insira:  
- Attribute name: Album  
- Empty value: The Dark Side of the Moon  

Adicione outro atributo, selecione Add new attribute.  
Na lista suspensa, escolha Number.  
Um novo atributo de número será adicionado.  
Para o novo atributo, insira:  
- Attribute name: Year  
- Empty value: 1973  
Selecione Create Item.

<img width="1286" height="363" alt="image" src="https://github.com/user-attachments/assets/0f61e5d5-3558-4eac-bf3b-ca4532f3fb8f" />

O item foi adicionado à tabela Music.  

<img width="789" height="281" alt="image" src="https://github.com/user-attachments/assets/fcfefac6-d6f3-47de-9b7b-12b8714e8357" />

Para criar um segundo item, use os seguintes atributos:  

<img width="847" height="216" alt="image" src="https://github.com/user-attachments/assets/d5326ee2-f9d6-42e2-827a-a1172db61812" />

Este item tem um atributo adicional chamado Genre (Gênero). Esse é um exemplo de como cada item pode ter atributos diferentes sem que seja preciso predefinir um esquema de tabela.  

Para criar um terceiro item, use os seguintes atributos:  

<img width="847" height="212" alt="image" src="https://github.com/user-attachments/assets/46486aba-0e95-4ab4-8664-5afc4afb4400" />

Este item tem um novo atributo LengthSeconds (Duração em segundos) identificando o quanto dura a música.  
Isso demonstra a flexibilidade de um banco de dados NoSQL.  

<img width="1284" height="415" alt="image" src="https://github.com/user-attachments/assets/bc48b8c8-14dd-411c-aec3-e0978bf76adb" />

---

## Tarefa 3: Modificar um item existente  

Você percebeu que há um erro nos dados. Precisamos modificar um item existente.  
No painel do DynamoDB, em Tables, escolha Explore Items.  

<img width="1397" height="187" alt="image" src="https://github.com/user-attachments/assets/abe6dcd5-5707-4381-bda4-617eac6b3b37" />

Escolha o botão Music.  

<img width="275" height="53" alt="image" src="https://github.com/user-attachments/assets/8aa22080-c144-4f19-8952-d30167ffa721" />

Selecione Psy.  
Selecione Actions, Edit item.  

<img width="789" height="204" alt="image" src="https://github.com/user-attachments/assets/b588809a-7831-43b7-90a0-eb360f0ccbf8" />

Altere Year de 2011 para 2012.  
Clique em Save and Close.  
O item agora está atualizado.  

---

## Tarefa 4: Consultar a tabela

Há duas maneiras de consultar uma tabela do DynamoDB: consulta e verificação.  

Uma operação de consulta localiza itens com base na chave primária e, opcionalmente, com base na chave de classificação.  
Isso fica totalmente indexado, por isso, é executado de modo bem rápido.  

Expanda **Scan or query items** e escolha **Query**.  

<img width="789" height="115" alt="image" src="https://github.com/user-attachments/assets/d9ebc9dd-a239-482d-b1f5-ad83cbda11f8" />

Serão exibidos campos: 
- **Partition key: Artist**: coloque 'Psy'  
- **Sort key: Song**: coloque 'Gangnam Style'
Clique em Run.

<img width="784" height="313" alt="image" src="https://github.com/user-attachments/assets/06b13b13-1707-4ece-944f-5bc6c40db8b5" />

A música aparece rapidamente na lista.  

<img width="787" height="287" alt="image" src="https://github.com/user-attachments/assets/e5c3f87d-daa6-4648-9b28-6ca65a015bfa" />

Role para cima e selecione **Scan**.  
Expanda **Filters** e insira os seguintes valores:  
- Em **Attribute name**, insira 'Year'  
- Em **Type**, selecione 'Number'  
- Em **Value**, insira '1971'  
Clique em Run  

<img width="785" height="448" alt="image" src="https://github.com/user-attachments/assets/d8083ca3-4538-40da-ba63-aaa4c5f6b814" />

Apenas a música lançada em 1971 é exibida.  

<img width="789" height="290" alt="image" src="https://github.com/user-attachments/assets/7a6c0aba-9565-48e3-a0cc-6224945f5187" />

---

## Tarefa 5: Excluir a tabela  

Nesta tarefa, vamos excluir a tabela **Music**. Todos os dados da tabela também serão excluídos.  
No painel do DynamoDB, em **Tables**, selecione a tabela Music.  
Selecione Delete.  

<img width="1412" height="593" alt="image" src="https://github.com/user-attachments/assets/6288147c-bb59-4c05-bba3-af20dc1f42bf" />

No painel de confirmação, insira "confirm".  

<img width="831" height="133" alt="image" src="https://github.com/user-attachments/assets/f181246d-5529-4b2e-a3d7-59e4d374b90f" />

A tabela será excluída.  

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
