---
allowed-tools: Bash(git:*)
argument-hint: <nome-da-feature>
description: Criar uma nova branch de feature Git Flow a partir de develop com nomenclatura e rastreamento adequados
---

# Branch de Feature Git Flow

Criar nova branch de feature: **$ARGUMENTS**

## Estado Atual do Repositório

- Branch atual: !`git branch --show-current`
- Status git: !`git status --porcelain`
- Status da branch develop: !`git log develop..origin/develop --oneline 2>/dev/null | head -5 || echo "Sem rastreamento remoto para develop"`

## Tarefa

Criar uma branch de feature Git Flow seguindo estas etapas:

### 1. Validação Pré-Voo

- **Verificar repositório git**: Confirmar que estamos em um repositório git válido
- **Validar nome da feature**: Garantir que `$ARGUMENTS` foi fornecido e segue as convenções de nomenclatura:
  - ✅ Válido: `user-authentication`, `payment-integration`, `dashboard-redesign`
  - ❌ Inválido: `feat1`, `My_Feature`, nome vazio
- **Verificar mudanças não commitadas**:
  - Se existem mudanças, avisar o usuário e solicitar commit/stash primeiro
  - OU oferecer para fazer stash automaticamente
- **Verificar existência da branch develop**: Garantir que a branch `develop` está presente

### 2. Criar Branch de Feature

Executar o seguinte workflow:

```bash
# Trocar para a branch develop
git checkout develop

# Fazer pull das últimas mudanças do remoto
git pull origin develop

# Criar branch de feature com convenção Git Flow
git checkout -b feature/$ARGUMENTS

# Configurar rastreamento remoto
git push -u origin feature/$ARGUMENTS
```

### 3. Fornecer Relatório de Status

Após criação bem-sucedida, exibir:

```
✓ Trocado para a branch develop
✓ Feito pull das últimas mudanças de origin/develop
✓ Branch criada: feature/$ARGUMENTS
✓ Rastreamento remoto configurado: origin/feature/$ARGUMENTS
✓ Branch enviada para o remoto

🌿 Branch de Feature Pronta

Branch: feature/$ARGUMENTS
Base: develop
Status: Diretório de trabalho limpo

🎯 Próximos Passos:
1. Comece a implementar sua feature
2. Faça commits usando formato convencional:
   git commit -m "feat: suas mudanças"
3. Envie mudanças regularmente: git push
4. Quando completo, use /finish para fazer merge de volta para develop

💡 Dicas Git Flow:
- Mantenha commits atômicos e bem descritos
- Faça push frequentemente para evitar conflitos
- Use formato de commit convencional (feat:, fix:, etc.)
- Teste completamente antes de finalizar
```

### 4. Tratamento de Erros

Lidar com estes cenários graciosamente:

**Mudanças Não Commitadas:**
```
⚠️  Você tem mudanças não commitadas:
M  src/file1.js
M  src/file2.js

Opções:
1. Fazer commit das mudanças primeiro
2. Fazer stash das mudanças: git stash
3. Descartar mudanças: git checkout .

O que você gostaria de fazer? [1/2/3]
```

**Nome da Feature Não Fornecido:**
```
❌ Nome da feature é obrigatório

Uso: /feature <nome-da-feature>

Exemplos:
  /feature user-profile-page
  /feature api-v2-integration
  /feature payment-gateway

Nomes de features devem:
- Ser descritivos e concisos
- Usar kebab-case (minúsculas-com-hífens)
- Descrever o que a feature faz
```

**Branch Já Existe:**
```
❌ Branch feature/$ARGUMENTS já existe

Branches de feature existentes:
  feature/user-authentication
  feature/payment-gateway
  feature/$ARGUMENTS ← Esta

Opções:
1. Trocar para branch existente: git checkout feature/$ARGUMENTS
2. Usar um nome de feature diferente
3. Deletar existente e recriar (destrutivo!)
```

**Develop Atrás do Remoto:**
```
⚠️  Local develop está 5 commits atrás de origin/develop

✓ Fazendo pull das últimas mudanças...
✓ Develop agora está atualizado
✓ Pronto para criar branch de feature
```

**Nenhuma Branch Develop:**
```
❌ Branch develop não encontrada

Git Flow requer uma branch 'develop'. Crie-a com:
  git checkout -b develop
  git push -u origin develop

Ou inicialize Git Flow:
  git flow init
```

## Contexto Git Flow

Este comando faz parte da estratégia de branching Git Flow:

- **main**: Código pronto para produção (protegida)
- **develop**: Branch de integração para features (protegida)
- **feature/***: Novas features (você está aqui)
- **release/***: Preparação de release
- **hotfix/***: Correções emergenciais de produção

Branches de feature:
- Branch a partir de: `develop`
- Fazer merge de volta para: `develop`
- Convenção de nomenclatura: `feature/<nome-descritivo>`
- Ciclo de vida: Curto a médio prazo

## Variáveis de Ambiente

Este comando respeita:
- `GIT_FLOW_DEVELOP_BRANCH`: Nome da branch develop (padrão: "develop")
- `GIT_FLOW_PREFIX_FEATURE`: Prefixo de feature (padrão: "feature/")

## Comandos Relacionados

- `/finish` - Completar e fazer merge da branch de feature para develop
- `/flow-status` - Verificar status atual do Git Flow
- `/release <versão>` - Criar branch de release a partir de develop
- `/hotfix <nome>` - Criar branch de hotfix a partir de main

## Boas Práticas

**FAÇA:**
- ✅ Use nomes de feature descritivos
- ✅ Mantenha o escopo da feature focado e pequeno
- ✅ Faça push para o remoto regularmente
- ✅ Teste suas mudanças antes de finalizar
- ✅ Use mensagens de commit convencionais

**NÃO FAÇA:**
- ❌ Criar features diretamente a partir de main
- ❌ Usar nomes genéricos como "feature1"
- ❌ Deixar branches de feature vivas por muito tempo
- ❌ Misturar múltiplas features não relacionadas
- ❌ Pular testes antes de fazer merge