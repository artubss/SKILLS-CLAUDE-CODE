---
allowed-tools: Bash(git:*), Read, Edit, Write
argument-hint: <nome-hotfix>
description: Criar uma nova ramificação hotfix Git Flow a partir de main para correções de produção emergenciais
---

# Ramificação Hotfix Git Flow

Criar ramificação hotfix emergencial: **$ARGUMENTS**

## Estado Atual do Repositório

- Ramificação atual: !`git branch --show-current`
- Status Git: !`git status --porcelain`
- Tag de produção mais recente: !`git describe --tags --abbrev=0 origin/main 2>/dev/null || echo "Nenhuma tag em main"`
- Status da ramificação main: !`git log main..origin/main --oneline 2>/dev/null | head -3 || echo "Sem rastreamento remoto para main"`
- Commits em main desde a última tag: !`git log $(git describe --tags --abbrev=0 origin/main 2>/dev/null)..origin/main --oneline 2>/dev/null | wc -l | tr -d ' '`

## Tarefa

Criar uma ramificação hotfix Git Flow para correções de produção emergenciais:

### 1. Validação Pré-Voo

**Verificações Críticas:**
- **Verificar nome do hotfix**: Assegurar que `$ARGUMENTS` foi fornecido e é descritivo
  - ✅ Válido: `critical-security-patch`, `payment-gateway-fix`, `auth-bypass-fix`
  - ❌ Inválido: `fix`, `hotfix1`, `bug`
- **Verificar existência de main**: Assegurar que a ramificação `main` está presente
- **Verificar mudanças não commitadas**: Diretório de trabalho limpo obrigatório
- **Confirmar status emergencial**: Hotfixes são apenas para problemas CRÍTICOS de produção

**⚠️ IMPORTANTE: Diretrizes de Uso de Hotfix**

Hotfixes são APENAS para:
- 🔒 Vulnerabilidades críticas de segurança
- 💥 Bugs que quebram a produção
- 💰 Falhas de pagamento/transação
- 🚨 Perda ou corrupção de dados
- 🔥 Indisponibilidade do sistema ou crashes

NÃO para:
- ❌ Correções de bugs regulares (use feature branch)
- ❌ Novas funcionalidades (use feature branch)
- ❌ Melhorias de performance (use feature branch)
- ❌ Problemas não críticos (aguarde o próximo lançamento)

### 2. Fluxo de Trabalho de Criação de Hotfix

```bash
# Mudar para ramificação main
git checkout main

# Fazer pull do código de produção mais recente
git pull origin main

# Criar ramificação hotfix a partir de main
git checkout -b hotfix/$ARGUMENTS

# Configurar rastreamento remoto
git push -u origin hotfix/$ARGUMENTS
```

### 3. Determinar Incremento de Versão

Analisar a tag mais recente para sugerir versão de hotfix:

```
Versão de produção atual: v1.2.0
Versão de hotfix: v1.2.1

Incremento de versão: PATCH (terceiro número incrementado)
```

**Regras de Versão de Hotfix:**
- Sempre incrementar versão PATCH (X.Y.Z → X.Y.Z+1)
- Nunca incrementar MAJOR ou MINOR para hotfixes
- Exemplos:
  - v1.2.0 → v1.2.1
  - v2.0.5 → v2.0.6
  - v1.5.9 → v1.5.10

### 4. Resposta de Sucesso

```
✓ Mudado para ramificação main
✓ Código de produção mais recente feito pull de origin/main
✓ Ramificação criada: hotfix/$ARGUMENTS
✓ Rastreamento remoto configurado: origin/hotfix/$ARGUMENTS
✓ Ramificação enviada para o remote

🔥 Ramificação Hotfix Pronta: hotfix/$ARGUMENTS

Ramificação: hotfix/$ARGUMENTS
Base: main (produção)
Será mesclada com: main E develop
Versão sugerida: v1.2.1

⚠️ FLUXO DE TRABALHO HOTFIX CRÍTICO

Este é um fix de produção EMERGENCIAL. Siga estas etapas:

1. 🔍 Identificar o Problema
   - Reproduzir o bug
   - Compreender a causa raiz
   - Documentar o impacto

2. 🛠️ Implementar a Correção
   - Fazer mudanças MÍNIMAS
   - Focar APENAS no problema crítico
   - Evitar refatoração ou melhorias
   - Adicionar testes para prevenir regressão

3. 🧪 Testar Completamente
   - Testar o fix específico
   - Executar testes de regressão completa
   - Testar em ambiente similar à produção
   - Verificar ausência de efeitos colaterais

4. 📝 Documentar a Correção
   - Atualizar versão em package.json
   - Adicionar entrada ao CHANGELOG.md
   - Documentar o bug e o fix
   - Incluir passos de reprodução

5. 🚀 Processo de Deploy
   - Criar PR para main
   - Obter revisão expedita
   - Executar /finish para mesclar e fazer tag
   - Fazer deploy para produção imediatamente
   - Monitorar problemas

🎯 Próximos Passos:
1. Corrigir o problema crítico (APENAS mudanças mínimas)
2. Testar completamente: npm test
3. Atualizar versão: v1.2.1
4. Criar PR emergencial: gh pr create --label "hotfix,critical"
5. Obter aprovação em fast-track
6. Executar /finish para mesclar com main E develop
7. Fazer deploy para produção
8. Monitorar sistemas de perto

⚠️ Lembre-se:
- Hotfix será mesclado com AMBOS main e develop
- Tag v1.2.1 será criada em main
- Deploy em produção deve acontecer imediatamente
- Equipe deve ser notificada do hotfix
```

### 5. Tratamento de Erros

**Nenhum Nome de Hotfix Fornecido:**
```
❌ Nome de hotfix é obrigatório

Uso: /hotfix <nome-hotfix>

Exemplos:
  /hotfix critical-security-patch
  /hotfix payment-processing-failure
  /hotfix auth-bypass-vulnerability

⚠️ IMPORTANTE: Hotfixes são apenas para problemas CRÍTICOS de produção!

Para fixes não críticos, use:
  /feature <nome> - Correções regulares de bugs
```

**Nome de Hotfix Inválido:**
```
❌ Nome de hotfix inválido: "fix"

Nomes de hotfix devem ser:
- Descritivos do problema
- Usar formato kebab-case
- Indicar severidade/urgência

Exemplos:
  ✅ critical-security-patch
  ✅ payment-gateway-timeout
  ✅ user-data-corruption-fix
  ❌ fix
  ❌ bug1
  ❌ hotfix
```

**Mudanças Não Commitadas:**
```
⚠️  Mudanças não commitadas detectadas no diretório de trabalho:
M  src/file.js
A  test.js

Hotfixes requerem um diretório de trabalho limpo.

Opções:
1. Fazer commit de suas mudanças primeiro
2. Guardá-las: git stash
3. Descartá-las: git checkout .

⚠️ Este é um hotfix emergencial. Por favor, limpe seu diretório de trabalho.
```

**Ramificação Main Atrás do Remote:**
```
⚠️  Local main está atrás de origin/main por 2 commits

✓ Fazendo pull do código de produção mais recente...
✓ Foram obtidos 2 commits
✓ Main agora está sincronizado com a produção
✓ Pronto para criar ramificação hotfix
```

**Não é um Problema Crítico:**
```
⚠️  Confirmação de Hotfix Requerida

Este é um problema CRÍTICO de produção que requer atenção imediata?

Problemas críticos incluem:
- Vulnerabilidades de segurança
- Falhas de sistema em produção
- Perda ou corrupção de dados
- Falhas de pagamento/transação

Se NÃO for crítico, considere:
- Criar uma feature branch em seu lugar
- Aguardar o próximo ciclo de lançamento
- Usar fluxo de trabalho regular de correção de bugs

Prosseguir com hotfix? [s/N]
```

### 6. Checklist de Hotfix

```
🔥 Checklist de Hotfix Emergencial

Identificação do Problema:
- [ ] Bug confirmado e reproduzível
- [ ] Causa raiz identificada
- [ ] Impacto documentado
- [ ] Stakeholders notificados

Desenvolvimento:
- [ ] Fix é mínimo e focado
- [ ] Nenhuma mudança desnecessária incluída
- [ ] Testes adicionados para prevenir regressão
- [ ] Código revisado (se tempo permitir)

Testes:
- [ ] Fix verificado em ambiente local
- [ ] Testes unitários passando
- [ ] Testes de integração passando
- [ ] Testado em ambiente similar à produção
- [ ] Nenhum efeito colateral detectado

Documentação:
- [ ] CHANGELOG.md atualizado
- [ ] Versão incrementada (PATCH)
- [ ] Descrição do bug documentada
- [ ] Explicação do fix documentada
- [ ] Notas de deployment preparadas

Deployment:
- [ ] PR criado com labels "hotfix" e "critical"
- [ ] Aprovação em fast-track obtida
- [ ] Plano de deployment em produção pronto
- [ ] Plano de rollback documentado
- [ ] Alertas de monitoramento configurados
- [ ] Equipe notificada do deployment

Após-Deployment:
- [ ] Fix verificado em produção
- [ ] Sistemas monitorados para problemas
- [ ] Métricas mostram melhoria
- [ ] Hotfix mesclado de volta ao develop
- [ ] Post-mortem agendado (se necessário)
```

### 7. Processo de Atualização de Versão

Após implementar a correção, atualizar a versão:

```bash
# Atualizar versão em package.json (incremento PATCH)
npm version patch --no-git-tag-version

# Atualizar CHANGELOG.md
cat >> CHANGELOG.md << EOF

## [v1.2.1] - $(date +%Y-%m-%d) - HOTFIX

### 🔥 Correções Críticas
- Fix $ARGUMENTS: [breve descrição]
  - Causa raiz: [explicação]
  - Impacto: [quem/o que foi afetado]
  - Resolução: [o que foi corrigido]

EOF

# Fazer commit do incremento de versão
git add package.json CHANGELOG.md
git commit -m "chore(hotfix): bump version to v1.2.1

Critical fix for $ARGUMENTS

🤖 Generated with Claude Code
Co-Authored-By: Claude <noreply@anthropic.com>"
```

### 8. Criar PR Emergencial

```bash
gh pr create \
  --title "🔥 HOTFIX v1.2.1: $ARGUMENTS" \
  --body "$(cat <<'EOF'
## 🔥 Hotfix Emergencial

**Severidade**: Crítica
**Versão**: v1.2.1
**Problema**: $ARGUMENTS

## Descrição do Problema

[Descrição detalhada do problema de produção]

## Causa Raiz

[Explicação do que causou o problema]

## Implementação do Fix

[Descrição da correção aplicada]

## Testes

- [x] Problema reproduzido localmente
- [x] Fix verificado localmente
- [x] Testes unitários passando
- [x] Testes de integração passando
- [x] Testado em ambiente de staging

## Plano de Deployment

1. Mesclar com main
2. Fazer tag como v1.2.1
3. Fazer deploy para produção imediatamente
4. Monitorar por 30 minutos
5. Mesclar de volta ao develop

## Plano de Rollback

[Como fazer rollback se problemas ocorrerem]

## Monitoramento

[O que monitorar após deployment]

---

**⚠️ Este é um hotfix crítico de produção que requer deployment imediato**

🤖 Generated with Claude Code
EOF
)" \
  --base main \
  --head hotfix/$ARGUMENTS \
  --label "hotfix,critical,priority-high" \
  --assignee @me \
  --reviewer team-leads
```

## Integração Git Flow

**Fluxo de Trabalho Hotfix no Git Flow:**

```
main (v1.2.0) ──────┬─────────────► (após mescla hotfix) v1.2.1
                    │
                    └─► hotfix/$ARGUMENTS
                         │
                         └─► (mescla de volta com ambos)
                             │
develop ────────────────────┴─────────────► (recebe hotfix)
```

**Importante:**
- Hotfixes ramificam de `main` (produção)
- Hotfixes mesclam com AMBOS `main` E `develop`
- Tags são criadas em `main` após mescla
- Deploy em produção acontece imediatamente

## Variáveis de Ambiente

- `GIT_FLOW_MAIN_BRANCH`: Nome da ramificação main (padrão: "main")
- `GIT_FLOW_DEVELOP_BRANCH`: Nome da ramificação develop (padrão: "develop")
- `GIT_FLOW_PREFIX_HOTFIX`: Prefixo de hotfix (padrão: "hotfix/")

## Comandos Relacionados

- `/finish` - Completar hotfix (mesclar com main e develop, criar tag, fazer deploy)
- `/flow-status` - Verificar status Git Flow atual
- `/feature <nome>` - Criar feature branch (para fixes não críticos)
- `/release <versão>` - Criar release branch

## Melhores Práticas

**FAÇA:**
- ✅ Use hotfixes APENAS para problemas críticos de produção
- ✅ Mantenha mudanças mínimas e focadas
- ✅ Teste completamente antes de fazer deploy
- ✅ Documente o problema e a correção claramente
- ✅ Notifique a equipe imediatamente
- ✅ Mescle de volta ao develop após deployment em produção
- ✅ Monitore a produção de perto após deployment
- ✅ Conduza post-mortem se apropriado

**NÃO FAÇA:**
- ❌ Use hotfix para correções regulares de bugs
- ❌ Adicione novas funcionalidades ao hotfix
- ❌ Refatore código durante hotfix
- ❌ Pule testes para economizar tempo
- ❌ Esqueça de mesclar de volta ao develop
- ❌ Faça deploy sem revisão adequada
- ❌ Pule documentação
- ❌ Ignore monitoramento após deployment

## Ações Pós-Hotfix

Após deployment bem-sucedido do hotfix:

1. **Verificar Fix em Produção**
   - Monitorar taxas de erro
   - Verificar funcionalidade afetada
   - Verificar se métricas retornam ao normal

2. **Atualizar Documentação**
   - Documentar o incidente
   - Atualizar runbooks se necessário
   - Compartilhar aprendizados com a equipe

3. **Mesclar com Develop**
   - Assegurar que hotfix está na ramificação develop
   - Resolver qualquer conflito de mescla
   - Fazer push para remote

4. **Post-Mortem (se necessário)**
   - Agendar reunião de revisão
   - Identificar medidas de prevenção
   - Atualizar processos se necessário

5. **Limpeza**
   - Deletar ramificação hotfix
   - Arquivar documentação relacionada
   - Atualizar rastreamento de incidentes