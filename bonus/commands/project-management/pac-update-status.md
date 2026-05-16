---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [ticket-id] | --status | --assignee | --comment
description: Atualizar status de ticket PAC e rastrear progresso no workflow Product as Code
---

# Atualizar Status de Ticket PAC

Atualizar status de ticket e rastrear progresso no workflow Product as Code: **$ARGUMENTS**

## Verificação do Ambiente PAC

- Diretório PAC: !`ls -la .pac/ 2>/dev/null || echo "No .pac directory found"`
- Tickets ativos: !`find .pac/tickets/ -name "*.yaml" 2>/dev/null | wc -l`
- Atualizações recentes: !`find .pac/tickets/ -name "*.yaml" -mtime -7 2>/dev/null | wc -l`

## Tarefa

Atualizar status de ticket PAC e rastrear progresso de desenvolvimento:

**Argumentos**:
- --ticket <ticket-id>: ID do ticket a atualizar (ou selecionar interativamente)
- --status <status>: Novo status (backlog/in-progress/review/blocked/done/cancelled)
- --assignee <assignee>: Atualizar responsável
- --comment <comment>: Adicionar comentário de progresso
- --epic <epic-id>: Filtrar tickets por epic para seleção

**Processo de Atualização de Status**:
1. Validar ambiente PAC e localizar ticket
2. Carregar estado atual do ticket e validar transições de status
3. Atualizar YAML do ticket com novo status e timestamp
4. Executar ações específicas do status (criação de branch, sugestões de PR)
5. Atualizar epic pai com progresso do ticket
6. Gerar resumo de atualização de status com próximas ações

**Transições de Status Válidas**: backlog→in-progress→review→done, com blocked/cancelled como estados intermediários.

**Integração com Git**: Sugere criação de branch para in-progress, criação de PR para review e merge para status done.