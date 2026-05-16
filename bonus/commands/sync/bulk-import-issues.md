---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [import-scope] | --state | --label | --milestone | --batch-size
description: Importação em massa de issues do GitHub para Linear com rastreamento abrangente de progresso e tratamento de erros
---

# Importação em Massa de Issues

Importação em massa de issues do GitHub para Linear com capacidades avançadas de processamento: **$ARGUMENTS**

## Contexto de Importação Atual

- Repositório: !`gh repo view --json nameWithOwner -q .nameWithOwner 2>/dev/null || echo "No repo context"`
- Contagem de issues: !`gh api repos/{owner}/{repo}/issues?state=all --paginate | jq length 2>/dev/null || echo "Check manually"`
- Times Linear: Verifique os times e projetos Linear disponíveis para mapeamento de importação
- Limites de taxa: !`gh api rate_limit -q '.rate | "GitHub: \(.remaining)/\(.limit)"' 2>/dev/null || echo "Check GitHub rate limit"`

## Tarefa

Execute importação em massa eficiente de issues do GitHub para Linear com gerenciamento abrangente:

**Escopo de Importação**: Use $ARGUMENTS para filtrar por estado, labels, milestones ou configurar parâmetros de processamento em lotes

**Pipeline de Importação**:
1. **Análise Pré-Importação** - Descoberta de issues, detecção de duplicatas, estimativa de importação, planejamento de recursos
2. **Configuração de Lotes** - Dimensionamento dinâmico de lotes, gerenciamento de limites de taxa, rastreamento de progresso, tratamento de erros
3. **Transformação de Dados** - Mapeamento de campos, inferência de prioridade, mapeamento de usuários, aprimoramento de conteúdo
4. **Execução de Importação** - Processamento paralelo, lógica de retry, gerenciamento de transações, relatório de progresso
5. **Recuperação de Erros** - Tratamento de itens com falha, mecanismos de retry, recuperação de importação parcial, validação
6. **Ações Pós-Importação** - Criação de referências cruzadas, atualizações do GitHub, arquivos de mapeamento, notificações

**Recursos Avançados**: Ajuste dinâmico de lotes, limitação inteligente de taxa, detecção de duplicatas, recuperação abrangente de erros, visualização de progresso.

**Garantia de Qualidade**: Validação pré-importação, verificação pós-importação, verificações de integridade de dados, trilhas de auditoria abrangentes.

**Saída**: Resultados completos de importação com métricas de sucesso, relatórios de itens com falha, documentação de mapeamento e análise de desempenho para migração de issues em larga escala.