---
allowed-tools: Bash(git branch:*), Bash(git checkout:*), Bash(git push:*), Bash(git merge:*), Bash(gh:*), Read, Grep
argument-hint: [--dry-run] | [--force] | [--remote-only] | [--local-only]
description: Use PROATIVAMENTE para limpar branches mescladas, remotos obsoletos e organizar a estrutura de branches
---

# Limpeza e Organização de Branches Git

Limpe branches mescladas e organize a estrutura do repositório: $ARGUMENTS

## Estado Atual do Repositório

- Todos os branches: !`git branch -a`
- Branches recentes: !`git for-each-ref --count=10 --sort=-committerdate refs/heads/ --format='%(refname:short) - %(committerdate:relative)'`
- Branches remotos: !`git branch -r`
- Branches mesclados: !`git branch --merged main 2>/dev/null || git branch --merged master 2>/dev/null || echo "Nenhum branch main/master encontrado"`
- Branch atual: !`git branch --show-current`

## Tarefa

Realize limpeza e organização abrangente de branches com base no estado do repositório e nos argumentos fornecidos.

## Operações de Limpeza

### 1. Identificar Branches para Limpeza
- **Branches mesclados**: Encontre branches locais já mesclados em main/master
- **Branches remotos obsoletos**: Identifique branches de rastreamento remoto que não existem mais
- **Branches antigos**: Detecte branches sem atividade recente (>30 dias)
- **Branches de feature**: Organize branches feature/*, hotfix/*, release/*

### 2. Verificações de Segurança Antes da Exclusão
- Verifique se branches foram realmente mesclados usando `git merge-base`
- Verifique se branches têm commits não enviados
- Confirme que branches não são o branch de trabalho atual
- Valide contra padrões de branches protegidos

### 3. Categorias de Branches para Manipular
- **Seguro deletar**: Branches de feature mesclados, branches antigos de hotfix
- **Requer revisão**: Branches não mesclados com commits antigos
- **Manter**: Branches principais (main, master, develop), branches de feature ativos
- **Arquivar**: Branches de longa duração que podem precisar de preservação

### 4. Sincronização de Branches Remotos
- Remova branches de rastreamento remoto de remotos deletados
- Limpe referências remotas com `git remote prune origin`
- Atualize relacionamentos de rastreamento de branches
- Limpe referências de branches remotos

## Modos de Comando

### Modo Padrão (Interativo)
1. Mostre análise de branches com recomendações
2. Peça confirmação antes de cada exclusão
3. Forneça resumo das ações realizadas
4. Ofereça envio de exclusões para o remote

### Modo Simulado (`--dry-run`)
1. Mostre o que seria deletado sem fazer mudanças
2. Exiba análise de branches e recomendações
3. Forneça estatísticas de limpeza
4. Saia sem modificar o repositório

### Modo Forçado (`--force`)
1. Delete branches mesclados sem confirmação
2. Limpe remotos obsoletos automaticamente
3. Forneça resumo de todas as ações realizadas
4. Use com cuidado — sem capacidade de desfazer

### Apenas Remote (`--remote-only`)
1. Limpe apenas branches de rastreamento remoto
2. Sincronize com o estado remoto real
3. Remova referências remotas obsoletas
4. Mantenha todos os branches locais intactos

### Apenas Local (`--local-only`)
1. Limpe apenas branches locais
2. Não afete branches de rastreamento remoto
3. Mantenha sincronização remota intacta
4. Concentre-se na organização do workspace local

## Recursos de Segurança

### Validação Pré-limpeza
- Garanta que o diretório de trabalho está limpo
- Verifique se há mudanças não confirmadas
- Verifique se o branch atual é seguro (não é alvo de exclusão)
- Crie referências de backup se solicitado

### Branches Protegidos
Nunca delete branches que correspondem a esses padrões:
- `main`, `master`, `develop`, `staging`, `production`
- `release/*` (a menos que explicitamente confirmado)
- Branch de trabalho atual
- Branches com commits não enviados (a menos que forçado)

### Informações de Recuperação
- Exiba referências de reflog git para branches deletados
- Forneça comandos para recuperar branches acidentalmente deletados
- Mostre hashes SHA para pontas de branches antes da exclusão
- Crie script de recuperação se vários branches foram deletados

## Recursos de Organização de Branches

### Aplicação de Convenção de Nomenclatura
- Sugira renomear branches para seguir convenções da equipe
- Organize branches por tipo (feature/, bugfix/, hotfix/)
- Identifique branches que não seguem padrões de nomenclatura
- Forneça sugestões de renomeação em lote

### Configuração de Rastreamento de Branches
- Configure rastreamento upstream apropriado para feature branches
- Configure comportamento de push/pull para novos branches
- Identifique branches com configuração upstream ausente
- Corrija relacionamentos de rastreamento quebrados

## Saída e Relatórios

### Resumo de Limpeza
```
Resumo de Limpeza de Branches:
✅ Deletados 3 branches de feature mesclados
✅ Removidas 5 referências remotas obsoletas
✅ Limpas 2 branches antigos de hotfix
⚠️  Encontrado 1 branch não mesclado que requer atenção
📊 Repositório agora tem 8 branches ativos (eram 18)
```

### Instruções de Recuperação
```
Comandos de Recuperação de Branches:
git checkout -b feature/user-auth 1a2b3c4d  # Recuperar feature/user-auth
git push origin feature/user-auth            # Restaurar para remote
```

## Melhores Práticas

### Cronograma de Manutenção Regular
- Execute limpeza semanalmente em repositórios ativos
- Use `--dry-run` primeiro para revisar mudanças
- Coordene com a equipe antes de limpezas grandes
- Documente qualquer branch não padrão para preservar

### Coordenação em Equipe
- Comunique planos de exclusão de branches com a equipe
- Verifique se alguém tem trabalho em progresso em branches antigos
- Use regras de proteção de branches do GitHub/GitLab
- Mantenha documentação compartilhada de políticas de branches

### Gerenciamento de Ciclo de Vida de Branches
- Delete branches de feature imediatamente após mesclagem
- Mantenha branches de release até próxima release grande
- Archive branches experimentais de longa duração
- Use tags para marcar estados importantes de branches antes da exclusão

## Exemplo de Uso

```bash
# Limpeza interativa segura
/branch-cleanup

# Veja o que seria limpo sem fazer mudanças
/branch-cleanup --dry-run

# Limpe apenas branches de rastreamento remoto
/branch-cleanup --remote-only

# Force limpeza de branches mesclados
/branch-cleanup --force

# Limpe apenas branches locais
/branch-cleanup --local-only
```

## Integração com GitHub/GitLab

Se GitHub CLI ou GitLab CLI estiverem disponíveis:
- Verifique status de PR antes de deletar branches
- Valide se branches foram realmente mesclados na interface web
- Limpe branches locais e remotos consistentemente
- Atualize regras de proteção de branches se necessário