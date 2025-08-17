## 🧪 Lab - Monitorar uma instância do EC2

Registro e monitoramento são técnicas implementadas para alcançar um objetivo comum.  
Eles trabalham juntos para assegurar que as referências de desempenho de um sistema e as diretrizes de segurança sejam sempre atendidas.  

**Registro**  
Se refere a gravar e armazenar eventos de dados em arquivos log.  
De um ponto de vista de segurança, o registro ajuda os administradores de segurança a identificar sinais de alerta que estejam facilmente sendo ignorados no sistema.  

**Monitoramento**  
É o processo de analisar e coletar dados para ajudar a assegurar o desempenho ideal.  
O monitoramento ajuda a detectar acesso não autorizado e a alinhar o uso dos serviços com a segurança organizacional.  

Neste laboratório, vamos criar um alarme com o **CloudWatch** que iniciará quando a instância do Amazon EC2 exceder o limite de utilização de uma unidade de processamento central (CPU) específica.  
Vamos criar uma assinatura usando o **Amazon SNS** que enviará um e-mail se esse alarme disparar.  
Faremos login na instância do EC2 e executaremos um comando de teste de estresse que fará com que a utilização de CPU da instância do EC2 alcance 100%.  

Este teste simula um agente malicioso ganhando controle da instância do EC2 e atingindo o pico da CPU.  
Picos na CPU podem ter diferentes causas, e uma delas é um **malware**!  

---

