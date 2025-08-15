## 🐧 Linux - Comandos para Trabalhar com Arquivos

Vários comandos facilitam o trabalho com arquivos no Linux.  

### 📜 Lista de comandos

**1. `hash`** → Exibe um histórico de programas e comandos executados a partir da linha de comando.  
```bash
hash
```
**2. `cksum`** → Verifica se um arquivo não foi alterado, exibindo o checksum.  
```bash
cksum arquivo.txt
```
**3. `find`** → Pesquisa arquivos usando critérios como nome do arquivo, tamanho e proprietário.  
```bash
# Buscar por um arquivo chamado "meu_arquivo.txt" na pasta atual
find . -name "meu_arquivo.txt"

# Buscar arquivos maiores que 5MB
find . -size +5M
```
**4. `grep`** → Pesquisa um padrão de texto no conteúdo de um arquivo.  
```bash
# Procurar pela palavra "Linux" no arquivo texto.txt
grep "Linux" texto.txt
```
**5. `diff`** → Mostra rapidamente a diferença entre dois arquivos.  
```bash
diff arquivo1.txt arquivo2.txt
```
**6. `ln`** → Cria apontadores (links) para um arquivo específico.  
```bash
# Criar um link simbólico
ln -s /caminho/original.txt link.txt
```
**7. `tar`** → Compacta diversos arquivos em um único arquivo `.tar`.  
```bash
tar -cvf arquivos.tar arquivo1.txt arquivo2.txt
```
**8. `gzip`** → Compacta o tamanho de um arquivo.  
```bash
gzip arquivo.txt
```
**9. `zip`** → Compacta o conteúdo de um arquivo ou pasta.  
```bash
zip arquivos.zip arquivo1.txt arquivo2.txt
```
**10. `unzip`** → Descompacta o conteúdo de um arquivo `.zip`.  
```bash
unzip arquivos.zip
```
---

💡 **Dica:** Combine comandos com pipes (`|`) e curingas (`*`) para criar buscas e operações ainda mais poderosas.

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒

