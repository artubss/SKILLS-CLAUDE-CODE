---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [sync-scope] | --team | --project | --priority | --states
description: Sincronizar tarefas Linear com issues GitHub com mapeamento de estados e tratamento de anexos
---

# Sincronizar Linear para Issues

Sincronize tarefas Linear com issues GitHub com mapeamento abrangente de estados e campos: **$ARGUMENTS**

## Contexto Linear Atual

- Times Linear: Times disponíveis e atribuições de projetos
- Contagem de tarefas: Query de tarefas Linear para determinar escopo
- Repositório de destino: !`gh repo view --json nameWithOwner -q .nameWithOwner 2>/dev/null || echo "No repo context"`
- Mapeamentos de usuários: Correspondência entre email Linear e nome de usuário GitHub

## Tarefa

Execute sincronização abrangente de tarefas Linear para issues GitHub:

**Escopo de Sincronização**: Use $ARGUMENTS para filtrar por time Linear, projeto, níveis de prioridade ou estados de tarefa

**Framework de Sincronização**:
1. **Descoberta de Tarefas** - Query de tarefas Linear com filtros, extração de metadados, validação de requisitos, priorização de sincronização
2. **Mapeamento de Estados** - Transformar estados Linear em equivalentes GitHub, tratamento de conversão de prioridade, mapeamento de atribuições de projetos
3. **Transformação de Conteúdo** - Construir corpo da issue GitHub, preservar formatação, tratar anexos, manter estrutura
4. **Integração GitHub** - Criar issues com labels apropriadas, atribuir usuários, definir milestones, gerenciar relacionamentos
5. **Migração de Anexos** - Baixar anexos Linear, enviar para GitHub, atualizar referências, manter acessibilidade
6. **Sincronização de Comentários** - Transferir comentários com atribuição, preservar contexto, tratar menções, manter threading

**Recursos Avançados**: Mapeamento inteligente de estados, tratamento de anexos, threading de comentários, tradução de menções de usuários, validação abrangente.

**Fidelidade de Dados**: Preservar formatação Linear, manter relacionamentos de tarefas, manter timestamps, garantir integridade de referências.

**Output**: Resultados completos de sincronização com issues criadas, migrações de anexos, transferências de comentários e relatório abrangente de sincronização.