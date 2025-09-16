## 🧪 Lab 173 - Solucionar problemas para criar uma instância do EC2

Nesta atividade, vamos usar a **AWS Command Line Interface (AWS CLI)** para iniciar instâncias do **Amazon Elastic Compute Cloud 
(Amazon EC2)**.  

## Índice

1. [Objetivos](#objetivos)
2. [Funcionamento](#funcionamento)
3. [Arquitetura](#arquitetura)
4. [Tarefa 1: Conectar-se à instância CLI Host](#tarefa-1-conectar-se-à-instância-cli-host)
5. [Tarefa 2: Configurar a AWS CLI](#tarefa-2-configurar-a-aws-cli)
6. [Tarefa 3: Criar uma instância do EC2 usando a AWS CLI](#tarefa-3-criar-uma-instância-do-ec2-usando-a-aws-cli)
   - [Tarefa 3.1: Observar os detalhes do script](#tarefa-31-observar-os-detalhes-do-script)
   - [Tarefa 3.2: Tentar executar o script](#tarefa-32-tentar-executar-o-script)
   - [Tarefa 3.3: Solução de problemas](#tarefa-33-solução-de-problemas)
       - [Problema #1](#problema-1)
       - [Problema #2](#problema-2)
7. [Tarefa 4: Verificar a funcionalidade do site](#tarefa-4-verificar-a-funcionalidade-do-site)

---

### Objetivos

- Configurar a instância usando um **script de dados do usuário**.  
- Garantir execução de: **Apache**, **MariaDB** e **PHP**.  
- Iniciar uma instância **Amazon EC2** via **AWS CLI**.  
- Solucionar problemas usando dicas e o utilitário **nmap**.

Esses softwares juntos formam a **pilha LAMP**: **Linux, Apache, MySQL/MariaDB e PHP**, uma forma prática de criar um site com back-
end de banco de dados em uma única máquina.

---

### Funcionamento

O **script de dados do usuário** realizará:  

- Implantação dos **arquivos do site**.  
- Execução dos **scripts de configuração do banco de dados**.  

O resultado será uma instância que **hospeda o aplicativo web de uma cafeteria**.

---

### Arquitetura

O diagrama a seguir mostra a arquitetura do aplicativo web que vamos criar nesta atividade:  

<img width="3136" height="1536" alt="image" src="https://github.com/user-attachments/assets/2f43fefa-c9b3-4927-b150-3346e41ba7c8" />
 
---

## Tarefa 1: Conectar-se à instância CLI Host

Nesta tarefa, vamos usar o **EC2 Instance Connect** para conectar-se à instância do **EC2 CLI Host** que foi criada quando o 
laboratório foi provisionado.  
Vamos usar essa instância para executar comandos da **AWS CLI**.  

### Passo a passo

1. No **Console de Gerenciamento da AWS**, na barra **Pesquisar**, digite e escolha **EC2**.  
2. No painel de navegação, selecione **Instâncias**.  
3. Na lista de instâncias, selecione a instância **CLI Host**.  
4. Acima da lista de instâncias, selecione **Conectar-se**.  

<img width="1418" height="213" alt="image" src="https://github.com/user-attachments/assets/fe7a7f88-c4af-4477-957a-1fa442e951f9" />

5. Na guia **EC2 Instance Connect**, clique em **Conectar-se**.  

<img width="1408" height="180" alt="image" src="https://github.com/user-attachments/assets/2a5d058b-45b4-485e-851e-f32b16c2dc4e" />

Agora que a conexão com a instância **CLI Host** foi estabelecida, você poderá configurar e usar a **AWS CLI** para chamar os serviços 
da AWS.

[⬆ Voltar ao índice](#índice)

---

## Tarefa 2: Configurar a AWS CLI

Nesta tarefa, vamos configurar a **AWS CLI**, fornecendo os parâmetros de configuração disponíveis quando o laboratório foi provisionado.  
Após a configuração, será possível executar os comandos da CLI para interagir com os serviços da **AWS**.  

> 💡 **Observação:** As imagens de máquina da Amazon (**AMIs**) do **Amazon Linux** já vêm com a AWS CLI pré-instalada.

Para configurar o perfil da AWS CLI com credenciais, execute o seguinte comando no terminal: `aws configure`  

Quando solicitado, insira as seguintes informações:  

- **ID de chave de acesso da AWS**: No início destas instruções, selecione **Detalhes** e escolha **Mostrar**.
  Copie o valor **AccessKey** e cole na janela do terminal.  
- **AWS Secret Access Key (Chave de acesso secreta da AWS)**: Copie e cole o valor **SecretKey** na janela do terminal.  
- **Default region name (Nome da região padrão)**: Copie o valor **LabRegion** das instruções do laboratório e cole na janela do terminal.  
- **Default output format (Formato de saída padrão)**: Insira `json`.

<img width="903" height="314" alt="image" src="https://github.com/user-attachments/assets/e233eba3-854d-44ef-ad8c-63ed48ab1181" />

[⬆ Voltar ao índice](#índice)

---

## Tarefa 3: Criar uma instância do EC2 usando a AWS CLI

Nesta tarefa, vamos **observar e executar um script de shell** fornecido para criar uma instância **LAMP** do EC2 usando comandos 
da **AWS CLI**.  

O script contém **problemas intencionais**.  
Seu desafio é:

1. **Encontrar os problemas** no script.  
2. **Corrigi-los** para que o script funcione corretamente.  
3. **Executar o script novamente** para verificar se os problemas foram resolvidos.  

---

### Tarefa 3.1: Observar os detalhes do script

Primeiro, crie um **backup do script** que você editará em uma etapa posterior.  

Para mudar para o diretório onde se encontra o arquivo de script e criar um backup, execute os seguintes comandos:  

```
cd ~/sysops-activity-files/starters
cp create-lamp-instance-v2.sh create-lamp-instance.backup
```

** É prática recomendada fazer backup dos arquivos antes de modificá-los.**  
<img width="809" height="69" alt="image" src="https://github.com/user-attachments/assets/09ab3ffe-cf6b-4bb0-bac3-8015792b9db0" />

Abra o arquivo de script **create-lamp-instance-v2.sh** em um editor de texto de linha de comando, como o **VI**.
Use o comando: `view create-lamp-instance-v2.sh`

### Analisando o conteúdo do script:

> 💡 **Dica:**  
> Se você estiver usando o VI, poderá exibir os números de linha digitando `:set number`:  
> Pressione a tecla Esc para garantir que você está no modo normal do VI.  
> Digite ':' (dois pontos) para entrar no modo de comando, depois digite 'set number' e pressione Enter.  

- **Linha 1:** este é um arquivo bash, portanto, a primeira linha contém `#!/bin/bash`.  

<img width="1420" height="101" alt="image" src="https://github.com/user-attachments/assets/5ad90f71-3170-4238-9ed8-a8d040a3d935" />

- **Linhas 7 a 11:** o tamanho da instância é definido como `t3.small`, suficiente para executar o banco de dados e o servidor web.  

<img width="1422" height="84" alt="image" src="https://github.com/user-attachments/assets/286e51ef-9891-438d-b403-60839833571f" />

- **Linhas 16 a 29:** o script invoca o comando `describe-regions` da AWS CLI para listar todas as regiões AWS.
  Em cada região, ele consulta uma VPC existente denominada **VPC da cafeteria**, captura o ID da VPC e a região e interrompe o
  loop `while`.
  Esta é a VPC onde a instância LAMP será implantada.  

<img width="1424" height="236" alt="image" src="https://github.com/user-attachments/assets/998c81c1-e137-4605-b507-62fbc2ae5c80" />

- **Linhas 31 a 55:** o script pesquisa os valores **ID da sub-rede**, **Nome do par de chaves** e **ID da AMI** necessários para criar
  a instância EC2.  
  - Observação: a linha 32 termina com uma barra invertida (`\`), usada para continuar um comando em outra linha, facilitando a leitura.  

<img width="1423" height="422" alt="image" src="https://github.com/user-attachments/assets/031d999b-1b11-4b05-9f3e-998f82c5cbf1" />

- **Linhas 57 a 122:** o script limpa a conta AWS caso já tenha sido executado antes.
  Ele verifica se há uma instância chamada `cafeserver` e se existe um grupo de segurança com `cafeSG` no nome.
  Se algum recurso for encontrado, o script solicitará que você os exclua.  

<img width="1421" height="526" alt="image" src="https://github.com/user-attachments/assets/8ffde6c5-8fa5-402d-b4c2-7c2b9ea4d1d4" />
<img width="1426" height="527" alt="image" src="https://github.com/user-attachments/assets/0e7b59d5-cdfd-4465-8020-c84743425046" />
<img width="1423" height="47" alt="image" src="https://github.com/user-attachments/assets/b8127c3a-476a-44e3-aad3-024b6d3e7abc" />

- **Linhas 124 a 152:** o script cria um **grupo de segurança** com portas 22 e 80 abertas.  

<img width="1423" height="491" alt="image" src="https://github.com/user-attachments/assets/9f40863f-920a-4551-9864-35b5bcaa126e" />

- **Linhas 154 a 168:** o script cria a **instância EC2**, usando os valores definidos nas linhas 8 e 10 e os valores coletados nas
  linhas 16 a 57.  
  - Há referência ao arquivo de **dados do usuário**, que será revisado em outra etapa.  
  - A chamada para criar a instância é capturada na variável `instanceDetails`, exibida no terminal e formatada com **Python JSON**
    para melhor visualização.

<img width="1422" height="255" alt="image" src="https://github.com/user-attachments/assets/9efaecc9-3296-4d7d-ba31-e2cd2977cd44" />

- **Linhas 179 a 188:** o valor `instanceId` é analisado a partir de `instanceDetails`.
  Um loop `while` verifica a cada 10 segundos se um **endereço IP público** foi atribuído à instância.
  Quando concluído, o endereço IP é exibido no terminal.  

<img width="1422" height="166" alt="image" src="https://github.com/user-attachments/assets/791039eb-e057-4e35-9cb7-377dd3944b10" />

Saia do editor de texto digitando `:q!`
<img width="111" height="36" alt="image" src="https://github.com/user-attachments/assets/7b626f66-1532-4e45-8b21-dabf58462351" />

Para exibir o conteúdo do **script de dados do usuário**, execute o seguinte comando: `cat create-lamp-instance-userdata-v2.txt`  

> Observe que o **script de dados do usuário** executa uma série de comandos na instância após a inicialização, instalando:  
> - **Servidor web Apache**  
> - **PHP**  
> - **Servidor de banco de dados MariaDB**

<img width="1425" height="192" alt="image" src="https://github.com/user-attachments/assets/2bd07666-5563-405e-ae83-81d8a3710a5c" />

[⬆ Voltar ao índice](#índice)

---

### Tarefa 3.2: Tentar executar o script

Agora que você já analisou o que o **script de shell** se destina a fazer, tente executá-lo: `./create-lamp-instance-v2.sh` 

⚠️ Haverá uma falha no script e ele será encerrado sem ser concluído.  
Esse comportamento é esperado, pois o script contém **problemas intencionais** para que você os identifique e corrija.  

<img width="1424" height="441" alt="image" src="https://github.com/user-attachments/assets/dddb14f0-a538-4517-88b5-3b59ca87a0ca" />

[⬆ Voltar ao índice](#índice)

---

## Tarefa 3.3: Solução de problemas

### Problema #1

A saída do terminal exibe a seguinte mensagem de erro:  
“Ocorreu um erro (InvalidAMIID.NotFound) ao chamar a operação RunInstances: o ID da imagem ‘[ami-xxxxxxxxxx”]’ não existe”.  
Isso significa que o ID da AMI que o script está tentando usar não é válido na região em que você está tentando criar a instância.  
Esse é um erro bem comum quando se trabalha com AWS CLI.  

<img width="1425" height="37" alt="image" src="https://github.com/user-attachments/assets/83d042ab-f4e1-48f7-aec5-0dc4d0feff28" />

#### Dicas para resolver o Problema #1:  

Aqui está como resolver:

<img width="701" height="120" alt="image" src="https://github.com/user-attachments/assets/9edb7deb-d667-4f42-addf-f1c15b3a6c62" />

- Localize a **linha no script** que gerou esse erro. Algo parece incorreto no script?  
- O **valor da Região** usado para o comando `run-instances` está correto?  

Após identificar o problema, **atualize o script** para corrigi-lo e execute-o novamente.  
> ⚠️ Se for solicitado que você exclua instâncias ou grupos de segurança criados na execução anterior do script,
> sempre responda com `Y`.  

- O erro foi resolvido?  
- Depois de corrigir o problema, o comando `run-instances` deve ser executado com êxito e um **endereço IPv4 público** será atribuído à
  nova instância.  

#### Testando a instância:

Tente se conectar à página da web.  
- Em um navegador, acesse o endereço: `http://35.85.61.87`
> Substitua `<public-ip>` pelo **endereço IPv4 público** da instância criada.  
> ⚠️ Se houver falha na tentativa, será necessário resolver o **Problema #2**.

<img width="1197" height="390" alt="image" src="https://github.com/user-attachments/assets/c655f2db-926f-4cf5-b9eb-2602debe7af3" />

[⬆ Voltar ao índice](#índice)

---

### Problema #2

O comando `run-instances` foi executado com êxito e um endereço IP público foi atribuído à nova instância.  
No entanto, **não é possível carregar a página da web de teste**.

Conecte-se à nova instância LAMP usando o **EC2 Instance Connect**, que é o mesmo método usado para acessar a instância CLI Host.

#### Dicas para resolver o problema #2

1. **Verifique a porta do servidor web**
   - O servidor web é executado na **porta TCP 80**.  
   - Certifique-se de que a porta está aberta.

2. **Verifique se o serviço do servidor web está funcionando**
   - O serviço do servidor web é o **httpd**.

3. **Instale e use o nmap para verificar portas**
   - No terminal da instância CLI Host, execute: `sudo yum install -y nmap`
   - Substitua <public-ip> pelo endereço IPv4 público da instância LAMP: `nmap -Pn <public-ip>`
   - Verifique se a porta 80 está aberta para acessar o servidor web.

#### Testando o script de dados do usuário

1. **Verifique se o script de dados do usuário foi executado corretamente**.  

2. **Acesse a página web da instância**  
   - No navegador, substitua `<public-ip>` pelo endereço IPv4 público da instância LAMP: `http://<public-ip>`
   - Se o **Problema #2** foi resolvido, você verá a mensagem:  **“Saudações do seu servidor web”**.

3. **Confira o arquivo de log do cloud-init**  
- Ele mostra se o script de dados do usuário (`user-data`) foi executado conforme o esperado.  
- No terminal da instância LAMP, execute: `sudo tail -f /var/log/cloud-init-output.log`
O comando exibirá as entradas do log à medida que são gravadas.
Você deve ver mensagens relacionadas à instalação do MariaDB, PHP e arquivos do aplicativo web.

#### Observando a execução do `user-data` com cloud-init

Em uma instância do **Amazon Linux**, o serviço **cloud-init** executa os comandos presentes no arquivo de dados do usuário 
(`user-data`).

1. **Verifique as entradas do arquivo de log**
   - Observe as mensagens relacionadas à instalação do **MariaDB** e do **PHP**.  
   - Não deve haver mensagens de erro.

2. **Verifique as mensagens do aplicativo web**
   - Você também verá mensagens relacionadas aos arquivos do aplicativo web da cafeteria que foram baixados e extraídos para a
     instância. Exemplo de mensagem esperada:  `Criação de script de banco de dados concluída`

3. **Finalizando a visualização do log**
   - Para sair do utilitário `tail -f`, pressione `Ctrl-C`.

4. **Visualização completa do log**
   - Para ver todo o conteúdo do arquivo de log, execute: `sudo cat /var/log/cloud-init-output.log`

[⬆ Voltar ao índice](#índice)

---

## Tarefa 4: Verificar a funcionalidade do site

1. **Acesse o site da cafeteria**  
   - No navegador, substitua `<public-ip>` pelo endereço IPv4 público da instância criada: `http://<public-ip>/cafe`  
   - Se a implantação foi bem-sucedida, você verá a **página inicial do site da cafeteria**.  
   ✅ Parabéns!

2. **Verifique a funcionalidade de pedidos**
   - Clique no link **Menu**.  
     - Uma nova página será carregada em: `http://<public-ip>/cafe/menu.php`
   - Selecione algumas sobremesas e clique em **Enviar pedido**.  
   - A página **Confirmação do pedido** exibirá os detalhes dos itens selecionados.

3. **Faça outro pedido**
   - Escolha itens diferentes e envie o pedido.  
   - Depois, selecione a página **Histórico de pedidos**.  
   - Os detalhes de ambos os pedidos devem estar capturados corretamente.

> **Observação:** os detalhes dos pedidos são armazenados no **banco de dados** em execução na instância LAMP que você iniciou.

[⬆ Voltar ao índice](#índice)

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
