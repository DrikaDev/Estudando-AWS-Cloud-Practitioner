## 🐧 Sistema de Arquivos no Linux: criação de estrutura de pastas

### 🎯 Objetivos do laboratório
- Criar uma estrutura de pastas fornecida pelo exercício.
- Criar arquivos.
- Copiar e mover arquivos e diretórios.
- Excluir arquivos e diretórios.

---

### 📝 Tarefa 1: Conectar à instância via SSH

Conecte-se à sua instância Linux usando o comando `ssh` no terminal.

---

### 📝 Tarefa 2: Criar uma estrutura de pastas

Usando o terminal, recrie a seguinte estrutura na máquina Linux.  
<img width="500" height="232" alt="image" src="https://github.com/user-attachments/assets/44749aed-23c8-476f-bb60-e83b2efe8790" />
<img width="377" height="288" alt="image" src="https://github.com/user-attachments/assets/3ce7304f-0cb7-475d-aea8-58a27f58f7ec" />

1. Confirme que você está na pasta inicial do usuário atual com **pwd**  
2. No terminal, digite **mkdir CompanyA**  
3. Mude de diretório, digite **cd CompanyA**  
4. Crie todas as subpastas, digite **mkdir Finance HR Management**  
5. Verifique se as pastas foram criadas, digite **ls**  
6. Passe seu diretório atual para o diretório **RH**, digite **cd HR**  
7. Crie os arquivos vazios dentro da pasta **RH**, digite **touch Assessments.csv TrialPeriod.csv**  
8. Verifique se os arquivos foram criados, digite **ls**  
9. Passe seu diretório atual para o diretório **Finance**, digite **cd ../Finance**  
10. Crie os arquivos vazios dentro da pasta **Finance**, digite **touch Salary.csv ProfitAndLossStatements.csv**  
11. Verifique se os arquivos foram criados, digite **ls**  
12. Suba um nível de diretório, até a pasta **CompanyA**, digite **cd ..**  
13. Crie os novos arquivos vazios na pasta **Management**, digite **touch Management/Managers.csv Management/Schedule.csv**  
14. Verifique se os arquivos foram criados, digite **ls Management**  
15. Verifique se todos os arquivos e pastas da pasta **CompanyA** foram criados, digite **ls -laR**

<img width="834" height="919" alt="image" src="https://github.com/user-attachments/assets/a4dd3c90-461d-4a72-b15b-2b4942eee4fa" />

---

## 📝 Tarefa 3: Excluir e reorganizar pastas

1. Digite **pwd** para ter certeza de que está na pasta **CompanyA**  
2. Copie a pasta **Finance** e seu conteúdo para a pasta **HR** com o comando **cp -r Finance HR**  
3. Verifique se a pasta e o conteúdo foram copiados com **ls HR/Finance**  
4. Remova a pasta **Finance** da pasta **CompanyA**, digite **rmdir Finance**  
  
  > Observação:
  > rmdir só funciona sobre um diretório vazio.
  > Para remover a pasta, existem duas opções:

5. Remova os arquivos dentro da pasta **Finance**, digite **rm Finance/ProfitAndLossStatements.csv Finance/Salary.csv**  
6. Verifique se a pasta está vazia, digite **ls Finance**  
7. Remova a pasta, digite **rmdir Finance**  
8. Verifique se a pasta foi removida, digite **ls**  
9. Mover a pasta **Management** para dentro da pasta **HR**, digite **mv Management HR**  
10. Verifique se a pasta e os arquivos foram movidos, digite **ls . HR/Management**
11. Navegue dentro da pasta **RH**, digite **cd HR**  
12. Crie a pasta **Employees**, digite **mkdir Employees**  
13. Mova os arquivos **Assessments.csv e TrialPeriod.csv** para dentro da pasta **Employees**, digite **mv Assessments.csv TrialPeriod.csv Employees**  
14. Verificar se os arquivos foram movidos, digite **ls . Employees**

---

👉🏻 [Clique aqui para voltar ao Readme](https://github.com/DrikaDev/Estudando-AWS-Cloud-Practitioner/blob/main/README.md) 📒
