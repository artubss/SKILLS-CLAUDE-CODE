# Comando de Limpeza de Branches

Limpe branches mescladas e obsoletas do git

## Instruções

Siga esta abordagem sistemática para limpar branches do git: **$ARGUMENTS**

1. **Análise do Estado do Repositório**
   - Verifique o branch atual e alterações não commitadas
   - Liste todos os branches locais e remotos
   - Identifique o nome do branch main/master
   - Revise a atividade recente de branches e histórico de mesclagens

   ```bash
   # Verifique o status atual
   git status
   git branch -a
   git remote -v
   
   # Verifique o nome do branch main
   git symbolic-ref refs/remotes/origin/HEAD | sed 's@^refs/remotes/origin/@@'
   ```

2. **Precauções de Segurança**
   - Garanta que o diretório de trabalho está limpo
   - Mude para o branch main/master
   - Puxe as últimas alterações do repositório remoto
   - Crie backup do estado atual do branch se necessário

   ```bash
   # Garanta um estado limpo
   git stash push -m "Backup antes da limpeza de branches"
   git checkout main  # ou master
   git pull origin main
   ```

3. **Identifique Branches Mesclados**
   - Liste branches que foram mesclados no main
   - Exclua branches protegidos (main, master, develop)
   - Verifique branches mesclados locais e remotos
   - Valide o status de mesclagem para evitar deleção acidental

   ```bash
   # Liste branches locais mesclados
   git branch --merged main | grep -v "main\\|master\\|develop\\|\\*"
   
   # Liste branches remotos mesclados
   git branch -r --merged main | grep -v "main\\|master\\|develop\\|HEAD"
   ```

4. **Identifique Branches Obsoletos**
   - Encontre branches sem atividade recente
   - Verifique a data do último commit de cada branch
   - Identifique branches mais antigos que um período especificado (ex: 30 dias)
   - Considere padrões de nomenclatura para branches de feature/hotfix

   ```bash
   # Liste branches por data do último commit
   git for-each-ref --format='%(committerdate) %(authorname) %(refname)' --sort=committerdate refs/heads
   
   # Encontre branches mais antigos que 30 dias
   git for-each-ref --format='%(refname:short) %(committerdate)' refs/heads | awk '$2 < "'$(date -d '30 days ago' '+%Y-%m-%d')'"'
   ```

5. **Revisão Interativa de Branches**
   - Revise cada branch antes da deleção
   - Verifique se o branch possui alterações não mescladas
   - Valide o propósito e status do branch
   - Solicite confirmação antes de deletar

   ```bash
   # Verifique alterações não mescladas
   git log main..branch-name --oneline
   
   # Exiba informações do branch
   git show-branch branch-name main
   ```

6. **Configuração de Branches Protegidos**
   - Identifique branches que nunca devem ser deletados
   - Configure regras de proteção para branches importantes
   - Documente políticas de proteção de branches
   - Estabeleça proteção automatizada para novos repositórios

   ```bash
   # Exemplo de branches protegidos
   PROTECTED_BRANCHES=("main" "master" "develop" "staging" "production")
   ```

7. **Limpeza de Branches Locais**
   - Delete branches locais mesclados com segurança
   - Remova branches de feature obsoletos
   - Limpe branches de rastreamento para remotos deletados
   - Atualize referências de branches locais

   ```bash
   # Delete branches mesclados (interativo)
   git branch --merged main | grep -v "main\\|master\\|develop\\|\\*" | xargs -n 1 -p git branch -d
   
   # Force delete se necessário (use com cautela)
   git branch -D branch-name
   ```

8. **Limpeza de Branches Remotos**
   - Remova branches remotos mesclados
   - Limpe referências de rastreamento remoto
   - Delete branches remotos obsoletos
   - Atualize informações de branches remotos

   ```bash
   # Limpe branches de rastreamento remoto
   git remote prune origin
   
   # Delete branch remoto
   git push origin --delete branch-name
   
   # Remova rastreamento local de branches remotos deletados
   git branch -dr origin/branch-name
   ```

9. **Script de Limpeza Automatizada**
   
   ```bash
   #!/bin/bash
   
   # Script de limpeza de branches do git
   set -e
   
   # Configuração
   MAIN_BRANCH="main"
   PROTECTED_BRANCHES=("main" "master" "develop" "staging" "production")
   STALE_DAYS=30
   
   # Funções
   is_protected() {
       local branch=$1
       for protected in "${PROTECTED_BRANCHES[@]}"; do
           if [[ "$branch" == "$protected" ]]; then
               return 0
           fi
       done
       return 1
   }
   
   # Mude para o branch main
   git checkout $MAIN_BRANCH
   git pull origin $MAIN_BRANCH
   
   # Limpe branches mesclados
   echo "Limpando branches mesclados..."
   merged_branches=$(git branch --merged $MAIN_BRANCH | grep -v "\\*\\|$MAIN_BRANCH")
   
   for branch in $merged_branches; do
       if ! is_protected "$branch"; then
           echo "Deletando branch mesclado: $branch"
           git branch -d "$branch"
       fi
   done
   
   # Limpe branches de rastreamento remoto
   echo "Limpando branches de rastreamento remoto..."
   git remote prune origin
   
   echo "Limpeza de branches concluída!"
   ```

10. **Coordenação em Equipe**
    - Notifique a equipe antes de limpar branches compartilhados
    - Verifique se branches estão sendo usados por outros
    - Coordene agendas de limpeza de branches
    - Documente procedimentos de limpeza de branches

11. **Limpeza de Convenção de Nomenclatura de Branches**
    - Identifique branches com nomenclatura não padrão
    - Limpe branches temporários ou experimentais
    - Remova branches antigos de hotfix e feature
    - Enforce convenções de nomenclatura consistentes

12. **Verificação e Validação**
    - Verifique se branches importantes ainda estão presentes
    - Confirme que nenhum trabalho ativo foi deletado
    - Valide a sincronização de branches remotos
    - Confirme que nenhum membro da equipe teve problemas

    ```bash
    # Verifique os resultados da limpeza
    git branch -a
    git remote show origin
    ```

13. **Documentação e Relatório**
    - Documente quais branches foram limpos
    - Reporte quaisquer problemas ou conflitos encontrados
    - Atualize a documentação da equipe sobre ciclo de vida de branches
    - Crie cronograma e políticas de limpeza de branches

14. **Procedimentos de Rollback**
    - Documente como recuperar branches deletados
    - Use reflog para encontrar commits de branches deletados
    - Crie procedimentos de recuperação de emergência
    - Configure scripts de restauração de branches

    ```bash
    # Recupere branch deletado usando reflog
    git reflog --no-merges --since="2 weeks ago"
    git checkout -b recovered-branch commit-hash
    ```

15. **Configuração de Automação**
    - Configure scripts de limpeza de branches automatizados
    - Configure pipeline CI/CD para limpeza de branches
    - Crie jobs de limpeza agendados
    - Implemente políticas de ciclo de vida de branches

16. **Implementação de Melhores Práticas**
    - Estabeleça diretrizes de ciclo de vida de branches
    - Configure detecção automatizada de mesclagens
    - Configure regras de proteção de branches
    - Implemente requisitos de revisão de código

**Opções Avançadas de Limpeza:**

```bash
# Limpe todos os branches mesclados exceto os protegidos
git branch --merged main | grep -E "^  (feature|hotfix|bugfix)/" | xargs -n 1 git branch -d

# Limpeza interativa com confirmação
git branch --merged main | grep -v "main\|master\|develop" | xargs -n 1 -p git branch -d

# Delete remoto em lote
git branch -r --merged main | grep origin | grep -v "main\|master\|develop\|HEAD" | cut -d/ -f2- | xargs -n 1 git push origin --delete

# Limpe branches mais antigos que uma data específica
git for-each-ref --format='%(refname:short) %(committerdate:short)' refs/heads | awk '$2 < "2023-01-01"' | cut -d' ' -f1 | xargs -n 1 git branch -D
```

Lembre-se de:
- Sempre fazer backup de branches importantes antes da limpeza
- Coordenar com membros da equipe antes de deletar branches compartilhados
- Testar scripts de limpeza em um ambiente seguro primeiro
- Documentar todos os procedimentos e políticas de limpeza
- Estabelecer cronogramas regulares de limpeza para evitar acúmulo