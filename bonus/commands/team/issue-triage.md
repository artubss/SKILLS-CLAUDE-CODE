---
allowed-tools: Read, Write, Bash
argument-hint: [escopo] | --github-issues | --linear-tasks | --priority-analysis | --team-assignment
description: Triagem inteligente de issues com categorização automática, priorização e atribuição de equipe
---

# Triagem de Issues

Realize triagem e priorização inteligente de issues com roteamento e atribuição de equipe automatizados: **$ARGUMENTS**

## Contexto de Triagem Atual

- Repositório: !`gh repo view --json nameWithOwner -q .nameWithOwner 2>/dev/null || echo "No repo context"`
- Issues abertas: !`gh issue list --state open --limit 1 --json number | jq length 2>/dev/null || echo "Check manually"`
- Times Linear: Times Linear disponíveis e atribuições de projetos para roteamento
- Backlog de triagem: Volume atual e idade de issues não triadas

## Tarefa

Execute análise inteligente de issues com triagem automatizada e atribuição de prioridades:

**Escopo de Triagem**: Use $ARGUMENTS para focar em issues do GitHub, tarefas Linear, análise de prioridades ou otimização de atribuição de equipe

**Framework de Triagem**:
1. **Análise de Issues** - Extrair metadados de issues, analisar padrões de conteúdo, avaliar indicadores de severidade, avaliar escopo de impacto
2. **Classificação de Categorias** - Identificar tipo de issue (bug, feature, documentação), avaliar nível de complexidade, determinar fatores de urgência
3. **Avaliação de Prioridade** - Calcular pontuação de prioridade usando métricas de severidade, impacto, esforço e valor de negócio
4. **Roteamento de Equipe** - Corresponder skills de issue à expertise de equipe, balancear distribuição de carga de trabalho, considerar capacidade do sprint atual
5. **Gerenciamento de Labels** - Aplicar esquema de labels consistente, manter padrões de taxonomia, habilitar filtragem e relatórios
6. **Atribuição de SLA** - Definir expectativas de tempo de resposta, estabelecer metas de resolução, rastrear métricas de desempenho

**Recursos Avançados**: Detecção automática de severidade, correspondência inteligente de equipe, balanceamento de carga de trabalho, monitoramento de SLA, workflows de escalação.

**Garantia de Qualidade**: Validação de consistência, rastreamento de acurácia de triagem, monitoramento de satisfação da equipe, feedback de otimização de processo.

**Output**: Triagem completa de issues com atribuições de prioridade, recomendações de roteamento de equipe, metas de SLA e insights de melhoria de processo.