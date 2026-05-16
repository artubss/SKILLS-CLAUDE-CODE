---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [pr-number] | --task | --auto-detect | --enable-auto | --update-state
description: Vincular pull requests do GitHub a tarefas Linear com sincronização automática de estado e integração de workflow
---

# Sincronizar PR para Tarefa

Vincular pull requests do GitHub a tarefas Linear com integração abrangente de workflow: **$ARGUMENTS**

## Contexto Atual do PR

- Repositório: !`gh repo view --json nameWithOwner -q .nameWithOwner 2>/dev/null || echo "No repo context"`
- Detalhes do PR: Baseado no número de PR ou critérios de auto-detecção em $ARGUMENTS
- Referências Linear: Detecção de IDs de tarefas no conteúdo do PR e nomes de branch
- Status do webhook: Configuração de automação atual para sincronização PR-tarefa

## Tarefa

Implementar vinculação abrangente de pull request para tarefas Linear com integração automática de workflow:

**Alvo do PR**: Use $ARGUMENTS para especificar número do PR, atribuição de tarefa, modo de auto-detecção ou configuração de automação

**Framework de Integração**:
1. **Detecção de Referências** - Extrair IDs de tarefas Linear do título, corpo, nomes de branch e mensagens de commit do PR
2. **Análise do PR** - Buscar dados completos do PR, analisar estado, status de revisão, métricas de mudança, timeline
3. **Sincronização de Estado** - Mapear estados do PR para equivalentes Linear, gerenciar ciclos de revisão, eventos de merge
4. **Atualizações de Tarefa** - Atualizar status da tarefa Linear, adicionar referências de PR, criar comentários, sincronizar metadados
5. **Aprimoramento do GitHub** - Adicionar contexto Linear ao PR, criar labels, postar resumos de tarefas, manter links
6. **Automação de Workflow** - Configurar webhooks, ativar sincronização em tempo real, implementar manipuladores de eventos, manter consistência

**Recursos Avançados**: Detecção inteligente de branch, mapeamento automático de estado, integração de revisão, análise de commit, validação abrangente.

**Integração de Workflow**: Atualizações em tempo real, sincronização bidirecional, automação orientada por eventos, monitoramento abrangente.

**Saída**: Integração completa PR-tarefa com sincronização automática, aprimoramento de workflow, gerenciamento de estado e rastreamento abrangente de relacionamentos.