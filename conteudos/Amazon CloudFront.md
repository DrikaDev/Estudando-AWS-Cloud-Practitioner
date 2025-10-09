## Amazon CloudFront

O **Amazon CloudFront** é um serviço de **Content Delivery Network (CDN)** que atua camada de aplicação 7 do modelo OSI.
É um serviço rápido e seguro que entrega **dados, vídeos, aplicativos e APIs** globalmente com **baixa latência** e **altas velocidades de transferência**.
Atua sobre protocolos de aplicação (HTTP/HTTPS) e não diretamente no nível de transporte ou rede.  
Isso permite cache inteligente, distribuição geográfica e personalização de conteúdo próximo do usuário final.  

## Funcionalidades Principais
- **Distribuição de Conteúdo**  
  Entrega conteúdo **estático** (HTML, CSS, imagens) e **dinâmico** (APIs, aplicativos) para usuários finais rapidamente.  
  O CloudFront é o serviço que expõe seu conteúdo para os usuários finais.  
  Todo o tráfego da internet passa primeiro pelo CloudFront.  
  Por estar distribuído globalmente, ele absorve picos de tráfego e reduz latência.  
  
- **Segurança Integrada**  
  Integra-se com **AWS Shield** para mitigação de DDoS e outros serviços de segurança da AWS.
  - O **AWS Shield Standard** é ativado automaticamente em CloudFront.  
  - Ele monitora e bloqueia tráfego malicioso em L3/L4, como SYN floods, UDP reflection attacks, etc.  
  - Para ataques mais sofisticados na camada de aplicação (L7), o **Shield Advanced** pode ser usado em conjunto com **WAF (Web Application Firewall)**
    para proteger o CloudFront.

> 💡 Em resumo: o CloudFront é o “canal de entrega” na camada de aplicação, e o Shield protege esse canal nas camadas mais baixas.

- **Origem Flexível**  
  Pode usar como origem:  
  - Amazon S3  
  - Elastic Load Balancing  
  - Amazon EC2  

- **Personalização com Lambda@Edge**  
  Permite **executar código próximo ao usuário final** para personalizar experiências e lógica de aplicação.

## Benefícios
- Redução de **latência** para usuários finais.  
- Acelera **distribuição de conteúdo web** globalmente.  
- **Segurança reforçada** contra ataques.  
- Integração fácil com outros serviços AWS para maior **flexibilidade** e **escalabilidade**.

---

# Distribuições no Amazon CloudFront

Uma **distribuição** do CloudFront é o sistema de rede global que acelera a entrega de **sites, APIs, vídeos ou outros conteúdos da web**.

## Função da Distribuição
- Define **de onde o CloudFront obtém os arquivos** que serão entregues aos usuários.  
- Pode usar como origem:  
  - **Bucket do Amazon S3**  
  - **Servidor HTTP**  

## Tipos de Distribuições
1. **Distribuição Web**  
   - Usada principalmente para **sites e aplicações web**.  

2. **Distribuição RTMP**  
   - Usada principalmente para **streaming de mídia** via Real-Time Messaging Protocol.

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
