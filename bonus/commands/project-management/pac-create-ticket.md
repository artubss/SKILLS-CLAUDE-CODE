---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [ticket-name] | --epic | --type | --assignee | --priority
description: Criar novo ticket PAC dentro de um epic seguindo a especificação Product as Code
---

# Criar Ticket PAC

Criar um novo ticket dentro de um epic seguindo a especificação Product as Code: **$ARGUMENTS**

## Verificação de Configuração PAC

- Diretório PAC: !`ls -la .pac/ 2>/dev/null || echo "No .pac directory found"`
- Configuração PAC: @.pac/pac.config.yaml (se existir)
- Epics disponíveis: !`ls -la .pac/epics/ 2>/dev/null | head -10`

## Tarefa

Criar um novo ticket Product as Code dentro de um epic existente:

**Argumentos**:
- Nome do ticket (obrigatório se não usar flag --name)
- --epic <epic-id>: ID do epic pai (obrigatório)
- --type <type>: Tipo de ticket (feature/bug/task/spike)
- --assignee <assignee>: Desenvolvedor atribuído
- --priority <priority>: Nível de prioridade
- --create-branch: Criar automaticamente branch git

**Processo de Criação de Ticket**:
1. Validar se configuração PAC existe (sugerir `/project:pac-configure` se ausente)
2. Selecionar ou validar epic pai
3. Gerar ID único de ticket e número de sequência
4. Criar arquivo YAML do ticket seguindo especificação PAC v0.1.0 em `.pac/tickets/[ticket-id].yaml`
5. Incluir metadados obrigatórios: id, name, epic, timestamp de criação, assignee
6. Adicionar spec com descrição, tipo, status, prioridade, critérios de aceitação, tarefas
7. Vincular ticket ao epic pai
8. Criar branch git se solicitado

Se informações estiverem faltando, solicitar detalhes de ticket de forma interativa ao usuário.

**Próximos Passos**: Usar `/project:pac-update-status` para rastrear progresso do ticket.