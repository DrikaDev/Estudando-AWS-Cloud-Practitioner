## 🌐 Protocolos da Internet: IPs Públicos e Privados na AWS

Nessa atividade, **“Protocolos da Internet: endereços IP públicos e privados”**, recebemos um desafio interessante: investigar um problema real de um cliente e propor uma solução.

---

## 📌 O Problema

O cliente possuía uma **VPC (Nuvem Virtual Privada)** com duas instâncias do **Amazon EC2**, chamadas **A** e **B**.

> Embora ambas estivessem na mesma sub-rede pública e com configurações semelhantes,  
> a instância **A** não conseguia acessar a internet, enquanto a instância **B** conseguia.

**Figura: Arquitetura do cliente**

<img width="655" height="427" alt="arquitetura" src="https://github.com/user-attachments/assets/2dd8399d-8b0c-40a0-8624-478ee6217833" />

---

## 🔎 Tarefa 1 – Entender o ambiente

Acessamos o console do EC2 e identificamos:

- **Instância A → não tinha um IPv4 público**
  
<img width="886" height="368" alt="instancia-a" src="https://github.com/user-attachments/assets/0461aaed-3edd-4757-9493-bee57038ff25" />

- **Instância B → possuía um IPv4 público**

<img width="886" height="371" alt="instancia-b" src="https://github.com/user-attachments/assets/c8d3da76-9b82-4474-b6de-ddc789dd68b3" />

---

## 🔎 Tarefa 2 – Testar conectividade com SSH

### Instância A  
- Ao tentar conexão via SSH → ❌ *Connection timed out*  
- Motivo: sem IP público, só possui endereço IP privado → **não acessível de fora da VPC**

<img width="886" height="200" alt="ssh-a" src="https://github.com/user-attachments/assets/c94e3497-ec75-4841-aa30-ae4bbb21e2a3" />

---

### Instância B  
- Ao tentar conexão via SSH → ✅ *Conexão bem-sucedida*  
- Motivo: possui IPv4 público → **acessível de fora da VPC**

<img width="886" height="471" alt="ssh-b" src="https://github.com/user-attachments/assets/fd410e6e-aafe-4880-8239-6a427fa5ae62" />

---

## 💡 Conclusão e Solução

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

## 📚 Aprendizado do laboratório

- **IPs privados** → usados dentro da VPC, não permitem acesso externo.  
- **IPs públicos / Elastic IPs** → permitem acesso externo, essenciais para conexão via SSH ou comunicação com a internet.  
- **Validação de configuração** → verificar atribuição de IPs, rotas e permissões de segurança é fundamental em troubleshooting.  

✅ Atividade concluída com sucesso, aplicando conceitos de **endereços IP públicos e privados na AWS** e práticas de **troubleshooting em instâncias EC2**.
