## 🐧 O que é o Linux?

Linux é um sistema operacional criado em 1991 por Linus Torvalds.  
Ele gerencia os recursos de hardware e software de um computador e executa aplicativos.  

Principais características do Linux:  
- Código aberto (open source), disponível publicamente para modificações e expansões.  
- Suporta múltiplos usuários e multitarefa.  
- Desenvolvido para processamento em redes.  
- Oferece diversas ferramentas e utilitários do sistema.  

Com o Linux:
- Várias pessoas podem usar o mesmo computador ao mesmo tempo.
- É possível executar vários aplicativos simultaneamente.
- Ele é modular, estável e pode ser usado tanto em desktops quanto em servidores.

---

## 📦 Distribuição Linux

Uma **distribuição Linux** é uma versão empacotada do Linux, desenvolvida por um grupo ou empresa. 
Inclui:
- Funcionalidade principal do sistema operacional (**kernel**).
- Ferramentas complementares.
- Aplicativos de software.

**Exemplos de distribuições:**
- Amazon Linux 2
- Red Hat Enterprise Linux (RHEL)
- Debian
- Ubuntu

Na **AWS**, o Amazon Linux 2 é distribuído como uma **Amazon Machine Image (AMI)**.

---

## 💻 Formas de Interagir com o Linux

### Interface de Linha de Comando (CLI)
- Recebe comandos em forma de texto no terminal.
- Requer um **shell** para interpretar os comandos.

### Interface Gráfica do Usuário (GUI)
- Apresenta ícones visuais e elementos clicáveis.

---

## 🐚 Shell no Linux

- O **shell** interpreta os comandos e aciona o componente do kernel responsável.
- Cada shell tem sua própria sintaxe e recursos.

**Shells populares no Linux:**
- `sh` — Bourne shell original do Unix.
- `bash` — Bourne Again Shell, padrão na maioria das distribuições Linux.
- `ksh` — KornShell, comum no Unix.

**Principais diferenças entre shells:**
- Aparência do prompt de comando.
- Recursos como histórico de comandos e autocompletar.
- Tipo de editor de texto integrado.

---

## 📖 Páginas de Manual (man pages)

- São a principal fonte de documentação do Linux.
- Mostram finalidade, sintaxe e opções de comandos.

**Comando para acessar:**
```bash
man <comando>
```

### ⌨ Navegação nas páginas
- **Seta para cima/baixo** → rola uma linha.
- **Page Up / Page Down** → rola uma página.
- **Barra de espaço** → avança uma página.

---

## 🖥 Sintaxe de Comando

O fluxo de trabalho de login do Linux é composto por três etapas principais:

1. **Autenticação:**  
   O usuário é solicitado a fazer login com **nome de usuário** e **senha**.
2. **Carregamento das definições da sessão:**  
   As configurações são carregadas a partir dos arquivos de **perfil** do usuário.
3. **Prompt de comando:**  
   O usuário recebe um **prompt** no seu **diretório base**.

---

### ⌨ Dicas de uso no terminal
- Use a tecla **Tab** para **autocompletar comandos** no prompt.
- Comandos úteis no Linux:
  - `history` → mostra o histórico de comandos executados.
  - `whoami` → exibe o nome do usuário atual.
  - `hostname` → mostra o nome da máquina.

---

## Gerenciamento de Usuários no Linux

1. **Adicionar uma nova conta de usuário:**  
   Comando `useradd`.

2. **Redefinir a senha de um usuário:**  
   Comando `passwd`.

3. **Conceder permissões administrativas completas a um usuário:**  
   Comando `su root`, que eleva o usuário ao perfil administrador (root) no ambiente do usuário.

> **Dica:** Em muitas distribuições, recomenda-se usar `sudo` para executar comandos com permissões administrativas sem trocar totalmente para o usuário root.  

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
