## 🐧 Trabalhar com o sistema de arquivos do Linux

### 📚 Objetivos da lição
- Navegar por arquivos e diretórios no Linux.
- Explicar comandos básicos para gerenciar arquivos e diretórios.
- Comparar caminhos absolutos e relativos.

---

## 🗂 Conceitos importantes

### Tudo no Linux é um arquivo 📂
- Unidades, processos e outros elementos são representados como arquivos.
- Arquivos permitem:
  - **Transparência**: acesso a informações (ex: `ls /proc` exibe processos).
  - **Interoperabilidade**: mesmas ferramentas para diferentes tipos de arquivos, podendo ser combinadas (ex: `ls -l | grep .txt`).

### Regras para nomes de arquivos
- Diferenciam letras **maiúsculas** de **minúsculas**.
- Devem ser **únicos** no diretório.
- Não devem conter `/` ou espaços.
- Extensões são **opcionais** e não necessariamente mapeadas para aplicativos.

---

## 📁 Sistema de arquivos e FHS (File System Hierarchy)

O sistema de arquivos organiza como os arquivos são armazenados no disco rígido.  
Um arquivo está localizado dentro de um diretório.

| Diretório  | Função |
|------------|--------|
| `/`        | Raiz do sistema de arquivos |
| `/boot`    | Arquivos de inicialização e kernel |
| `/dev`     | Dispositivos |
| `/etc`     | Arquivos de configuração |
| `/home`    | Diretórios base dos usuários padrão |
| `/media`   | Mídia removível |
| `/mnt`     | Unidades de rede |
| `/root`    | Diretório base do usuário root |
| `/var`     | Arquivos de log, spool de impressão, serviços de rede |

---

## 📜 Principais comandos do Linux

### `ls` — Listar arquivos e diretórios
- Lista arquivos no diretório atual ou no diretório especificado.
- Cores diferentes indicam tipos diferentes de arquivos.
- Exemplo:  
  ```bash
  ls          # Lista o diretório atual
  ls dir      # Lista o conteúdo do diretório "dir"
  ```
| Opção | Descrição                                        |
| ----- | ------------------------------------------------ |
| `-l`  | Formato longo (mostra permissões, dono, tamanho) |
| `-h`  | Tamanho em formato amigável (ex: KB, MB)         |
| `-a`  | Mostra arquivos ocultos                          |
| `-R`  | Lista subdiretórios recursivamente               |
| `-X`  | Ordena por extensão                              |
| `-S`  | Ordena por tamanho                               |
| `-t`  | Ordena por data de modificação                   |
| `-v`  | Ordena por número de versão                      |

---

### `more` — Visualizar conteúdo de arquivos
Exibe conteúdo que não cabe em uma tela (somente rolar para baixo).  

Pode ser usado com pipes, ex:  
```bash
cat file.txt | more
```
| Opção      | Descrição                      |
| ---------- | ------------------------------ |
| `-d`       | Mostra instruções de navegação |
| `-f`       | Impede quebra de linha         |
| `-p`       | Limpa a tela antes de exibir   |
| `-s`       | Comprime linhas em branco      |
| `num`      | Número de linhas a exibir      |
| `/pattern` | Busca um padrão no arquivo     |

---

### `less` — Visualizar conteúdo (avançado)
Pode rolar para cima e para baixo.  
Carrega mais rápido que `more`.  
```bash
less arquivo.txt
```
| Opção | Descrição                       |
| ----- | ------------------------------- |
| `-N`  | Mostra número das linhas        |
| `-X`  | Mantém conteúdo na tela ao sair |
| `+F`  | Observa alterações no arquivo   |

---

### `head` — Mostrar primeiras linhas de um arquivo
Exibe 10 linhas por padrão.  

Exemplo:  
```bash
head -n 20 arquivo.txt
```
| Opção      | Descrição            |
| ---------- | -------------------- |
| `-n <num>` | Quantidade de linhas |
| `-c <num>` | Quantidade de bytes  |

---

### `tail` — Mostrar últimas linhas de um arquivo
Exibe 10 linhas por padrão.  

`tail -f` monitora arquivos em tempo real (útil para logs).  
| Opção      | Descrição                   |
| ---------- | --------------------------- |
| `-n <num>` | Últimas linhas              |
| `-c <num>` | Últimos bytes               |
| `-f`       | Segue alterações no arquivo |

---

### `cp` — Copiar arquivos/diretórios
| Opção | Descrição                                   |
| ----- | ------------------------------------------- |
| `-a`  | Arquiva mantendo atributos                  |
| `-f`  | Força sobrescrita                           |
| `-i`  | Interativo (pergunta antes de sobrescrever) |
| `-n`  | Não sobrescreve                             |
| `-R`  | Cópia recursiva                             |
| `-u`  | Copia apenas se origem for mais recente     |
| `-v`  | Modo detalhado                              |

---

### `rm` — Remover arquivos/diretórios
Exemplos:  
```bash
rm arquivo.txt
rm -rf pasta/
rm *.png
```
| Opção | Descrição                   |
| ----- | --------------------------- |
| `-d`  | Remove diretório vazio      |
| `-r`  | Remove recursivamente       |
| `-f`  | Força remoção sem perguntar |
| `-i`  | Pede confirmação            |
| `-v`  | Mostra o que está removendo |

---

### `mkdir` — Criar diretórios
Exemplos:  
```bash
mkdir pasta1 pasta2 pasta3
mkdir -m 700 privada
mkdir -p /home/user/pasta/pasta2
```
| Opção       | Descrição           |
| ----------- | ------------------- |
| `-m <mask>` | Define permissões   |
| `-p`        | Cria diretórios pai |

---

### `mv` — Mover ou renomear
Exemplos:  
```bash
mv arquivo.txt pasta/
mv arquivo.txt pasta/novo_nome.txt
mv *.png imagens/
mv antigo.txt novo.txt
```
| Opção | Descrição                      |
| ----- | ------------------------------ |
| `-i`  | Pergunta antes de sobrescrever |
| `-f`  | Força sem perguntar            |
| `-n`  | Não sobrescreve                |
| `-v`  | Modo detalhado                 |

---

### `rmdir` — Remover diretórios vazios
```bash
rmdir pasta_vazia
```
Para remover diretórios com conteúdo:  
```bash
rm -r pasta
```

---

### `pwd` — Mostrar caminho absoluto do diretório atual
```bash
pwd
```
Útil para saber exatamente onde você está no sistema de arquivos.
