---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [número-issue] | --team | --project | --close-github | --skip-comments
description: Converte issues individuais do GitHub em tarefas Linear com preservação abrangente de dados
---

# Issue para Tarefa Linear

Converte issues do GitHub em tarefas Linear com mapeamento abrangente de campos: **$ARGUMENTS**

## Contexto de Conversão Atual

- Repositório: !`gh repo view --json nameWithOwner -q .nameWithOwner 2>/dev/null || echo "No repo context"`
- Detalhes da issue: Com base no número da issue ou critérios de seleção em $ARGUMENTS
- Times Linear: Times e atribuições de projeto disponíveis no Linear
- Mapeamentos de usuário: @user-mappings.json ou correspondência de usuários GitHub-Linear

## Tarefa

Execute conversão precisa de issues individuais do GitHub em tarefas Linear:

**Alvo da Issue**: Use $ARGUMENTS para especificar número da issue, opções de conversão, atribuição de time ou preferências de processamento

**Framework de Conversão**:
1. **Análise da Issue** - Busque dados completos da issue, extraia metadados, analise estrutura de conteúdo, infira prioridades
2. **Transformação de Dados** - Mapeie campos com precisão, converta formatos, preserve relacionamentos, melhore descrições
3. **Integração Linear** - Crie tarefa com formatação adequada, atribua time/projeto, defina prioridades, gerencie labels
4. **Migração de Conteúdo** - Importe comentários com atribuição, manipule anexos, preserve formatação, mantenha threading
5. **Gerenciamento de Referências** - Crie links bidirecionais, atualize banco de dados de sincronização, mantenha referências cruzadas, habilite navegação
6. **Validação e Confirmação** - Verifique precisão da conversão, confirme mapeamentos de campos, valide relacionamentos, forneça preview

**Recursos Avançados**: Inferência inteligente de prioridade, mapeamento inteligente de usuários, manipulação de anexos, threading de comentários, validação abrangente.

**Fidelidade de Dados**: Preserve formatação original, mantenha todos os metadados, guarde atribuição de comentários, garanta integridade de relacionamentos.

**Saída**: Tarefa Linear convertida com sucesso com preservação completa de dados, mapeamentos de campos precisos, referências bidirecionais e resumo abrangente da conversão.