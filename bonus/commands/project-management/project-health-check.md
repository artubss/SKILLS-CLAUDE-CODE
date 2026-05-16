---
allowed-tools: Read, Bash, Grep, Glob
argument-hint: [evaluation-period] | --30-days | --sprint | --quarter
description: Analisar saúde geral do projeto e gerar relatório abrangente de métricas
---

# Verificação de Saúde do Projeto

Analisar saúde geral do projeto e métricas: **$ARGUMENTS**

## Estado Atual do Projeto

- Atividade Git: !`git log --oneline --since="30 days ago" | wc -l`
- Contribuidores: !`git shortlog -sn --since="30 days ago" | head -5`
- Status de branches: !`git branch -r | wc -l` branches remotas
- Alterações de código: !`git diff --stat HEAD~30 2>/dev/null || echo "Not enough history"`
- Dependências: @package.json ou @requirements.txt ou @Cargo.toml (se existir)

## Tarefa

Gerar um relatório abrangente de saúde do projeto analisando:

**Período de Avaliação**: Use $ARGUMENTS ou padrão dos últimos 30 dias

**Dimensões de Saúde**:
1. **Métricas de Qualidade de Código**
   - Cobertura de testes e tendências
   - Análise de complexidade de código
   - Vulnerabilidades de segurança (executar npm audit ou equivalente)
   - Indicadores de débito técnico

2. **Performance de Entrega**
   - Tendências de velocidade de sprint (se ferramentas de gerenciamento de tarefas disponíveis)
   - Análise de tempo de ciclo
   - Proporção de bugs vs features
   - Métricas de entrega dentro do prazo

3. **Indicadores de Saúde do Time**
   - Tempo de resposta de revisão de PR
   - Distribuição de frequência de commits
   - Equilíbrio na distribuição de trabalho
   - Risco de concentração de conhecimento

4. **Saúde de Dependências**
   - Avaliação de pacotes desatualizados
   - Resultados de auditoria de segurança
   - Verificação de conformidade de licenças
   - Dependências de serviços externos

**Formato do Relatório de Saúde**:
- Pontuação geral de saúde (0-100) com status codificado por cor
- Resumo executivo com principais achados
- Tabelas de métricas detalhadas com valores atuais vs alvo
- Análise de tendências e avaliação de riscos
- Recomendações acionáveis priorizadas por impacto

**Saída**: Gerar relatório markdown com gráficos, tabelas de métricas e itens de ação específicos para melhorar a saúde do projeto.