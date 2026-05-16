---
allowed-tools: Bash(git bisect:*), Bash(git log:*), Bash(git show:*), Bash(git checkout:*), Bash(npm:*), Bash(yarn:*), Bash(pnpm:*), Read, Edit, Grep
argument-hint: [good-commit] [bad-commit] | --auto [test-command] | --reset | --continue
description: Use PROACTIVELY para orientar sessões automatizadas de git bisect na busca de commits de regressão com execução inteligente de testes
---

# Assistente de Git Bisect & Automação

Sessão automatizada de git bisect para encontrar commits de regressão: $ARGUMENTS

## Estado Atual do Repositório

- Branch atual: !`git branch --show-current`
- Commits recentes: !`git log --oneline -10`
- Status git: !`git status --porcelain`
- Status de bisect: !`git bisect log 2>/dev/null || echo "Nenhuma sessão de bisect ativa"`
- Tags disponíveis: !`git tag --sort=-version:refname | head -10`

## Tarefa

Configurar e gerenciar uma sessão inteligente de git bisect para identificar o commit exato que introduziu uma regressão ou bug.

## Gerenciamento de Sessão de Bisect

### 1. Inicialização da Sessão
- Analisar histórico de commits para sugerir candidatos bom/ruim
- Configurar sessão de bisect com intervalo apropriado
- Validar que o intervalo realmente contém a regressão
- Criar branch de backup antes de iniciar bisect

### 2. Execução Automática de Testes
- Executar comando de teste especificado em cada ponto de bisect
- Interpretar resultados de teste (códigos de saída, padrões de saída)
- Marcar commits como bom/ruim automaticamente com base nos resultados
- Gerenciar setup/teardown do ambiente de teste

### 3. Suporte à Verificação Manual
- Fornecer instruções claras para teste manual em cada etapa
- Mostrar alterações relevantes no commit atual
- Guiar o usuário através do processo de decisão bom/ruim
- Manter log de bisect com raciocínio detalhado

### 4. Análise Inteligente de Commits
- Analisar mensagens de commit para palavras-chave relevantes
- Mostrar alterações de arquivo que podem estar relacionadas ao problema
- Destacar padrões suspeitos ou grandes mudanças
- Pular commits obviamente não relacionados quando possível

## Modos de Bisect

### Bisect Automático (`--auto [test-command]`)
```bash
# Automaticamente bisect usando comando de teste
/git-bisect-helper --auto "npm test"
/git-bisect-helper --auto "python -m pytest tests/test_regression.py"
/git-bisect-helper --auto "./scripts/check-performance.sh"
```

**Processo:**
1. Executar comando de teste em cada ponto de bisect
2. Marcar commit como bom (código de saída 0) ou ruim (não-zero)
3. Continuar até encontrar o commit de regressão
4. Fornecer relatório detalhado dos achados

### Bisect Guiado Manual
```bash
# Bisect interativo com orientação
/git-bisect-helper v1.2.0 HEAD
/git-bisect-helper abc123 def456
```

**Processo:**
1. Mostrar detalhes do commit atual e alterações
2. Fornecer sugestões de teste
3. Aguardar entrada do usuário (bom/ruim)
4. Continuar para o próximo ponto de bisect
5. Oferecer insights sobre o commit atual

### Continuar Sessão Existente (`--continue`)
```bash
# Retomar sessão de bisect interrompida
/git-bisect-helper --continue
```

**Processo:**
1. Analisar estado atual de bisect
2. Mostrar progresso e etapas restantes
3. Continuar com modo apropriado
4. Fornecer contexto de etapas anteriores

### Redefinir Sessão (`--reset`)
```bash
# Limpar e redefinir sessão de bisect
/git-bisect-helper --reset
```

**Processo:**
1. Encerrar sessão de bisect atual
2. Retornar para o branch original
3. Limpar arquivos temporários
4. Fornecer resumo da sessão

## Execução Inteligente de Testes

### Detecção do Ambiente de Teste
- **Node.js**: Detectar package.json e executar gerenciador de pacotes apropriado
- **Python**: Identificar requirements.txt, setup.py, pyproject.toml
- **Ruby**: Procurar Gemfile e usar bundler
- **Java**: Detectar Maven (pom.xml) ou Gradle (build.gradle)
- **Go**: Identificar go.mod e usar go test
- **Rust**: Detectar Cargo.toml e usar cargo test

### Integração com Sistema de Build
- Executar processo de build antes de testar se necessário
- Gerenciar instalação de dependências para commits antigos
- Gerenciar requisitos de variáveis de ambiente
- Pular build para commits que não compilam (marcar como ruim)

### Interpretação de Resultados de Teste
- Analisar saída de teste para padrões de erro significativos
- Distinguir entre falhas de teste e problemas de ambiente
- Gerenciar testes flaky com lógica de retry
- Fornecer níveis de confiança para decisões automatizadas

## Recursos de Análise de Commit

### Avaliação de Impacto de Mudanças
```bash
# Analisar commit atual de bisect
Arquivos alterados: !`git show --name-only --pretty="" HEAD`
Mensagem de commit: !`git log -1 --pretty=format:"%s"`
Autor e data: !`git log -1 --pretty=format:"%an (%ar)"`
```

### Detecção de Padrão de Regressão
- Identificar commits tocando áreas críticas
- Sinalizar commits com padrões suspeitos de alteração
- Destacar modificações relacionadas a performance
- Detectar mudanças de dependência ou configuração

### Preservação de Contexto
- Manter log detalhado de decisões de bisect
- Registrar raciocínio para cada marcação bom/ruim
- Salvar saídas de teste para análise posterior
- Documentar fatores ambientais

## Estratégias Avançadas de Bisect

### Estratégia de Skip para Problemas de Build
- Automaticamente pular commits que não compilam
- Gerenciar conflitos de versão de dependência
- Pular commits com problemas conhecidos de sistema de build
- Focar bisect apenas em commits funcionais

### Detecção de Regressão de Performance
- Usar benchmarks de performance em vez de testes de pass/fail
- Definir limites aceitáveis de performance
- Rastrear tendências de performance através de commits
- Identificar pontos de queda de performance

### Bisect Multi-Critério
- Testar múltiplos aspectos simultaneamente
- Gerenciar casos onde bom/ruim não é binário
- Suportar cenários complexos de regressão
- Fornecer tomada de decisão ponderada

## Relatórios de Sessão de Bisect

### Rastreamento de Progresso
```
Progresso de Bisect:
🎯 Alvo: Encontrar regressão em autenticação de usuário
📊 Commits restantes: ~4 (de 127)
⏱️  Tempo estimado: 8 minutos
🔍 Commit atual: abc123 - "refactor auth middleware"
```

### Relatório Final
```
🎉 Regressão Encontrada!

Commit Ruim: def456
Autor: João Silva
Data: 2024-01-15 14:30:00
Mensagem: "otimizar queries de banco de dados"

Arquivos Alterados:
- src/auth/database.js
- src/middleware/auth.js
- tests/auth.test.js

Log de Bisect: 15 etapas, 3 verificações manuais
Tempo Total: 12 minutos

Comandos de Recuperação:
git revert def456                    # Reverter o commit problemático
git cherry-pick def456^..def456~1    # Cherry-pick das partes boas
```

## Integração com Fluxo de Desenvolvimento

### Integração com CI/CD
- Usar mesmos comandos de teste que pipeline CI
- Respeitar variáveis de ambiente CI
- Gerenciar ambientes de teste containerizados
- Integrar com gates de qualidade existentes

### Colaboração de Equipe
- Compartilhar sessões de bisect com membros da equipe
- Documentar achados no rastreamento de issues
- Criar scripts de bisect reproduzíveis
- Estabelecer melhores práticas de bisect em equipe

### Melhoria de Debugging
- Gerar relatórios de debug para commits problemáticos
- Criar casos de reprodução mínima
- Sugerir abordagens de correção com base no tipo de regressão
- Vincular a documentação relevante ou problemas similares

## Segurança e Recuperação

### Backup de Sessão
- Criar branch de backup antes de iniciar
- Salvar posição original de HEAD
- Manter informações de recuperação
- Gerenciar sessões interrompidas graciosamente

### Tratamento de Erros
- Recuperar de estado de bisect corrompido
- Gerenciar conflitos de estado de repositório
- Gerenciar problemas de espaço em disco durante bisects longos
- Fornecer mensagens de erro claras e soluções

## Exemplo de Fluxos de Trabalho

### Regressão de Performance
```bash
# Encontrar quando testes ficaram mais lentos
/git-bisect-helper --auto "timeout 30s npm test"
```

### Regressão de Feature
```bash
# Encontrar quando feature X quebrou
/git-bisect-helper --auto "./test-feature-x.sh"
```

### Regressão de Build
```bash
# Encontrar quando build começou a falhar
/git-bisect-helper --auto "npm run build"
```

### Investigação Manual
```bash
# Bisect interativo para problemas complexos
/git-bisect-helper v2.1.0 HEAD
```

O assistente de bisect fornece automação inteligente mantendo controle total sobre o processo de debugging, tornando a caça de regressão eficiente e sistemática.