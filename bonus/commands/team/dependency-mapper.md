---
allowed-tools: Read, Glob, Grep, Bash
argument-hint: [escopo] | --tasks | --code | --circular | --critical-path
description: Mapear dependências de projeto e tarefas com análise de caminho crítico e detecção de dependências circulares
---

# Mapeador de Dependências

Mapear e analisar dependências de projeto com otimização de ordenação de tarefas: **$ARGUMENTS**

## Contexto de Dependência Atual

- Repositório: !`gh repo view --json nameWithOwner -q .nameWithOwner 2>/dev/null || echo "No repo context"`
- Arquivos de projeto: !`find . -name "*.js" -o -name "*.ts" -o -name "*.py" | wc -l` arquivos de código analisados
- Rastreamento de tarefas: Conectividade do servidor MCP Linear e dados de relacionamento de tarefas
- Análise de importações: Estrutura de dependência de código e detecção de dependências circulares

## Tarefa

Executar análise abrangente de dependências com recomendações de otimização:

**Escopo de Análise**: Use $ARGUMENTS para focar em dependências de tarefas, dependências de código, detecção de dependências circulares ou análise de caminho crítico

**Framework de Análise de Dependências**:
1. **Mapeamento de Dependência de Código** - Extrair declarações de importação, analisar relacionamentos de módulos, identificar níveis de acoplamento, mapear interdependências de arquivos
2. **Análise de Relacionamento de Tarefas** - Consultar dependências de tarefas Linear, extrair menções de tarefas, analisar relacionamentos de projeto, mapear estruturas de épicos
3. **Construção de Grafo de Dependências** - Construir estrutura de grafo abrangente, identificar cadeias de dependências, calcular caminhos críticos, detectar gargalos
4. **Detecção de Dependência Circular** - Implementar algoritmos de detecção de ciclos, identificar loops problemáticos, avaliar severidade de impacto, recomendar estratégias de resolução
5. **Otimização de Ordem de Execução** - Calcular ordenação topológica, otimizar sequência de tarefas, equilibrar capacidade de equipe, minimizar dependências bloqueantes
6. **Avaliação de Risco** - Identificar cadeias de alto risco, avaliar pontos únicos de falha, mensurar complexidade de dependências, recomendar estratégias de mitigação

**Recursos Avançados**: Grafos visuais de dependências, representações em árvore ASCII, análise de impacto, otimização de planejamento de sprint, rastreamento de dependências em tempo real.

**Insights de Qualidade**: Métricas de saúde de dependências, análise de acoplamento, avaliação de manutenibilidade, distribuição de carga de trabalho da equipe.

**Saída**: Análise completa de dependências com representações visuais, recomendações de ordem de execução, estratégias de mitigação de risco e roteiro de otimização.