---
allowed-tools: Bash(git:*), Read
description: Exibir status abrangente do Git Flow incluindo tipo de branch, status de sincronização, alterações e destinos de merge
---

# Status do Git Flow

Exibir status abrangente do repositório Git Flow

## Estado Atual do Repositório

- Branch atual: !`git branch --show-current`
- Status Git: !`git status --porcelain`
- Lista de branches: !`git branch -a | grep -E '(feature|release|hotfix|develop|main)' | head -20`
- Tags mais recentes: !`git tag --sort=-version:refname | head -5`
- Commits recentes: !`git log --oneline --graph --all -10`
- Status remoto: !`git remote -v`

## Tarefa

Forneça um relatório abrangente de status do Git Flow:

### 1. Análise de Branch

Determine o tipo e o estado da branch atual:

```bash
CURRENT_BRANCH=$(git branch --show-current)

# Detect branch type
if [[ $CURRENT_BRANCH == "main" ]]; then
  BRANCH_TYPE="🏠 Production"
  ICON="🏠"
  STATUS_COLOR="red"
elif [[ $CURRENT_BRANCH == "develop" ]]; then
  BRANCH_TYPE="🔀 Integration"
  ICON="🔀"
  STATUS_COLOR="blue"
elif [[ $CURRENT_BRANCH == feature/* ]]; then
  BRANCH_TYPE="🌿 Feature"
  ICON="🌿"
  STATUS_COLOR="green"
elif [[ $CURRENT_BRANCH == release/* ]]; then
  BRANCH_TYPE="🚀 Release"
  ICON="🚀"
  STATUS_COLOR="yellow"
elif [[ $CURRENT_BRANCH == hotfix/* ]]; then
  BRANCH_TYPE="🔥 Hotfix"
  ICON="🔥"
  STATUS_COLOR="red"
else
  BRANCH_TYPE="📁 Other"
  ICON="📁"
  STATUS_COLOR="gray"
fi
```

### 2. Exibição de Status Abrangente

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🌿 STATUS DO GIT FLOW
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📍 BRANCH ATUAL
   $ICON $CURRENT_BRANCH
   Tipo: $BRANCH_TYPE
   Base: [origin branch]
   Destino: [merge destination]

📊 INFORMAÇÕES DO REPOSITÓRIO
   Remoto: origin ($REMOTE_URL)
   Tag mais recente: v1.2.0
   Total de branches: 12
   Features ativas: 3
   Releases ativas: 0
   Hotfixes ativos: 0

🔄 STATUS DE SINCRONIZAÇÃO
   Commits à frente: ↑ 2
   Commits atrás: ↓ 1
   Status: ⚠️  Branch divergiu do remoto

   Recomendações:
   - Puxar alterações mais recentes: git pull
   - Enviar seus commits: git push

📝 DIRETÓRIO DE TRABALHO
   Modificado: ● 3 arquivos
   Adicionado: ✚ 5 arquivos
   Deletado: ✖ 1 arquivo
   Não rastreado: ? 2 arquivos
   Total de alterações: 11 arquivos

   Status: ⚠️  Alterações não confirmadas

📈 HISTÓRICO DE COMMITS
   Commits na branch: 5
   Commits desde a base: 7
   Último commit: 2 horas atrás
   Autor: John Doe <john@example.com>

🎯 DESTINO DO MERGE
   Será feito merge em: develop
   Status de merge: ✓ Pronto (sem conflitos)

   Arquivos afetados estimados: 12
   Linhas alteradas estimadas: +245 -87

🏷️  INFORMAÇÕES DE VERSÃO
   Produção atual: v1.2.0 (em main)
   Último release: 3 dias atrás
   Próxima sugerida: v1.3.0 (baseada em commits)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### 3. Informações Específicas da Branch

**Para Feature Branches:**
```
🌿 FEATURE BRANCH: feature/user-authentication

Informações da branch:
  Criada: 2 dias atrás
  Branch base: develop
  Destino do merge: develop

Progresso:
  Commits: 5
  Arquivos alterados: 12
  Linhas adicionadas: 245
  Linhas removidas: 87

Status:
  ✓ Sem conflitos de merge com develop
  ✓ Branch atualizada com remoto
  ⚠️  3 alterações não confirmadas
  ⚠️  Testes não foram executados recentemente

Próximos passos:
  1. Confirmar suas alterações
  2. Executar testes: npm test
  3. Enviar para remoto: git push
  4. Quando pronto: /finish
```

**Para Release Branches:**
```
🚀 RELEASE BRANCH: release/v1.3.0

Informações do release:
  Versão: v1.3.0
  Criada: 1 dia atrás
  Branch base: develop
  Destinos do merge: main, develop

Conteúdo do release:
  Features: 5
  Correções de bugs: 3
  Performance: 1
  Total de commits: 15

Análise de versão:
  Atual: v1.2.0
  Proposta: v1.3.0
  Incremento: MINOR (novas features)

Checklist:
  ✓ CHANGELOG.md atualizado
  ✓ Versão em package.json
  ⚠️  Testes não foram executados
  ✗ Nenhuma tag criada ainda

Próximos passos:
  1. Executar testes finais: npm test
  2. Revisar CHANGELOG.md
  3. Criar PR: gh pr create
  4. Obter aprovações
  5. Finalizar release: /finish
```

**Para Hotfix Branches:**
```
🔥 HOTFIX BRANCH: hotfix/critical-security-patch

Informações do hotfix:
  Problema: critical-security-patch
  Criado: 2 horas atrás
  Branch base: main
  Destinos do merge: main, develop
  Severidade: CRÍTICA

Informações de versão:
  Produção atual: v1.2.0
  Versão do hotfix: v1.2.1
  Incremento: PATCH

Status:
  ✓ Correção implementada
  ✓ Testes passando
  ⚠️  Ainda não foi feito deploy
  ⚠️  2 alterações não confirmadas

⚠️  URGENTE: Este é um hotfix crítico de produção!

Próximos passos:
  1. Confirmar alterações restantes
  2. Teste final
  3. Criar PR de emergência
  4. Obter aprovação com prioridade
  5. Finalizar e fazer deploy: /finish
  6. Monitorar produção
```

**Para Main Branch:**
```
🏠 MAIN BRANCH (Produção)

Informações de produção:
  Tag mais recente: v1.2.0
  Lançada: 3 dias atrás
  Último commit: 3 dias atrás
  Status: ✓ Limpa e estável

Trabalho ativo:
  Feature branches: 3
  Release branches: 0
  Hotfix branches: 0

Releases recentes:
  v1.2.0 - 3 dias atrás
  v1.1.5 - 1 semana atrás
  v1.1.4 - 2 semanas atrás

⚠️  AVISO: Você está na branch de produção!

Evite fazer commits diretos em main.
Use branches feature/release/hotfix em seu lugar.

Para iniciar novo trabalho:
  /feature <name>    - Nova feature
  /release <version> - Novo release
  /hotfix <name>     - Correção de emergência
```

**Para Develop Branch:**
```
🔀 DEVELOP BRANCH (Integração)

Informações de integração:
  À frente de main: 12 commits
  Último merge: 1 dia atrás
  Status: ✓ Estável

Features mescladas:
  feature/user-authentication (2 dias atrás)
  feature/payment-gateway (1 semana atrás)
  feature/dashboard-redesign (2 semanas atrás)

Features ativas:
  feature/notifications (em progresso)
  feature/api-v2 (em progresso)
  feature/mobile-app (em progresso)

Próximo release:
  Versão sugerida: v1.3.0
  Features estimadas: 5
  Timeline estimada: 1 semana

Para iniciar novo trabalho:
  /feature <name> - Criar nova feature
```

### 4. Todas as Branches do Git Flow

Liste todas as branches ativas do Git Flow:

```
📋 BRANCHES ATIVAS

🌿 Features (3):
  feature/notifications        (2 commits, 1 dia antigo)
  feature/api-v2              (8 commits, 1 semana antigo)
  feature/mobile-app          (15 commits, 2 semanas antigo)

🚀 Releases (0):
  Nenhum release ativo

🔥 Hotfixes (0):
  Nenhum hotfix ativo

🏠 Branches principais:
  main    (produção, v1.2.0)
  develop (integração, +12 commits à frente)

📦 Branches obsoletas (com mais de 30 dias):
  feature/old-experiment       (45 dias antigo)
  feature/deprecated-feature   (60 dias antigo)

  Sugestão de limpeza: /clean-branches
```

### 5. Recomendações

Forneça recomendações acionáveis baseadas no status:

```
💡 RECOMENDAÇÕES

Ações Prioritárias:
  1. ⚠️  Confirmar suas 3 alterações não confirmadas
  2. ⚠️  Enviar 2 commits não enviados para remoto
  3. ⚠️  Puxar 1 commit do remoto (atrás)
  4. ℹ️  Executar testes antes de finalizar

Higiene de Branch:
  - 2 branches obsoletas podem ser deletadas
  - feature/mobile-app está com 2 semanas (considere dividir)
  - Nenhum conflito de merge detectado ✓

Próximos Passos:
  1. Confirmar alterações: git add . && git commit
  2. Puxar atualizações: git pull
  3. Enviar commits: git push
  4. Executar testes: npm test
  5. Finalizar quando pronto: /finish
```

### 6. Estados de Erro

**Não em Repositório Git:**
```
❌ Não em um repositório git

Inicializar repositório git:
  git init
  git remote add origin <url>

Ou navegue até um repositório git.
```

**Estrutura Git Flow Não Detectada:**
```
⚠️  Estrutura Git Flow não detectada

Branches faltando:
  - develop (branch de integração)
  - main (branch de produção)

Inicializar Git Flow:
  git flow init

Ou criar branches manualmente:
  git checkout -b develop
  git checkout -b main
```

**Repositório Remoto Não Configurado:**
```
⚠️  Nenhum repositório remoto configurado

Adicionar remoto:
  git remote add origin <repository-url>

Verificar remoto:
  git remote -v
```

### 7. Estatísticas Rápidas

```
📊 ESTATÍSTICAS RÁPIDAS

Commits:
  Hoje: 3
  Esta semana: 12
  Este mês: 45

Branches:
  Features: 3 ativas
  Releases: 0 ativas
  Hotfixes: 0 ativos
  Outros: 5

Colaboradores:
  Ativos esta semana: 4
  Total: 8

Repositório:
  Total de commits: 1.234
  Total de tags: 25
  Mais recente: v1.2.0
  Idade: 6 meses
```

### 8. Sugestões de Workflow

Baseado no estado atual, sugira os próximos comandos:

```
🎯 PRÓXIMOS COMANDOS SUGERIDOS

Para branch atual (feature/user-authentication):
  /finish           - Completar e fazer merge da feature
  /flow-status      - Atualizar este status

Para iniciar novo trabalho:
  /feature <name>   - Nova branch feature
  /release <version> - Novo release
  /hotfix <name>    - Correção de emergência

Manutenção do repositório:
  /clean-branches   - Limpar branches antigas
  git fetch --prune - Remover refs remotas obsoletas
```

## Comandos Relacionados

- `/feature <name>` - Criar branch feature
- `/release <version>` - Criar branch release
- `/hotfix <name>` - Criar branch hotfix
- `/finish` - Completar branch atual

## Boas Práticas

**Verificações de Status Regulares:**
- ✅ Executar /flow-status diariamente
- ✅ Verificar antes de iniciar novo trabalho
- ✅ Verificar antes de finalizar branches
- ✅ Monitorar branches obsoletas

**Indicadores de Status:**
- ✓ Verde: Pronto para prosseguir
- ⚠️ Amarelo: Atenção necessária
- ✗ Vermelho: Ação necessária
- ℹ️ Azul: Informacional