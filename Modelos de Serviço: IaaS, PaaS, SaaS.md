# ☁️ Modelos de Serviço de Nuvem

Os **modelos de serviço em nuvem** nos ajudam a definir o **nível de controle** que a empresa tem sobre a infraestrutura e os serviços fornecidos pelo provedor de nuvem.  

Eles também orientam a escolha da **solução mais adequada às necessidades do negócio**, variando no nível de responsabilidades sobre a forma como o serviço é configurado e gerenciado.

---

## 🖥️ IaaS — Infraestrutura como Serviço
**O que é?**  
Oferece infraestrutura de TI virtualizada, como servidores, redes e armazenamento.  
O cliente é responsável pelo sistema operacional, aplicações e dados, enquanto a nuvem cuida do hardware.  

**Exemplos na AWS:**  
- **Amazon EC2** → criação de instâncias (servidores virtuais).  
- **Amazon EBS** → volumes de armazenamento em bloco.  
- **Amazon VPC** → redes privadas virtuais.  

**Quando usar:**  
- Hospedagem de sites e sistemas que exigem personalização.  
- Migração de servidores físicos para a nuvem.  
- Criação de ambientes de teste e desenvolvimento.

---

## ⚙️ PaaS — Plataforma como Serviço
**O que é?**  
Fornece um ambiente pronto para desenvolver, testar e gerenciar aplicativos.  
O desenvolvedor cuida apenas do **código e dados da aplicação**, enquanto a AWS cuida da infraestrutura, escalabilidade, balanceamento e monitoramento.  

**Exemplos na AWS:**  
- **AWS Elastic Beanstalk** → implantação de aplicativos sem se preocupar com servidores.  
- **AWS Lambda** → execução de funções sob demanda (serverless).  
- **Amazon RDS** → banco de dados relacional gerenciado.  

**Quando usar:**  
- Desenvolvimento de aplicações web e móveis sem precisar gerenciar servidores.  
- Projetos que exigem **rápida escalabilidade**.  
- Equipes que querem focar apenas no código e não na infraestrutura.

---

## 👩‍💻 SaaS — Software como Serviço
**O que é?**  
O software já vem pronto e é totalmente gerenciado pelo provedor de nuvem.  
O usuário só precisa saber **como utilizar** o aplicativo, sem se preocupar com instalação, manutenção ou atualizações.  

**Exemplos na AWS e no dia a dia:**  
- **Amazon Chime** → serviço de videoconferência da AWS.  
- **AWS WorkMail** → serviço de e-mail corporativo.  
- **Dropbox, Google Drive, Office 365** (fora da AWS) → exemplos de SaaS populares.  

**Quando usar:**  
- Empresas que desejam soluções prontas sem investir em infraestrutura.  
- Uso de e-mails corporativos, ferramentas de colaboração e videoconferência.  
- Redução de custos com manutenção e suporte técnico.

---

## 🎯 Resumindo

| Modelo | Responsável pela Infraestrutura | Exemplos AWS | Quem usa |
|--------|--------------------------------|--------------|----------|
| **IaaS** | Cliente gerencia SO, apps e dados | EC2, EBS, VPC | DevOps, sysadmins |
| **PaaS** | Cliente gerencia apenas o código e dados | Elastic Beanstalk, Lambda, RDS | Desenvolvedores |
| **SaaS** | Tudo gerenciado pelo provedor | WorkMail, Chime | Usuários finais |

> ✅ *Lembre-se: quanto mais subimos de IaaS para SaaS, menos responsabilidade temos e mais facilidade no uso.*  

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
