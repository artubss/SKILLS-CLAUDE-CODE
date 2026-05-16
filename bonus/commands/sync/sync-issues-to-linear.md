---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [sync-scope] | --state | --label | --assignee | --milestone
description: Sincronizar issues do GitHub com workspace Linear com mapeamento abrangente de campos e gerenciamento de rate limit
---

# Sincronizar Issues para Linear

Sincronizar issues do GitHub com workspace Linear com mapeamento inteligente de campos: **$ARGUMENTS**

## Contexto do Repositório Atual

- Repositório: !`gh repo view --json nameWithOwner -q .nameWithOwner 2>/dev/null || echo "No repo context"`
- Contagem de issues: !`gh issue list --state all --limit 1 --json number | jq length 2>/dev/null || echo "Check manually"`
- Times Linear: Times Linear disponíveis e atribuições de projeto
- Rate limits: !`gh api rate_limit -q '.rate | "GitHub: \(.remaining)/\(.limit)"' 2>/dev/null`

## Tarefa

Execute sincronização abrangente de issues do GitHub com workspace Linear:

**Escopo de Sincronização**: Use $ARGUMENTS para filtrar por estado de issue, labels, assignees, milestones ou conjuntos de issues específicos

**Framework de Sincronização**:
1. **Descoberta de Issues** - Busque issues do GitHub com metadados abrangentes, aplique filtros, valide requisitos
2. **Mapeamento de Campos** - Transforme campos do GitHub para formato Linear, mapeie prioridades, converta labels, trate assignees
3. **Validação de Dados** - Verifique campos obrigatórios, valide mapeamentos de usuários, garanta integridade de dados, previna duplicatas
4. **Integração Linear** - Crie tasks com formatação apropriada, aplique atribuições de time, defina projetos, gerencie relacionamentos
5. **Gerenciamento de Rate Limit** - Implemente backoff exponencial, operações em lote, monitore limites de API, otimize requisições
6. **Rastreamento de Progresso** - Forneça atualizações em tempo real, trate erros adequadamente, mantenha estado de sincronização, gere relatórios

**Funcionalidades Avançadas**: Inferência inteligente de prioridades, mapeamento inteligente de usuários, capacidades de sincronização incremental, recuperação de erros abrangente.

**Integridade de Dados**: Preserve formatação, mantenha metadados, crie referências bidirecionais, garanta trilhas de auditoria.

**Saída**: Resultados completos de sincronização com métricas de sucesso, relatórios de erros, resumos de mapeamento e análises abrangentes de sincronização.