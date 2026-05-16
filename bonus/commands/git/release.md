---
allowed-tools: Bash(git:*), Read, Edit, Write
argument-hint: <version>
description: Criar uma nova branch de release Git Flow a partir de develop com bump de versão e geração de changelog
---

# Branch de Release Git Flow

Criar nova branch de release: **$ARGUMENTS**

## Estado Atual do Repositório

- Branch atual: !`git branch --show-current`
- Status git: !`git status --porcelain`
- Tag mais recente: !`git describe --tags --abbrev=0 2>/dev/null || echo "No tags found"`
- Commits desde última tag: !`git log $(git describe --tags --abbrev=0 2>/dev/null)..HEAD --oneline 2>/dev/null | wc -l | tr -d ' '`
- Versão package.json: !`cat package.json 2>/dev/null | grep '"version"' | head -1 || echo "No package.json found"`
- Commits recentes: !`git log --oneline -10`

## Tarefa

Criar uma branch de release Git Flow seguindo estas etapas:

### 1. Validação de Versão

Validar o formato da versão e garantir que seja mais recente que a atual:

**Requisitos de Formato de Versão:**
- Deve seguir versionamento semântico: `vMAJOR.MINOR.PATCH`
- Exemplos: `v1.0.0`, `v2.1.3`, `v0.5.0-beta.1`
- Padrão: `v` + `NÚMERO.NÚMERO.NÚMERO` + opcional `-prerelease.NÚMERO`

**Lógica de Incremento de Versão:**

Analisar commits desde última tag para sugerir versão:
- **MAJOR** (v2.0.0): Mudanças quebradas (contém "BREAKING CHANGE:" nos commits)
- **MINOR** (v1.3.0): Novas funcionalidades (contém commits "feat:")
- **PATCH** (v1.2.1): Apenas correções de bugs (apenas commits "fix:" e "chore:")

**Análise de Versão Atual:**
```
Tag mais recente: [de git describe]
Versão sugerida: [baseada na análise de commits]
Versão fornecida: $ARGUMENTS
```

Se a versão for inválida ou não mais recente, mostrar:
```
❌ Formato de versão inválido: "$ARGUMENTS"

✅ Use versionamento semântico: vMAJOR.MINOR.PATCH

Exemplos:
  - v1.0.0 (release inicial)
  - v1.2.0 (novas funcionalidades)
  - v1.2.1 (correções de bugs)
  - v2.0.0 (mudanças quebradas)
  - v1.0.0-beta.1 (pre-release)

💡 Versão sugerida baseada em commits: v1.3.0
```

### 2. Workflow de Criação de Branch de Release

```bash
# Trocar para develop e atualizar
git checkout develop
git pull origin develop

# Criar branch de release
git checkout -b release/$ARGUMENTS

# Atualizar package.json versão (se projeto Node.js)
npm version ${ARGUMENTS#v} --no-git-tag-version

# Gerar CHANGELOG.md a partir de commits
# (analisar git log desde última tag)

# Fazer commit do bump de versão
git add package.json CHANGELOG.md
git commit -m "chore(release): bump version to ${ARGUMENTS#v}

- Updated package.json version
- Generated CHANGELOG.md from commits

🤖 Generated with Claude Code
Co-Authored-By: Claude <noreply@anthropic.com>"

# Fazer push para remote com rastreamento
git push -u origin release/$ARGUMENTS
```

### 3. Geração de CHANGELOG

Gerar changelog a partir de commits desde última tag, agrupados por tipo:

```markdown
# Changelog

## [$ARGUMENTS] - [Data Atual]

### ✨ Funcionalidades
- [Listar todos os commits feat: com links de PR]

### 🐛 Correções de Bugs
- [Listar todos os commits fix: com links de PR]

### 📝 Documentação
- [Listar todos os commits docs:]

### ♻️ Refatoração
- [Listar todos os commits refactor:]

### ⚡️ Performance
- [Listar todos os commits perf:]

### 🔒️ Segurança
- [Listar todos os commits relacionados a segurança]

### 💥 Mudanças Quebradas
- [Listar todos os commits com BREAKING CHANGE]

### 🧪 Testes
- [Listar todos os commits test:]

### 🔧 Chore
- [Listar todos os commits chore:]
```

### 4. Checklist de Release

Exibir este checklist após a criação:

```
🚀 Checklist de Release para $ARGUMENTS

Tarefas Pré-Release:
- [ ] Todos os testes passando (executar: npm test)
- [ ] Documentação atualizada
- [ ] CHANGELOG.md revisado e preciso
- [ ] Números de versão consistentes em todos os arquivos
- [ ] Sem mudanças quebradas (ou devidamente documentadas)
- [ ] Dependências atualizadas (executar: npm audit)

Tarefas de Teste:
- [ ] Testes manuais concluídos
- [ ] Testes de regressão aprovados
- [ ] Benchmarks de performance aceitáveis
- [ ] Verificação de segurança limpa (executar: npm audit)
- [ ] Testes entre navegadores (se aplicável)

Preparação para Deploy:
- [ ] Deploy em staging bem-sucedido
- [ ] Plano de deployment em produção revisado
- [ ] Plano de rollback documentado
- [ ] Monitoramento e alertas configurados

Passos Finais:
- [ ] Criar PR para main (executar: gh pr create)
- [ ] Obter aprovações necessárias (mínimo 2 revisores)
- [ ] Executar /finish para fazer merge e criar tag de release
- [ ] Anunciar release para o time

🎯 Próximos Comandos:
- Revisar CHANGELOG: cat CHANGELOG.md
- Executar testes: npm test
- Criar PR: gh pr create --base main --head release/$ARGUMENTS
- Quando pronto: /finish
```

### 5. Resposta de Sucesso

```
✓ Alternado para branch develop
✓ Puxadas as mudanças mais recentes de origin/develop
✓ Branch criada: release/$ARGUMENTS
✓ Versão package.json atualizada para ${ARGUMENTS#v}
✓ CHANGELOG.md gerado (15 commits analisados)
✓ Alterações de bump de versão feitas commit
✓ Rastreamento remoto configurado: origin/release/$ARGUMENTS
✓ Branch feita push para remote

🚀 Branch de Release Pronta: $ARGUMENTS

Branch: release/$ARGUMENTS
Base: develop
Alvo: main (após revisão)

📊 Estatísticas de Release:
  - 5 novas funcionalidades
  - 3 correções de bugs
  - 1 melhoria de performance
  - 0 mudanças quebradas
  - 2 atualizações de documentação

📝 Resumo de CHANGELOG:
  - Criado com 15 commits
  - Agrupado por tipo de commit
  - Inclui referências de PR
  - Pronto para revisão

🎯 Próximos Passos:
1. Revisar CHANGELOG.md para precisão
2. Executar testes finais: npm test
3. Testar em ambiente de staging
4. Criar PR para main: gh pr create
5. Obter aprovações do time
6. Executar /finish para completar a release

💡 Dicas de Release:
- Nenhuma nova funcionalidade deve ser adicionada à branch de release
- Apenas correções de bugs e atualizações de documentação são permitidas
- Manter branch de release de curta duração (horas, não dias)
- Tag será criada automaticamente quando feito merge para main
```

### 6. Tratamento de Erros

**Nenhuma Versão Fornecida:**
```
❌ Versão é obrigatória

Uso: /release <version>

Exemplos:
  /release v1.2.0
  /release v2.0.0-beta.1

Versão atual: v1.1.0
Versão sugerida: v1.2.0 (baseada em commits)
```

**Formato de Versão Inválido:**
```
❌ Formato de versão inválido: "1.0"

✅ Formato correto: v1.0.0 (deve começar com 'v')

Exemplos:
  ✅ v1.0.0
  ✅ v2.1.3
  ✅ v1.0.0-beta.1
  ❌ 1.0.0 (faltando 'v')
  ❌ v1.0 (incompleto)
  ❌ version-1.0.0 (formato errado)
```

**Versão Não Incrementada:**
```
❌ Versão $ARGUMENTS não é mais recente que a atual v1.2.0

💡 Bumps de versão válidos a partir de v1.2.0:
  - v1.2.1 (patch - apenas correções de bugs)
  - v1.3.0 (minor - novas funcionalidades)
  - v2.0.0 (major - mudanças quebradas)

📊 Análise de Commits:
  - 3 commits feat: → sugere bump MINOR (v1.3.0)
  - 0 BREAKING CHANGE → sem necessidade de bump MAJOR
  - 2 commits fix: → poderia usar PATCH (v1.2.1)

Recomendado: v1.3.0
```

**Mudanças Não Feitas Commit:**
```
⚠️  Mudanças não feitas commit detectadas:
M  src/feature.js
M  README.md

Antes de criar release:
1. Fazer commit de suas mudanças
2. Fazer stash delas: git stash
3. Ou descartá-las: git checkout .

Favor limpar seu diretório de trabalho primeiro.
```

**Develop Atrás do Remote:**
```
⚠️  Local develop está atrás de origin/develop por 3 commits

✓ Puxando mudanças mais recentes...
✓ Buscados 3 commits
✓ Develop agora está atualizado com remote
✓ Pronto para criar branch de release
```

## Criando Pull Request

Se CLI `gh` estiver disponível, oferecer para criar PR:

```bash
gh pr create \
  --title "Release $ARGUMENTS" \
  --body "$(cat <<'EOF'
## Resumo de Release

Versão: $ARGUMENTS
Base: develop
Alvo: main

## Mudanças Incluídas

[Auto-gerado a partir de CHANGELOG.md]

## Checklist de Release

- [ ] Todos os testes passando
- [ ] Documentação atualizada
- [ ] CHANGELOG revisado
- [ ] Sem mudanças quebradas (ou documentadas)
- [ ] Auditoria de segurança limpa
- [ ] Deploy em staging bem-sucedido

## Plano de Deployment

1. Fazer merge para main
2. Criar tag de release: $ARGUMENTS
3. Fazer deploy para produção
4. Fazer merge de volta para develop
5. Monitorar para problemas

---
🤖 Gerado com Claude Code
EOF
)" \
  --base main \
  --head release/$ARGUMENTS \
  --label "release" \
  --assignee @me
```

## Guia de Versionamento Semântico

**Versão MAJOR (X.0.0)**: Mudanças quebradas
- Alterações de API que quebram compatibilidade com versões anteriores
- Remoção de funcionalidades descontinuadas
- Mudanças arquiteturais importantes

**Versão MINOR (1.X.0)**: Novas funcionalidades
- Nova funcionalidade adicionada
- Mudanças compatíveis com versões anteriores
- Novas APIs ou métodos

**Versão PATCH (1.0.X)**: Correções de bugs
- Apenas correções de bugs
- Nenhuma nova funcionalidade
- Nenhuma mudança quebrada

## Variáveis de Ambiente

- `GIT_FLOW_DEVELOP_BRANCH`: Nome da branch develop (padrão: "develop")
- `GIT_FLOW_MAIN_BRANCH`: Nome da branch main (padrão: "main")
- `GIT_FLOW_PREFIX_RELEASE`: Prefixo de release (padrão: "release/")

## Comandos Relacionados

- `/finish` - Completar release (merge para main e develop, criar tag)
- `/flow-status` - Verificar status atual do Git Flow
- `/feature <name>` - Criar branch de feature
- `/hotfix <name>` - Criar branch de hotfix

## Melhores Práticas

**FAÇA:**
- ✅ Analisar commits para determinar o correto bump de versão
- ✅ Gerar CHANGELOG abrangente
- ✅ Testar completamente em branch de release
- ✅ Manter branch de release de curta duração
- ✅ Permitir apenas correções de bugs em branch de release
- ✅ Criar PR para revisão do time

**NÃO FAÇA:**
- ❌ Adicionar novas funcionalidades à branch de release
- ❌ Pular fase de testes
- ❌ Deixar branch de release ativa por dias
- ❌ Pular geração de CHANGELOG
- ❌ Esquecer de fazer merge de volta para develop
- ❌ Criar releases sem aprovação do time