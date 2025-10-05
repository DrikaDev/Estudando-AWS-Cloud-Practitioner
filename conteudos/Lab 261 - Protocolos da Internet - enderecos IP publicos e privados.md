## 🧪 Lab 261 - Protocolos da Internet: endereços IPs Públicos e Privados

Nessa atividade, vamos:  
- Resumir e investigar o **cenário do cliente**.  
- Analisar a diferença entre **endereço IP público e privado**.  
- Desenvolver uma **solução para o problema do cliente**.  

### Índice

1. [Cenário](#cenário)
2. [Ticket do Cliente](#ticket-do-cliente)
3. [Arquitetura do Cliente](#arquitetura-do-cliente)
4. [Tarefa 1: Investigar o ambiente do cliente](#tarefa-1-investigar-o-ambiente-do-cliente)
5. [Tarefa 2: Usar SSH para se conectar a uma instância do Amazon Linux EC2](#tarefa-2-usar-ssh-para-se-conectar-a-uma-instância-do-amazon-linux-ec2)
6. [Aprendizado do laboratório](#aprendizado-do-laboratório)
7. [Resposta ao Cliente: Acesso à Internet e CIDR da VPC](#resposta-ao-cliente-acesso-à-internet-e-cidr-da-vpc)

---

### Cenário

Você atua como **engenheiro de suporte de nuvem** na **Amazon Web Services (AWS)**.  

Durante o seu turno, um cliente de uma empresa da Fortune 500 abriu um chamado solicitando auxílio em relação a um **problema de rede** dentro da infraestrutura AWS.  

### Ticket do Cliente

<img width="1203" height="556" alt="image" src="https://github.com/user-attachments/assets/1df4d540-7f68-4b62-83a7-14d50075b452" />

---

### Arquitetura do Cliente

O cliente descreveu a seguinte arquitetura:  

- Uma **VPC** com **CIDR 10.0.0.0/16**  
- Um **gateway de internet**  
- Uma **sub-rede pública** com:
  - **Instância A** (sem acesso à internet)  
  - **Instância B** (com acesso à internet)  

📌 **Figura:** Arquitetura do cliente mostrando a VPC, gateway de internet, sub-rede pública e as duas instâncias do EC2.  

<img width="655" height="427" alt="arquitetura" src="https://github.com/user-attachments/assets/2dd8399d-8b0c-40a0-8624-478ee6217833" />

---

### Tarefa 1: Investigar o ambiente do cliente

Lembre-se de que você abordou anteriormente **endereços IP públicos e privados** e **CIDRs**.  
Ao longo deste laboratório:  
- Pense nas diferenças entre endereços IP públicos e privados (**Tarefa 1**).  
- Pense na importância de usar intervalos de IP **privados** em vez de intervalos de IP **públicos** (**Tarefa 2**).  

### Cenário

Jess, o cliente solicitando assistência, possui **duas instâncias do EC2** em uma **VPC (10.0.0.0/16)**:

- **Instância A** → ❌ Não consegue acessar a internet pois não tinha um IPv4 público.  

<img width="886" height="368" alt="instancia-a" src="https://github.com/user-attachments/assets/0461aaed-3edd-4757-9493-bee57038ff25" />

- **Instância B** → ✅ Consegue acessar a internet pois possui um IPV4público.  

<img width="886" height="371" alt="instancia-b" src="https://github.com/user-attachments/assets/c8d3da76-9b82-4474-b6de-ddc789dd68b3" />

Ambas estão configuradas da mesma forma.  

Jess suspeita que o problema esteja na **configuração da instância**.  

Além disso, ele questiona sobre o uso de um **intervalo público de endereços IP** em uma nova VPC e pede mais informações.  

---

### Método de Solução de Problemas

Uma abordagem comum é usar o modelo **de baixo para cima** (ou em camadas), como no modelo OSI.  
Veja o paralelo com a infraestrutura da AWS:  

| **Camada OSI** | **Função** | **Infraestrutura AWS** |
|-----------------|------------|-------------------------|
| **7. Aplicação** | Interação do usuário final | Aplicação |
| **6. Apresentação** | Tradução entre camadas | Servidores web / aplicações |
| **5. Sessão** | Estabelecimento de sessão, segurança | Instâncias do EC2 |
| **4. Transporte** | TCP, controle de fluxo | Grupos de segurança, NACL |
| **3. Rede** | Pacotes com endereços IP | Tabelas de rota, IGW, Sub-redes |
| **2. Data Link** | Estruturas com endereços MAC | Tabelas de rota, IGW, Sub-redes |
| **1. Física** | Bits, cabos, transmissão | Regiões, Zonas de Disponibilidade |

> 📊 Essa tabela mostra como os elementos da **infraestrutura AWS** se relacionam com o modelo **OSI** e pode ajudar na solução de problemas.

---

### Tarefa 2: Usar SSH para se conectar a uma instância do Amazon Linux EC2

Nesta tarefa, vamos conectar a uma **instância do EC2 do Amazon Linux** usando um utilitário **SSH**.  

**❓Pergunta:** Você conseguiu usar o SSH para se conectar às duas instâncias? Por que sim ou por que não?

1. Instância A  
- Ao tentar conexão via SSH → ❌ *Connection timed out*  
- Motivo: sem IP público, só possui endereço IP privado → **não acessível de fora da VPC**

<img width="886" height="200" alt="ssh-a" src="https://github.com/user-attachments/assets/c94e3497-ec75-4841-aa30-ae4bbb21e2a3" />

2. Instância B  
- Ao tentar conexão via SSH → ✅ *Conexão bem-sucedida*  
- Motivo: possui IPv4 público → **acessível de fora da VPC**

<img width="886" height="471" alt="ssh-b" src="https://github.com/user-attachments/assets/fd410e6e-aafe-4880-8239-6a427fa5ae62" />

### 💡 Conclusão e Solução

➡️ O problema não estava na arquitetura, mas na **configuração da instância A**.  
➡️ Para corrigir, foi atribuído um **Elastic IP** à instância A.  

<img width="886" height="127" alt="etapa1" src="https://github.com/user-attachments/assets/d752d407-2792-4213-90c9-ae03eb3dc5e2" />  
<img width="886" height="130" alt="etapa2" src="https://github.com/user-attachments/assets/216ddc8d-4f7f-4c3f-88dd-cc07f5798d66" />  
<img width="886" height="328" alt="etapa3" src="https://github.com/user-attachments/assets/5dfae5e7-dc42-4cc3-91fa-8619367228eb" />  
<img width="886" height="197" alt="etapa4" src="https://github.com/user-attachments/assets/c76dbe21-102d-43f0-bb01-1b8faa59d122" />  
<img width="508" height="206" alt="etapa5" src="https://github.com/user-attachments/assets/1edf3048-72dc-4b0d-a63d-100e2a331f1a" />  
<img width="886" height="92" alt="etapa6" src="https://github.com/user-attachments/assets/f3d1a258-8d05-4f64-a54e-444f1c786f28" />

---

### Após a mudança

- A instância A passou a ter um IPv4 público  

<img width="886" height="663" alt="elastic-ip" src="https://github.com/user-attachments/assets/41d6bb23-cc4c-4c04-84fd-ac17fad4db34" />

- A conexão via SSH, que antes estava dando *Connection timed out*, dessa vez foi realizada com sucesso 🎉  

<img width="517" height="141" alt="ssh-ok1" src="https://github.com/user-attachments/assets/98fdab25-2dfc-435c-9b27-8788058b0ca8" />  

<img width="886" height="503" alt="ssh-ok2" src="https://github.com/user-attachments/assets/6ccbda7f-5f70-4fbf-bf49-aa2eb3bb84c8" />

---

### Aprendizado do laboratório

- **IPs privados** → usados dentro da VPC, não permitem acesso externo.  
- **IPs públicos / Elastic IPs** → permitem acesso externo, essenciais para conexão via SSH ou comunicação com a internet.  
- **Validação de configuração** → verificar atribuição de IPs, rotas e permissões de segurança é fundamental em troubleshooting.  

✅ Atividade concluída com sucesso, aplicando conceitos de **endereços IP públicos e privados na AWS** e práticas de **troubleshooting em instâncias EC2**.

---

### Resposta ao Cliente: Acesso à Internet e CIDR da VPC

Olá Jess,

Após revisar o seu ambiente e analisar as instâncias A e B em sua VPC, aqui estão nossas observações e recomendações:

1. Problema do Acesso à Internet:  

- A **instância A** não consegue acessar a internet porque **não possui um endereço IP público atribuído**.  
- A **instância B** possui um **endereço IP público**, o que permite a conexão direta com a internet e o uso do SSH externamente.  
- Observação: endereços IP **privados** só funcionam dentro da VPC, por isso a instância A não consegue comunicação externa.

2. Solução recomendada:  

- Atribuir um **endereço IP público** à instância A se você deseja que ela se conecte à internet.  
- Alternativamente, use um **NAT Gateway** em uma sub-rede pública para permitir que instâncias privadas acessem a internet de forma segura, sem expor seus IPs privados.

3. Sobre o uso de um CIDR público (como 12.0.0.0/16) para a VPC:  

- **Não recomendamos** usar intervalos de IP públicos para a sua VPC.  
- Razões:
  - Pode gerar **conflitos de endereçamento IP** com a internet pública.  
  - Reduz a **segurança**, pois seus IPs estariam roteáveis globalmente.  
- Recomendação: continue usando **intervalos de IP privados** (como 10.0.0.0/16), que podem ser expostos à internet de forma controlada apenas via NAT ou IPs públicos específicos.

Se desejar, podemos ajudá-la a **atribuir um IP público à instância A** ou configurar um **NAT Gateway**, garantindo que todas as suas instâncias privadas tenham acesso seguro à internet.

Atenciosamente,  
**[Seu Nome]**  
Engenheiro de Suporte de Nuvem AWS

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
