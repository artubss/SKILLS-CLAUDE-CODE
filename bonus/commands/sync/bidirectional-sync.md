---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [sync-mode] | --full | --incremental | --dry-run | --conflict-strategy
description: Ativar sincronização bidirecional abrangente GitHub-Linear com resolução de conflitos
---

# Sincronização Bidirecional

Ativar sincronização bidirecional abrangente GitHub-Linear: **$ARGUMENTS**

## Ambiente de Sincronização Atual

- Status GitHub: !`gh api user 2>/dev/null && echo "✓ Autenticado" || echo "⚠ Não autenticado"`
- MCP Linear: Verificar se o servidor MCP Linear está disponível e configurado
- Estado de sincronização: @.sync-state.json ou @sync/ (se existir)
- Webhooks: !`gh api repos/{owner}/{repo}/hooks 2>/dev/null | grep -c linear || echo "0"`

## Tarefa

Implementar sincronização bidirecional robusta entre Issues do GitHub e tarefas do Linear:

**Modo de Sincronização**: Use $ARGUMENTS para especificar sincronização completa, sincronização incremental, visualização dry-run ou estratégia de resolução de conflitos

**Framework de Sincronização**:
1. **Gerenciamento de Estado de Sincronização** - Inicializar banco de dados de sincronização, rastrear relacionamentos de entidades, manter histórico de sincronização
2. **Detecção de Conflitos** - Identificar mudanças simultâneas, conflitos em nível de campo, problemas de timing
3. **Estratégias de Resolução** - NEWER_WINS, GITHUB_WINS, LINEAR_WINS ou merge inteligente em nível de campo
4. **Gerenciamento de Transações** - Operações atômicas, capacidade de rollback, bloqueio distribuído
5. **Integração de Webhooks** - Manipulação de eventos em tempo real, prevenção de loops de sincronização, triggers automatizados
6. **Integridade de Dados** - Validação bidirecional, manutenção de referências cruzadas, trilhas de auditoria

**Recursos Avançados**: Regras de merge em nível de campo, prevenção de loops de sincronização, automação de webhooks, otimização de desempenho, monitoramento abrangente.

**Pronto para Produção**: Segurança de transações, resolução de conflitos, recuperação de erros, monitoramento de desempenho, logging abrangente.

**Saída**: Sistema completo de sincronização bidirecional com resolução de conflitos, integração de webhooks, métricas de desempenho e relatório abrangente de sincronização.