## VPC, Blocos CIDR, Sub-redes, Rotas, Firewalls e Gateways

Para conseguir sobreviver dentro desse universo de **Cloud**, é essencial saber os conceitos de **VPC, Blocos CIDR, Sub-redes, Rotas, Firewalls e Gateways**.

## Índice – Conceitos Fundamentais de Cloud

- [VPC (Virtual Private Cloud)](#vpc-virtual-private-cloud)  
- [Blocos CIDR](#blocos-cidr)  
- [Sub-redes (Subnets)](#sub-redes-subnets)  
- [Rotas (Route Tables)](#rotas-route-tables)  
- [Firewalls](#firewalls)  
- [Gateways (Portões de entrada)](#gateways-portões-de-entrada)  

---

## VPC (Virtual Private Cloud)

É uma **rede virtual privada**, segura, isolada e configurável que serve para termos **controle total sobre o nosso ambiente de rede na nuvem**.  

📌 É como um **condomínio fechado**: temos os portões de entrada e definimos quem pode entrar e como as casas (as sub-redes) se conectam.

Além disso, você pode criar uma **conexão de Rede Virtual Privada (VPN)** de hardware entre seu datacenter corporativo e sua VPC para aproveitar a nuvem AWS como uma extensão do seu datacenter corporativo.  

Principais funções da VPC:

- **Isolamento e segurança**  
  Cada VPC é separada das outras, então seus recursos não ficam “misturados” com os de outras empresas.  
  Você controla quem acessa, de onde acessa e por quais portas (usando regras de firewall: **Security Groups** em nível de instância e **NACLs** em nível de sub-rede).

- **Controle de endereços IP**  
  Você escolhe o intervalo de endereços IP (CIDR) que será usado.  
  Isso evita conflitos e te dá previsibilidade na comunicação entre recursos.

- **Organização dos recursos**  
  Dentro da VPC, você cria **sub-redes públicas e privadas**, organizando quais recursos ficam acessíveis externamente e quais ficam isolados.

- **Conexão com a Internet e outras redes**  
  A VPC permite configurar **gateways** para acessar a Internet, VPNs para conectar ao seu escritório físico, ou até *peering* para se conectar a outras VPCs.

- **Base para todos os serviços na nuvem**  
  Quase tudo que você lança (EC2, RDS, EKS, etc.) precisa estar dentro de uma VPC.  
  Ela é o **terreno onde suas aplicações vão morar**.

---

## Blocos CIDR

**CIDR** significa *Roteamento Interdomínio Sem Classes* (*Classless Inter-Domain Routing*)  

Na **VPC da AWS**, um **bloco CIDR** é o bloco de endereços IP a partir do qual os endereços IPv4 privados e públicos são alocados quando você cria uma VPC.  

Características principais:

- O bloco CIDR pode variar de **/28** (16 endereços IP) até **/16** (65.536 endereços IP).  
- Ele representa um **segmento de rede** e está associado a um **limite de rede**.  
- Após a criação, **não é possível alterar** o bloco CIDR principal da VPC.  
- É possível **adicionar blocos CIDR adicionais** caso seja necessário.  
- O bloco CIDR da VPC **não deve se sobrepor** a nenhum dos blocos CIDR da rede existente.  

---

## Sub-redes (Subnets)

Dentro da VPC, dividimos a rede em **sub-redes menores** que são deploiadas dentro da VPC.

- **Públicas** → conectadas à Internet (usadas por servidores que precisam ser acessados de fora, como um site) através do 'Internet Gateway'.  
- **Privadas** → sem acesso direto à Internet (usadas para proteger dados sensíveis como bancos de dados ou sistemas internos) tem apenas a rota local para
  acesso interno ou se precisar de acesso externo, precisa de um 'Nat Gateway'.  

📌 É como separar o **condomínio em áreas públicas** (quadra, parquinho) e **áreas privadas** (escritório do síndico).

---

## Rotas (Route Tables)

Definem **para onde o tráfego vai dentro da rede**. 
Cada rota especifica uma origem e um destino: da onde o pacote tá saindo e pra onde ele vai.

Exemplos:

- Se a **sub-rede pública** precisar sair para a Internet → rota aponta para o **Internet Gateway**.  
  - **Sub-rede pública** → rota default `0.0.0.0/0` aponta direto para o **Internet Gateway (IGW)**.

- Se a **sub-rede privada** precisar se comunicar com a Internet → rota aponta para o **NAT Gateway**.  
  - **Sub-rede privada** → rota default `0.0.0.0/0` aponta para o **NAT Gateway**.  
  - O **NAT Gateway** → tem rota para o **Internet Gateway (IGW)**.

📌 É como o **mapa do condomínio**: diz por onde as pessoas (pacotes de dados) devem circular.

---

## Firewalls

### 🛡️ NACLs (Network Access Control Lists)
- **Firewalls em nível de sub-rede**.  
- Controlam o tráfego de **entrada e saída** (*inbound/outbound*) para as sub-redes da sua VPC.  
- Diferente dos Security Groups (que funcionam em instâncias), as **NACLs atuam antes**, protegendo a sub-rede inteira.

Características principais:
- Funcionam por **regras numéricas** (rule number).  
- A AWS avalia da menor para a maior até achar uma que se aplique.  
- Permitem e negam tráfego (**Allow** ou **Deny**).  
- São **Stateless**: se liberar na entrada, precisa liberar também na saída (e vice-versa).  
  - Exemplo: liberar porta **80 (HTTP)** inbound não garante automaticamente que a resposta vá sair; você precisa liberar no outbound também.  
- Aplicam-se a **todas as instâncias da sub-rede**.

### 🛡️ Security Groups
- **Firewalls em nível de instância**.  
- Funcionam diretamente sobre cada recurso (EC2, RDS, etc.).  
- São **Stateful**: se você libera a entrada, a saída correspondente já é automaticamente liberada.
  
---

## Gateways (Portões de entrada)

- **Internet Gateway (IGW)** → fica atachado na VPC, conecta a sub-rede pública diretamente à Internet.  

- **NAT Gateway (Network Address Translation)** → fica em uma sub-rede pública e tem rota para a Internet via IGW.  
  - Permite que **recursos da sub-rede privada saiam para a Internet**, mas sem permitir conexões de entrada da Internet para eles.  
  - Ou seja: a máquina privada consegue acessar "lá fora", mas ninguém "lá de fora" consegue iniciar conexão com ela.

  > 👉 Então sim: o NAT Gateway é a “ponte” para a sub-rede privada `sair para a Internet, mas sem abrir essa sub-rede para acesso externo.

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
