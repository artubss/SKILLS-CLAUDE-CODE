# Atualizar Nome da Branch

Siga estas etapas para atualizar o nome da branch atual:

1. Verifique as diferenças entre a branch atual e a HEAD da branch main usando `git diff main...HEAD`
2. Analise os arquivos alterados para entender qual trabalho está sendo realizado
3. Determine um nome descritivo apropriado para a branch com base nas alterações
4. Atualize o nome da branch atual usando `git branch -m [new-branch-name]`
5. Verifique se o nome da branch foi atualizado com `git branch`