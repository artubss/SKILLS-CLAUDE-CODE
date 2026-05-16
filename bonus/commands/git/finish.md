---
allowed-tools: Bash(git:*), Read, Edit
argument-hint: [--no-delete] [--no-tag]
description: Conclua e mescle a branch atual do Git Flow (feature/release/hotfix) com limpeza e tagging apropriados
---

# Git Flow Finish Branch

Conclua a branch atual do Git Flow: **$ARGUMENTS**

## Estado Atual do Repositório

- Branch atual: !`git branch --show-current`
- Tipo de branch: !`git branch --show-current | grep -oE '^(feature|release|hotfix)' || echo "Não é uma branch Git Flow"`
- Status Git: !`git status --porcelain`
- Commits não enviados: !`git log @{u}.. --oneline 2>/dev/null | wc -l | tr -d ' '`
- Tag mais recente: !`git describe --tags --abbrev=0 2>/dev/null || echo "Sem tags"`
- Status de testes: !`npm test 2>/dev/null | tail -20 || echo "Sem comando de teste disponível"`

## Tarefa

Conclua a branch atual do Git Flow mesclando-a à(s) branch(es) alvo apropriada(s):

### 1. Detecção do Tipo de Branch

Detecte o tipo de branch atual e determine a estratégia de merge:

```bash
CURRENT_BRANCH=$(git branch --show-current)

if [[ $CURRENT_BRANCH == feature/* ]]; then
  BRANCH_TYPE="feature"
  MERGE_TO="develop"
  CREATE_TAG="no"
elif [[ $CURRENT_BRANCH == release/* ]]; then
  BRANCH_TYPE="release"
  MERGE_TO="main develop"
  CREATE_TAG="yes"
  TAG_NAME="${CURRENT_BRANCH#release/}"
elif [[ $CURRENT_BRANCH == hotfix/* ]]; then
  BRANCH_TYPE="hotfix"
  MERGE_TO="main develop"
  CREATE_TAG="yes"
  # Increment patch version from current tag
  CURRENT_TAG=$(git describe --tags --abbrev=0 origin/main 2>/dev/null)
  TAG_NAME="${CURRENT_TAG%.*}.$((${CURRENT_TAG##*.} + 1))"
else
  echo "❌ Não está em uma branch Git Flow (feature/release/hotfix)"
  exit 1
fi
```

### 2. Validação Pré-Merge

Antes de mesclar, valide essas condições:

**Verificações Críticas:**
- ✅ Todas as alterações estão commitadas (sem arquivos não commitados)
- ✅ Todos os commits foram enviados ao remote
- ✅ Testes estão passando (execute o conjunto de testes)
- ✅ Sem conflitos de merge com a branch alvo
- ✅ Branch está atualizada com o remote

```
🔍 Validação Pré-Merge

✓ Diretório de trabalho limpo
✓ Todos os commits enviados ao remote
✓ Executando testes...
  ├─ Testes unitários: 45/45 passou
  ├─ Testes de integração: 12/12 passou
  └─ Todos os testes passaram ✓

✓ Verificando conflitos de merge com develop...
  └─ Sem conflitos detectados ✓

✓ Branch está atualizada com o remote ✓

Pronto para mesclar!
```

### 3. Conclusão de Branch Feature

Para branches **feature/**:

```bash
# Certifique-se de que todos os commits foram enviados
git push

# Mude para develop
git checkout develop

# Puxe as alterações mais recentes
git pull origin develop

# Mescle a branch feature (sem fast-forward)
git merge --no-ff feature/$NAME -m "Merge feature/$NAME into develop

$(git log develop..feature/$NAME --oneline)

🤖 Generated with Claude Code
Co-Authored-By: Claude <noreply@anthropic.com>"

# Envie para o remote
git push origin develop

# Delete local branch (unless --no-delete)
git branch -d feature/$NAME

# Delete remote branch (unless --no-delete)
git push origin --delete feature/$NAME
```

**Resposta de Sucesso:**
```
✓ Todos os commits enviados ao remote
✓ Mudou para develop
✓ Puxou as alterações mais recentes
✓ Mesclou feature/$NAME em develop
✓ Enviado para origin/develop
✓ Branch local deletada: feature/$NAME
✓ Branch remote deletada: origin/feature/$NAME

🌿 Feature Concluída!

Mesclada: feature/$NAME
Alvo: develop
Commits inclusos: 5
Arquivos alterados: 12

🎉 Sua feature está agora na branch develop!

Próximos passos:
- Feature será incluída na próxima release
- Outros membros da equipe podem puxar de develop
- Você pode iniciar uma nova branch feature
```

### 4. Conclusão de Branch Release

Para branches **release/**:

```bash
# Extraia a versão do nome da branch
VERSION="${CURRENT_BRANCH#release/}"

# Certifique-se de que todos os commits foram enviados
git push

# Mude para main
git checkout main
git pull origin main
git merge --no-ff release/$VERSION -m "Merge release/$VERSION into main

Release notes:
$(cat CHANGELOG.md | sed -n "/## \[$VERSION\]/,/## \[/p" | head -n -1)

🤖 Generated with Claude Code
Co-Authored-By: Claude <noreply@anthropic.com>"

# Crie tag em main (unless --no-tag)
git tag -a $VERSION -m "Release $VERSION

$(cat CHANGELOG.md | sed -n "/## \[$VERSION\]/,/## \[/p" | head -n -1)"

# Envie main com tags
git push origin main --tags

# Mescle de volta para develop
git checkout develop
git pull origin develop
git merge --no-ff release/$VERSION -m "Merge release/$VERSION back into develop

🤖 Generated with Claude Code
Co-Authored-By: Claude <noreply@anthropic.com>"

# Envie develop
git push origin develop

# Delete branches (unless --no-delete)
git branch -d release/$VERSION
git push origin --delete release/$VERSION
```

**Resposta de Sucesso:**
```
✓ Todos os commits enviados ao remote
✓ Mesclou release/$VERSION em main
✓ Tag criada: $VERSION
✓ Main enviado com tags
✓ Mesclou release/$VERSION em develop
✓ Enviado para origin/develop
✓ Branch local deletada: release/$VERSION
✓ Branch remote deletada: origin/release/$VERSION

🚀 Release Concluída: $VERSION

Mesclada para: main, develop
Tag criada: $VERSION
Commits inclusos: 15
Alterações:
  - 5 features
  - 3 correções de bugs
  - 2 melhorias de performance

🎉 A Release $VERSION está agora em produção!

Próximos passos:
- Faça deploy para produção: [comando de deployment]
- Monitore a produção para problemas
- Anuncie o lançamento à equipe
- Atualize a documentação se necessário

Detalhes da tag:
  git show $VERSION
```

### 5. Conclusão de Branch Hotfix

Para branches **hotfix/**:

```bash
# Determine nova versão (bump de patch)
CURRENT_VERSION=$(git describe --tags --abbrev=0 origin/main)
NEW_VERSION="${CURRENT_VERSION%.*}.$((${CURRENT_VERSION##*.} + 1))"

# Certifique-se de que todos os commits foram enviados
git push

# Mude para main
git checkout main
git pull origin main
git merge --no-ff hotfix/$NAME -m "Merge hotfix/$NAME into main

Critical fix for: $NAME

🤖 Generated with Claude Code
Co-Authored-By: Claude <noreply@anthropic.com>"

# Crie tag em main (unless --no-tag)
git tag -a $NEW_VERSION -m "Hotfix $NEW_VERSION: $NAME

Critical production fix"

# Envie main com tags
git push origin main --tags

# Mescle de volta para develop
git checkout develop
git pull origin develop
git merge --no-ff hotfix/$NAME -m "Merge hotfix/$NAME back into develop

🤖 Generated with Claude Code
Co-Authored-By: Claude <noreply@anthropic.com>"

# Envie develop
git push origin develop

# Delete branches (unless --no-delete)
git branch -d hotfix/$NAME
git push origin --delete hotfix/$NAME
```

**Resposta de Sucesso:**
```
✓ Todos os commits enviados ao remote
✓ Mesclou hotfix/$NAME em main
✓ Tag criada: $NEW_VERSION (bump de patch)
✓ Main enviado com tags
✓ Mesclou hotfix/$NAME em develop
✓ Enviado para origin/develop
✓ Branch local deletada: hotfix/$NAME
✓ Branch remote deletada: origin/hotfix/$NAME

🔥 Hotfix Concluído: $NEW_VERSION

Mesclado para: main, develop
Tag criada: $NEW_VERSION
Problema corrigido: $NAME
Versão anterior: $CURRENT_VERSION

⚠️ CRÍTICO: Faça deploy para produção imediatamente!

Próximos passos:
1. Faça deploy de $NEW_VERSION para produção AGORA
2. Monitore os sistemas de produção atentamente
3. Verifique se a correção está funcionando
4. Notifique a equipe sobre o deployment do hotfix
5. Atualize a documentação de incidentes

Comando de deployment:
  [seu comando de deployment aqui]

Monitore:
  - Taxas de erro
  - Métricas do sistema
  - Relatórios de usuários
```

### 6. Tratamento de Erros

**Não está em uma Branch Git Flow:**
```
❌ Não está em uma branch Git Flow

Branch atual: $CURRENT_BRANCH

/finish funciona apenas em:
- branches feature/*
- branches release/*
- branches hotfix/*

Para concluir esta branch manualmente:
1. Mude para a branch alvo
2. Mescle manualmente: git merge $CURRENT_BRANCH
3. Envie: git push
```

**Alterações Não Commitadas:**
```
❌ Não é possível concluir: Alterações não commitadas detectadas

Arquivos modificados:
M  src/file1.js
M  src/file2.js

Por favor, commite ou guarde suas alterações primeiro:
1. Commitar: git add . && git commit
2. Guardar: git stash
3. Descartar: git checkout .
```

**Commits Não Enviados:**
```
⚠️  Aviso: 3 commits não enviados detectados

Commits não estão no remote:
  abc1234 feat: add new feature
  def5678 fix: resolve bug
  ghi9012 docs: update README

Gostaria de enviar agora? [S/n]
✓ Enviando commits...
✓ Todos os commits foram enviados ao remote
```

**Testes Falhando:**
```
❌ Não é possível concluir: Testes estão falhando

Testes falhando:
  ✗ UserService.test.js
    - should authenticate user (expected 200, got 401)
  ✗ PaymentController.test.js
    - should process payment (timeout)

Corrija os testes falhando antes de concluir:
1. Executar testes: npm test
2. Corrigir falhas
3. Commitar correções
4. Tentar /finish novamente

Pular testes? (NÃO RECOMENDADO) [s/N]
```

**Conflitos de Merge:**
```
❌ Conflito de merge detectado com develop

Arquivos em conflito:
  src/config.js
  package.json

Passos de resolução:
1. Buscar develop mais recente: git fetch origin develop
2. Tentar merge localmente: git merge origin/develop
3. Resolver conflitos manualmente
4. Commitar a resolução
5. Tentar /finish novamente

Gostaria de ver os detalhes do conflito? [S/n]
```

**Tag Faltando para Release:**
```
⚠️  Branch de release sem versão no CHANGELOG

Formato esperado em CHANGELOG.md:
## [v1.2.0] - 2025-10-01

CHANGELOG atual:
[mostrar seção relevante]

Por favor, atualize CHANGELOG.md com a versão de release.
Continuar mesmo assim? [s/N]
```

### 7. Argumentos

**--no-delete**: Mantenha a branch após mesclar
```bash
/finish --no-delete

# Mescla mas mantém branches local e remote
```

**--no-tag**: Pule a criação de tag (apenas release/hotfix)
```bash
/finish --no-tag

# Mescla mas não cria tag de versão
```

### 8. Confirmação Interativa

Para operações destrutivas, peça confirmação:

```
🔍 Resumo de Conclusão

Branch: release/v1.2.0
Tipo: Release
Será mesclada para: main, develop
Será criada tag: v1.2.0
Será deletada: Branches local e remote

Ações a serem executadas:
  1. Mesclar para main
  2. Criar tag v1.2.0 em main
  3. Enviar main com tags
  4. Mesclar para develop
  5. Enviar develop
  6. Deletar release/v1.2.0 (local)
  7. Deletar origin/release/v1.2.0 (remote)

Prosseguir com a conclusão? [S/n]
```

### 9. Checklist Pós-Conclusão

**Para Features:**
```
✅ Checklist de Feature Concluída

- [x] Mesclada em develop
- [x] Branch remote deletada
- [x] Branch local deletada

O que fazer agora:
- Feature está agora em develop
- Será incluída na próxima release
- Equipe pode puxar de develop
- Você pode iniciar nova feature

Iniciar nova feature:
  /feature <name>
```

**Para Releases:**
```
✅ Checklist de Release Concluída

- [x] Mesclada em main
- [x] Mesclada em develop
- [x] Tag criada: v1.2.0
- [x] Branches deletadas

Checklist de deployment:
- [ ] Fazer deploy para produção
- [ ] Verificar deployment
- [ ] Monitorar problemas
- [ ] Anunciar release
- [ ] Atualizar documentação

Comando de deployment:
  [seu comando de deployment]
```

**Para Hotfixes:**
```
✅ Checklist de Hotfix Concluído

- [x] Mesclado em main
- [x] Mesclado em develop
- [x] Tag criada: v1.2.1
- [x] Branches deletadas

🚨 AÇÕES IMEDIATAS NECESSÁRIAS:
- [ ] Fazer deploy para produção AGORA
- [ ] Monitorar sistemas de produção
- [ ] Verificar se a correção está funcionando
- [ ] Notificar a equipe
- [ ] Atualizar documentação de incidentes

Este foi um hotfix de emergência - deploy em produção é CRÍTICO!
```

## Variáveis de Ambiente

- `GIT_FLOW_MAIN_BRANCH`: Branch principal (padrão: "main")
- `GIT_FLOW_DEVELOP_BRANCH`: Branch develop (padrão: "develop")

## Comandos Relacionados

- `/feature <name>` - Inicie uma nova branch feature
- `/release <version>` - Inicie uma nova branch release
- `/hotfix <name>` - Inicie uma nova branch hotfix
- `/flow-status` - Verifique o status do Git Flow

## Melhores Práticas

**FAÇA:**
- ✅ Execute testes antes de concluir
- ✅ Certifique-se de que todos os commits foram enviados
- ✅ Revise as alterações uma última vez
- ✅ Atualize CHANGELOG para releases
- ✅ Crie tags para releases/hotfixes
- ✅ Mescle para todas as branches necessárias
- ✅ Limpe as branches após o merge

**NÃO FAÇA:**
- ❌ Conclua com testes falhando
- ❌ Pule o envio de commits
- ❌ Esqueça de mesclar para develop
- ❌ Deixe branches sem deletar
- ❌ Pule tags para releases
- ❌ Force push após o merge