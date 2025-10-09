## Armazenamentos na AWS

Ao estudarmos armazenamento na AWS, é comum surgir a dúvida: **quais serviços existem e quando usar cada um?**  
A AWS oferece diferentes tipos de armazenamento, cada um com **características e finalidades específicas**.

### Índice

- [Armazenamento de Objetos - Amazon S3](#armazenamento-de-objetos---amazon-s3)
- [Armazenamento em Bloco - Instance Store & EBS](#armazenamento-em-bloco---instance-store--ebs)
- [Armazenamento em Arquivos - EFS & FSx](#armazenamento-em-arquivos---efs--fsx)
- [Armazenamento de cache de memória - Amazon ElastiCache](#armazenamento-de-cache-de-memória---amazon-elasticache)

---

## Armazenamento de Objetos - Amazon S3

- Armazenamento de **objetos** (arquivos).  

- Escalável, durável (99.999999999% de durabilidade).  

- Ideal para: arquivos, imagens, vídeos, backups, logs, data lakes.  

- Classes de armazenamento: Standard, Glacier, Intelligent-Tiering, etc.  

- Veja maiores detalhes sobre esse serviço clicando aqui 👉🏻 [Amazon S3 - Simple Storage Service](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/conteudos/Amazon%20S3%20-%20Simple%20Storage%20Service.md)

[⬆ Voltar ao índice](#índice)

---

## Armazenamento em Bloco - Instance Store & EBS

Os dados são organizados em blocos de armazenamento que o sistema operacional enxerga como um disco físico.  
O armazenamento é dividido em unidades fixas (blocos). O sistema operacional gerencia partições, sistemas de arquivos e arquivos sobre 
esses blocos.

- [AWS Instance Store](#aws-instance-store)
- [Amazon EBS (Elastic Block Store)](#amazon-ebs-elastic-block-store)

---

### AWS Instance Store

- Armazenamento **local** ligado ao host da EC2.  

- Sua latência é muito rápida (a leitura e escrita de dados acontecem quase instantaneamente + transferência de dados em alta velocidade)
  já que o armazenamento está fisicamente conectado ao servidor (host) da instância EC2, não na rede.

- Mas é **efêmero e volátil** (os dados somem ao encerrar/desligar/reiniciar  a instância).

- Indicada para:

  - **dados temporários** de aplicativos como os dados que são criados para uso imediato durante a execução do processo, e que não precisam ser
    salvos após a conclusão.

  - **caches de alto desempenho** como armazenamento de **Redis** e **Memcached**:

    - **Redis**: banco de dados em memória, muito rápido, usado principalmente para armazenar dados temporários ou cache, funciona como
      uma tabela de chave-valor, permitindo acessar informações instantaneamente.
      - **Usos comuns:**  
        - Armazenar sessões de usuários (login, carrinho de compras).  
        - Cache de resultados de consultas pesadas.  
        - Contadores, filas e mensagens em tempo real.    

    - **Memcached**: também é um sistema de cache em memória, extremamente rápido e leve, ideal para cache simples, rodando diretamente
      na instância para acelerar consultas a banco de dados.
      - **Usos comuns:**  
        - Cache de páginas web ou partes de uma aplicação.  
        - Resultados temporários de cálculos frequentes.  

  - **processamento de dados que não precisa ser pesistido** como processamento de grandes volumes de logs ou imagens temporárias em
    pipelines de dados onde os arquivos são criados, processados e descartados no final, não havendo necessidade de salvá-los permanentemente.

💡**Resumidamente falando:**

A **Instance Store** é ideal para dados que precisam de velocidade extrema e não precisam ser armazenados de forma permanente.

[⬆ Voltar ao índice](#índice)

---

### Amazon EBS (Elastic Block Store)

Na AWS, um **EBS** é o volume de armazenamento usado por instâncias do **EC2 (Elastic Compute Cloud)**.  

Foi projetado para **durabilidade de dados**, e os volumes são **replicados automaticamente dentro da sua Zona de Disponibilidade** para evitar perda de dados devido à falha de qualquer componente individual.  

**Características principais:**  

- **Anexado a uma instância EC2**  
  Os volumes do EBS são conectados a uma instância e aparecem como uma unidade de rede que você pode montar e formatar usando o sistema de arquivos de sua escolha.

- **Uso típico**  
  - Armazenamento primário para dados que exigem atualizações frequentes.  
  - Unidade de sistema para uma instância EC2.  
  - Armazenamento para aplicativos de banco de dados.

- **Durabilidade e segurança**  
  A replicação automática dentro da Zona de Disponibilidade garante que seus dados permaneçam seguros mesmo em caso de falhas de hardware.

- **Localização e configuração**  
  - Fica na **mesma Zona de Disponibilidade** que a instância EC2.  
  - Você define a **configuração** (tamanho e tipo do volume).  

- **Backups e snapshots**  
  - Permite backups incrementais criando **snapshots** (cópias para backup ou recuperação).  
  - Após o primeiro snapshot completo, os próximos registram apenas as alterações desde o último snapshot, economizando espaço.

- **Persistência de dados**  
  Quando a instância encerra, os dados do volume permanecem disponíveis (desde que o volume não seja configurado para exclusão automática).

<img width="1299" height="520" alt="Amazon EBS" src="https://github.com/user-attachments/assets/fe19752c-4c19-49c9-8658-f031c4d9518d" />

**Resumo:**  

O **EBS** é como um **HD dedicado** para sua instância: é um volume “grudado” na instância, mas os dados permanecem mesmo após encerrar a instância, se o volume não for deletado.

[⬆ Voltar ao índice](#índice)

---

## Armazenamento em Arquivos - EFS & FSx

O armazenamento já está organizado em pastas e arquivos (como EFS), pronto para múltiplos usuários acessarem via rede.

- [Amazon EFS (Elastic File System)](#amazon-efs-elastic-file-system)  
- [Amazon FSx (File System)](#amazon-fsx-file-system)  

---

### Amazon EFS (Elastic File System)

- Armazena dados na infraestrutura da AWS, replicados em múltiplas zonas de disponibilidade dentro da região que você escolher,
  garantindo assim alta disponibilidade.  

- É um sistema de arquivos compartilhado, acessível por várias instâncias ao mesmo tempo.

- Você só cria o sistema de arquivos e o “monta” nas suas instâncias EC2 ou em servidores on-premises (via Direct Connect ou VPN).  

- É elástico sob demanda, cresce ou diminui automaticamente, sem necessidade de ajustes manuais.  

- O armazenamento não depende da vida útil de uma instância.  

👉🏻 Para o usuário, ele aparece como um ponto de montagem de diretório (ex: ` /mnt/efs `) na instância EC2.

**Isso acontece porque:**

- O EFS é um **serviço gerenciado** pela AWS e independente da instância.

- Funciona como um sistema de arquivos de rede através do prococolo NFS que:
  - a EC2 (ou outro servidor) usa para se conectar ao EFS e enxergar os arquivos como se fosse um diretório local.
  - permite que computadores diferentes compartilhem e acessem arquivos como se estivessem em um mesmo disco local,
    mesmo estando em servidores separados.

  > o servidor NFS é o endpoint gerenciado do EFS ` fs-12345678.efs.us-east-1.amazonaws.com `.

- Diferente do EBS, que é um volume em bloco acoplado a uma única instância por vez, o EFS permanece disponível enquanto o serviço existir.

💡**Resumidamente falando:**

O **EFS** é como um diretório de rede que várias instâncias podem acessar simultaneamente, ou seja, é “compartilhado” e independente de
instâncias, e está sempre disponível enquanto você não o excluir.

[⬆ Voltar ao índice](#índice)

---

### Amazon FSx (File System)

- É um **serviço de sistema de arquivos totalmente gerenciado** pela AWS.  

- Permite criar sistemas de arquivos **prontos para uso**, com suporte a diferentes protocolos, sem precisar se preocupar com a
  infraestrutura subjacente.

- O **x** → denota que é uma família de sistemas de arquivos gerenciados, ou seja, a AWS não entrega apenas um tipo, mas vários
  “sabores” de file system (Windows, Lustre, NetApp, ZFS).

#### Tipos de FSx:

- **FSx for Windows File Server** → compatível com **protocolo SMB (Server Message Block)**:
  - É um **protocolo de rede** que permite que computadores compartilhem **arquivos, impressoras e portas seriais**.  
  - Muito usado em **ambientes Windows** para acessar pastas compartilhadas na rede.  
  - Com SMB, os usuários ou aplicações **enxergam os arquivos remotos como se estivessem no próprio computador**.  

**Resumo:** SMB = protocolo para **compartilhamento de arquivos em rede**, usado pelo FSx for Windows File Server.

- **FSx for Lustre** → alto desempenho para HPC e processamento de dados.  

- **FSx for NetApp ONTAP** → funcionalidades avançadas de storage empresarial.  

- **FSx for OpenZFS** → baseado no sistema de arquivos ZFS.  

💡**Resumidamente falando:**

O **FSx** é a forma da AWS fornecer **sistemas de arquivos gerenciados** para diferentes necessidades de aplicações, seja Windows, 
Linux ou alto desempenho.  

[⬆ Voltar ao índice](#índice)

---

## Armazenamento de cache de memória - Amazon ElastiCache

O **Amazon ElastiCache** é um serviço de **armazenamento de dados em memória totalmente gerenciado** da AWS.  
Ele é projetado para **acelerar aplicações web dinâmicas**, reduzindo latência e aumentando a taxa de transferência em comparação com bancos de dados baseados em disco.  

### Mecanismos Suportados
O ElastiCache oferece suporte a dois mecanismos de armazenamento em memória de código aberto:  
- **Redis**  
  - Usado para **cache de banco de dados**, **gerenciamento de sessões**, **mensageria** e **enfileiramento**.  
  - Suporta persistência opcional e recursos avançados como **replicação** e **alta disponibilidade**.

- **Memcached**  
  - Ideal para **cache de conjuntos de dados menores e simples**.  
  - Muito rápido e eficiente para operações de leitura/escrita em memória.

### Principais Benefícios
- **Desempenho consistente**: baixa latência mesmo com grandes volumes de dados.  
- **Escalabilidade**: permite lidar com sites de alto tráfego e grandes conjuntos de dados.  
- **Gerenciamento simplificado**: AWS cuida de provisionamento, patching, backup e monitoramento.  

💡 Em resumo: O ElastiCache atua como um **cache em memória** que acelera aplicativos, reduzindo a dependência de armazenamento em disco e melhorando a experiência do usuário.

---

💡 **Observações rápidas sobre tipos de armazenamento na AWS:**

- **Instance Store:** armazenamento em bloco, usado por instâncias EC2 - volátil, local, muito rápido - ideal para dados temporários ou cache transitório - armazenamento em bloco, usado por instâncias EC2.
- **EBS (Elastic Block Store):** armazenamento em bloco, usado por instâncias EC2 - persistente, usado para sistemas de arquivos e bancos de dados.  
- **EFS (Elastic File System):** armazenamento de arquivos, compartilhado entre instâncias, elástico, baseado em protocolo NFS - perfeito para múltiplas instâncias acessando o mesmo sistema de arquivos.  
- **FSx:** armazenamento de arquivos, compartilhado entre instâncias, gerenciado, baseado em protocolos SMB (ou NFS conforme necessidade) - voltado para workloads específicos como Windows ou Lustre.  
- **ElastiCache:** armazenamento **em memória**, rápido, volátil - usado para **cache de banco de dados, gerenciamento de sessões e enfileiramento**, suportando Redis e Memcached.

[⬆ Voltar ao índice](#índice)

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
