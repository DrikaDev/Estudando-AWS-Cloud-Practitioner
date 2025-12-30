## Criação de um Aplicativo Web Dimensionável e Altamente Disponível

## Visão Geral e Objetivos

Em vários cursos da **AWS Academy**, os alunos concluíram laboratórios práticos utilizando diferentes serviços e recursos da AWS para:

- Criar instâncias de computação
- Instalar sistemas operacionais e softwares
- Implantar código
- Proteger recursos
- Configurar balanceamento de carga e Auto Scaling
- Arquitetar soluções altamente disponíveis

Neste projeto, o desafio é utilizar serviços familiares da AWS para criar uma solução **sem orientações detalhadas**, aplicando as habilidades adquiridas ao longo do aprendizado.

### Ao final deste projeto, você será capaz de:

- Criar um **diagrama de arquitetura** representando diversos serviços da AWS e suas interações
- Estimar custos usando a **AWS Pricing Calculator**
- Implantar um **aplicativo web funcional** em uma máquina virtual com banco de dados relacional
- Separar as camadas da aplicação (servidor web e banco de dados)
- Criar uma **rede virtual segura e acessível publicamente**
- Implantar um aplicativo web com **carga distribuída**
- Definir configurações de rede apropriadas
- Implementar **alta disponibilidade e escalabilidade**
- Configurar **permissões de acesso entre serviços da AWS**

---

## Cenário

A **Universidade Exemplo** está se preparando para o novo ano escolar.  
O departamento de admissões recebeu reclamações de que o aplicativo web de registros dos alunos é lento ou fica indisponível durante períodos de pico.

<img width="2242" height="750" alt="image" src="https://github.com/user-attachments/assets/8a550b4b-c37b-4261-a855-fe2a698fe6b7" />

Você é um **engenheiro de nuvem**, responsável por criar uma **Prova de Conceito (POC)** para hospedar o aplicativo web na AWS.  
Seu desafio é planejar, projetar, criar e implantar a solução seguindo as **práticas recomendadas do AWS Well-Architected Framework**.

Durante períodos de pico, o aplicativo deve:
- Suportar milhares de usuários
- Ser altamente disponível
- Ser escalável
- Possuir balanceamento de carga
- Oferecer alto desempenho

O aplicativo permite:
- Visualizar registros
- Adicionar registros
- Excluir registros
- Modificar registros de alunos

---

## Requisitos da Solução

### Funcionais
- Permitir visualizar, adicionar, excluir e modificar registros sem atrasos perceptíveis

### Não Funcionais

- **Carga balanceada:** distribuição adequada do tráfego
- **Dimensionável:** capacidade de escalar conforme a demanda
- **Altamente disponível:** baixa indisponibilidade em caso de falha
- **Seguro:**
  - Banco de dados inacessível pela internet
  - Acesso apenas por portas apropriadas
  - Credenciais não hardcoded
- **Econômico:** custos controlados
- **Alto desempenho:** operações rápidas sob cargas normais e de pico

---

## Pressuposições

- Implantação em **uma única região AWS**
- Sem necessidade de HTTPS ou domínio personalizado
- Uso de máquinas **Ubuntu**
- Uso do código JavaScript fornecido
- Banco de dados em **uma única Zona de Disponibilidade**
- Site acessível publicamente, sem autenticação
- Estimativa de custo aproximada

> **Aviso:** Boas práticas de segurança recomendam autenticação institucional, mas isso está fora do escopo da POC.

---

## Abordagem

Recomendação: desenvolver a solução **em fases**, garantindo funcionamento básico antes de adicionar complexidade.

---

## Fase 1: Planejamento do Design e Estimativa de Custo

### Tarefa 1: Criar um Diagrama da Arquitetura
Crie um diagrama ilustrando a arquitetura proposta e como os requisitos serão atendidos.

**Referências:**
- Ícones de arquitetura da AWS
- Diagramas de arquitetura de referência da AWS

### Tarefa 2: Desenvolver Estimativa de Custo
- Estimar o custo para **12 meses** na região `us-east-1`
- Usar a **AWS Pricing Calculator**
- Apresentar ao instrutor (se solicitado)

**Referências:**
- AWS Pricing Calculator
- Modelo de apresentação em PowerPoint

---

## Fase 2: Criar um Aplicativo Web Funcional Básico

### Tarefa 1: Criar uma Rede Virtual
- Criar VPC e sub-redes para hospedar o aplicativo

**Referência:**
- Laboratório AWS Academy – Criar uma VPC

### Tarefa 2: Criar uma Máquina Virtual
- Criar uma instância EC2
- Usar a AMI mais recente do Ubuntu
- Instalar o aplicativo web e banco de dados usando:
  - `SolutionCodePOC`

### Tarefa 3: Testar a Implantação
- Acessar via endereço IPv4 público
- Testar operações CRUD

---

## Fase 3: Desacoplamento dos Componentes

### Tarefa 1: Alterar Configuração da VPC
- Criar sub-redes privadas em pelo menos duas AZs

### Tarefa 2: Criar Banco de Dados no Amazon RDS
- Criar RDS MySQL
- Permitir acesso apenas pelo aplicativo
- Não ativar monitoramento aprimorado

### Tarefa 3: Configurar Ambiente AWS Cloud9
- Usar instância `t3.micro`
- Acessar via SSH

### Tarefa 4: Provisionar AWS Secrets Manager
- Criar segredo com credenciais do banco
- Usar `Script-1` via AWS CLI

### Tarefa 5: Criar Nova Instância para o Servidor Web
- Usar `Solution Code for the App Server`
- Anexar o perfil `LabInstanceProfile`

### Tarefa 6: Migrar Banco de Dados
- Migrar dados da EC2 para o RDS
- Usar `Script-3` do Cloud9

### Tarefa 7: Testar o Aplicativo
- Validar operações CRUD

---

## Fase 4: Alta Disponibilidade e Dimensionamento

### Tarefa 1: Criar um Application Load Balancer
- Usar pelo menos duas AZs

### Tarefa 2: Implementar Amazon EC2 Auto Scaling
- Criar Launch Template
- Criar Auto Scaling Group
- Usar política de monitoramento de objetivo

### Tarefa 3: Acessar o Aplicativo
- Testar funcionalidades via Load Balancer

### Tarefa 4: Testar o Aplicativo com Carga
- Usar `Script-2` do Cloud9
- Acessar via URL do Load Balancer

**Referência:**
- Repositório de ferramentas de load test no GitHub

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
