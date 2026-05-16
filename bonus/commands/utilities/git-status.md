# Comando Status do Git

Mostrar status detalhado do repositório git

*Comando originalmente criado por IndyDevDan (YouTube: https://www.youtube.com/@indydevdan) / DislerH (GitHub: https://github.com/disler)*

## Instruções

Analise o estado atual do repositório git executando os seguintes passos:

1. **Execute Comandos Git Status**
   - Execute `git status` para ver o estado atual da árvore de trabalho
   - Execute `git diff HEAD origin/main` para verificar diferenças com o repositório remoto
   - Execute `git branch --show-current` para exibir o branch atual
   - Verifique se há mudanças não confirmadas e arquivos não rastreados

2. **Analise o Estado do Repositório**
   - Identifique mudanças preparadas vs não preparadas
   - Liste todos os arquivos não rastreados
   - Verifique se o branch está à frente/atrás do remoto
   - Revise conflitos de merge, se houver

3. **Leia Arquivos Principais**
   - Revise o README.md para entender o contexto do projeto
   - Verifique se há mudanças recentes em arquivos importantes
   - Compreenda a estrutura do projeto, se necessário

4. **Forneça um Resumo**
   - Branch atual e sua relação com main/master
   - Número de commits à frente/atrás
   - Lista de arquivos modificados com tipos de alteração
   - Itens de ação (commits necessários, pulls necessários, etc.)

Este comando ajuda desenvolvedores a entender rapidamente:
- Quais mudanças estão pendentes
- Status de sincronização do repositório
- Se alguma ação é necessária antes de continuar trabalhando

Argumentos: $ARGUMENTS