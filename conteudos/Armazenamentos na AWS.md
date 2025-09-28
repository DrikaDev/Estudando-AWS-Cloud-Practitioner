## Armazenamentos na AWS

Ao estudarmos armazenamento na AWS, é comum surgir a dúvida: **quais serviços existem e quando usar cada um?**  
A AWS oferece diferentes tipos de armazenamento, cada um com **características e finalidades específicas**.

### Índice

- [Armazenamento de Objetos](#armazenamento-de-objetos) 
- [Armazenamento em Bloco](#armazenamento-em-bloco)
- [Armazenamento em Arquivos](#armazenamento-em-arquivos)  
- [Comparação lado a lado](#comparação-lado-a-lado)

---

## Armazenamento de Objetos

### Amazon S3 (Simple Storage Service)

- Armazenamento de **objetos** (arquivos).  

- Escalável, durável (99.999999999% de durabilidade).  

- Ideal para: arquivos, imagens, vídeos, backups, logs, data lakes.  

- Classes de armazenamento: Standard, Glacier, Intelligent-Tiering, etc.  

- Veja maiores detalhes sobre esse serviço clicando aqui 👉🏻 [Amazon S3 - Simple Storage Service](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/conteudos/Amazon%20S3%20-%20Simple%20Storage%20Service.md)

[⬆ Voltar ao índice](#índice)

---

## Armazenamento em Bloco

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

- Fica na mesma zona de disponibilidade que a instância EC2.  

- Você define a configuração (tamanho e tipo do volume).  

- Permite backups incrementais criando **snapshots** (cópias que servem para backup ou recuperação), ou seja, após o primeiro **snapshot**
  completo, os próximos registram apenas as alterações feitas desde o último snapshot, economizando espaço.

  <img width="1299" height="520" alt="image" src="https://github.com/user-attachments/assets/fe19752c-4c19-49c9-8658-f031c4d9518d" />

- Quando a instância encerra, os dados do volume permanecem disponíveis (desde que o volume não seja configurado para exclusão automática).

💡**Resumidamente falando:**

O **EBS** é como um **HD dedicado** para sua instância, é um volume “grudado” em uma instância, mas os dados ficam mesmo após encerrar a
instância, se o volume não for deletado.

[⬆ Voltar ao índice](#índice)

---

## Armazenamento em Arquivos

O armazenamento já está organizado em pastas e arquivos (como EFS), pronto para múltiplos usuários acessarem via rede.

- [Amazon EFS (Elastic File System)](#amazon-efs-elastic-file-system)
- [Amazon FSx](#amazon-fsx)

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

## Comparação lado a lado

| Característica            | Instance Store                                    | EBS (Elastic Block Store)                          | EFS (Elastic File System)                           | FSx (Amazon FSx)                                   |
|----------------------------|--------------------------------------------------|---------------------------------------------------|----------------------------------------------------|---------------------------------------------------|
| Tipo de armazenamento      | Bloco (disco local ligado ao host)              | Bloco (disco virtual)                             | Arquivos (sistema de rede)                         | Arquivos (sistema de rede gerenciado)            |
| Zona de disponibilidade    | Restrito à mesma AZ da instância EC2            | Restrito à mesma AZ da instância EC2              | Replicado em múltiplas AZs                         | Depende do tipo: geralmente replicado em AZs ou região |
| Acesso simultâneo          | Uma instância por vez                            | Uma instância por vez (salvo multi-attach)        | Várias instâncias ao mesmo tempo                   | Várias instâncias ao mesmo tempo (via SMB ou NFS) |
| Escalabilidade             | Limitada ao tamanho do disco físico             | Manual (definida no momento da criação)           | Automática, sob demanda                            | Manual ou automática, dependendo do tipo de FSx  |
| Dependência da instância   | Dados voláteis (perdem ao encerrar/reiniciar)  | Dados persistem, mas volume é acoplado à EC2      | Independente da vida útil de instâncias            | Independente da vida útil de instâncias          |
| Protocolo de acesso        | Conexão direta de bloco                          | Conexão direta de bloco                            | NFS (Network File System)                          | SMB (Windows) ou NFS (Linux), dependendo do tipo |
| Uso comum                  | Dados temporários, caches, processamento rápido | Disco para EC2 (banco de dados, SO, aplicações)  | Diretório compartilhado (web, microsserviços, etc) | Compartilhamento de arquivos corporativos, aplicações Windows ou HPC |

---

💡 **Observações rápidas:**

- **Instance Store:** volátil, local, rápido.  
- **EBS:** persistente, bloqueado a uma instância (salvo multi-attach).  
- **EFS:** compartilhado, elástico, baseado em protocolo NFS.  
- **FSx:** compartilhado, gerenciado, baseado em protocolos SMB (ou NFS conforme necessidade).

[⬆ Voltar ao índice](#índice)

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
