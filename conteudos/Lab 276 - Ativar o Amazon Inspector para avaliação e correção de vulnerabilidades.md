## Lab 276 - Ativar o Amazon Inspector para avaliação e correção de vulnerabilidades

Neste laboratório, vamos utilizar o **Amazon Inspector** para verificar vulnerabilidades em seus recursos da AWS, especificamente nas funções do **AWS Lambda**.  

---

### 📌 O que faz o Amazon Inspector?

- Realiza varredura automática das funções do **AWS Lambda**, respondendo rapidamente a novas implantações.  
- Verifica também **instâncias do EC2** e **repositórios do Amazon ECR** dentro da conta da AWS.  
- Fornece descobertas de vulnerabilidade que ajudam na mitigação e correção dos riscos de segurança.  

---

### Cenário

Os desenvolvedores da **UmaEmpresa** estão nas fases iniciais da criação de uma aplicação usando principalmente o **AWS Lambda**.  
Durante o processo de desenvolvimento, eles precisam de uma ferramenta de segurança automatizada que não apenas verifique pacotes de software vulneráveis, mas também o próprio código.  

Para atender a essa necessidade, você decide utilizar o **Amazon Inspector**.  

---

## 🔹 Tarefa 1: Ativar o Amazon Inspector  

Nesta tarefa, vamos ativar e executar o serviço **Amazon Inspector** para verificar continuamente as funções do **AWS Lambda**.  

1. No **Console de Gerenciamento da AWS**, na barra de pesquisa superior, digite e selecione **Inspector**.  
2. Abra o painel à esquerda e selecione **Ativar o Inspector**.  
3. Clique no botão **Ativar o Inspector**.  

> 💡 Observação: isso só é necessário na **primeira vez** que o serviço é ativado.  

Após a ativação:  
- Uma mensagem aparecerá na parte superior: **Boas-vindas ao Inspector. Sua primeira verificação está em andamento.**  
- Se for solicitado que você responda a uma pesquisa de **Feedback para o Amazon Inspector**, clique em **Cancelar**.  
- Feche todas as mensagens de banner exibidas na parte superior.  
- Atualize a página periodicamente até ver em:  
  **Painel > Resumo > Cobertura do ambiente > Funções do Lambda em 100%.**  

<img width="686" height="644" alt="image" src="https://github.com/user-attachments/assets/38391e8a-f8c9-43c4-b10f-b5688cb5f71c" />

O painel exibirá:  
- O número da sua conta.  
- O status de ativação do AWS Lambda.  
- Verificações padrão já ativadas para **Amazon EC2, Amazon ECR e AWS Lambda**.  

<img width="694" height="491" alt="image" src="https://github.com/user-attachments/assets/45d5dbe9-0d09-43c3-8e2a-6884ef6141de" />

---

## 🔹 Tarefa 2: Analisar os recursos inspecionados  

Enquanto aguarda a conclusão da verificação inicial, você analisará os recursos do ambiente (instância **EC2** e funções do **Lambda**) que estão sendo inspecionados pelo Amazon Inspector.  

### 🔸 Tarefa 2.1: Analisar suas funções do Lambda  

1. No painel à esquerda, em **Descobertas**, selecione **Todas as descobertas**.  
2. Serão exibidas **três vulnerabilidades** relacionadas à função do Lambda. Cada linha mostrará:  
   - **Gravidade**: Média.  
   - **Recurso afetado**: a função do Lambda vulnerável.  
   - **Título**: motivo da descoberta.  

<img width="1831" height="691" alt="image" src="https://github.com/user-attachments/assets/91db0f7f-d0e4-45a3-bacf-c50f6f7998bf" />

3. Selecione a descoberta `CVE-2023-32681 - requests`.  
  - Isso abrirá um painel com informações detalhadas da vulnerabilidade.  

<img width="1388" height="577" alt="image" src="https://github.com/user-attachments/assets/fe94cb8c-feb4-4f44-add7-78065b0e4656" />

4. Na seção **Detalhes da descoberta**, clique no link externo ao lado de **ID da vulnerabilidade**.  

   <img width="885" height="96" alt="image" src="https://github.com/user-attachments/assets/dcc1af8d-7909-42e4-8148-e23fc713fc32" />
  
  - Uma nova aba será aberta com a página da vulnerabilidade no **National Vulnerability Database (NVD)**, do **NIST**, contendo mais informações técnicas.

  <img width="1874" height="771" alt="image" src="https://github.com/user-attachments/assets/a07612b0-0c9c-438a-bb29-d86d3107b2b5" />

6. No painel de informações, localize a seção **Correção**.  

<img width="894" height="115" alt="image" src="https://github.com/user-attachments/assets/4440c82f-e929-45a9-b4bd-65ec6c07de0b" />

➡️ O problema identificado: o pacote **requests** está vulnerável e desatualizado.  
➡️ Recomendação: **atualizar o pacote**.  

---

## 🔹 Tarefa 3: Corrigir as descobertas de vulnerabilidades  

Agora, você interpretará os detalhes da vulnerabilidade e aplicará a correção diretamente na função do **AWS Lambda**.  

### 🔸 Tarefa 3.1: Corrigir as vulnerabilidades do pacote da função do Lambda  

1. No Console da AWS, pesquise por **Lambda** e selecione o serviço.  

2. Na lista de funções, clique na função **get-request**.  

<img width="1887" height="361" alt="image" src="https://github.com/user-attachments/assets/3623bef0-13e1-4436-a06c-fa42629e17ac" />

3. Role para baixo. No navegador de arquivos do editor de código, abra o arquivo **requirements.txt**.  

<img width="1872" height="462" alt="image" src="https://github.com/user-attachments/assets/0f984ea5-449b-4e4c-8ddc-0b7ea8576f34" />

4. Edite a linha:  ` requests==2.20.0 `, para ` requests`

<img width="1847" height="299" alt="image" src="https://github.com/user-attachments/assets/0895763f-9bdc-4209-9e7d-95361d1e8698" />

> Isso garante que a versão mais recente do pacote `requests` seja instalada.

5. Clique em **Deploy**.

- Um banner confirmará: Successfully updated the function get-request.
- Essa nova implantação acionará automaticamente uma nova verificação do Amazon Inspector.

<img width="1864" height="93" alt="image" src="https://github.com/user-attachments/assets/8d42af0b-888a-4470-b27d-aa763de71a13" />

6. Volte ao Amazon Inspector:

- No painel de navegação à esquerda, selecione Resultados > Todas as descobertas.  
- Altere o filtro **Declaração de resultados** de `Ativo` para `Fechado`.  
- Verifique que a vulnerabilidade `CVE-2023-32681 - requests` aparece na lista fechada.  

<img width="1465" height="660" alt="image" src="https://github.com/user-attachments/assets/596f1b9c-9418-4043-a906-543a645506d8" />

> 🔄 Observação: pode levar alguns minutos para que a verificação seja concluída. Clique em Atualizar para ver o status mais recente.

7. No painel de navegação à esquerda, em **Cobertura de recursos**, selecione **Funções do Lambda**.

Expanda a coluna **Última verificação** para visualizar o carimbo de data/hora atualizado.  

<img width="1791" height="510" alt="image" src="https://github.com/user-attachments/assets/fc2e1760-2f9a-49b9-8ca9-61d7c28adbaa" />

---

✅ Conclusão

Parabéns! 🎉 Você concluiu com sucesso o laboratório:
- Ativação e configuração do Amazon Inspector.
- Análise e interpretação das descobertas de vulnerabilidade.
- Correção das vulnerabilidades encontradas nas funções do AWS Lambda.

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
