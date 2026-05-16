---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [action] | detect | resolve | analyze | configure | report
description: Resolver conflitos de sincronização com estratégias inteligentes e resolução automatizada
---

# Resolvedor de Conflitos de Sincronização

Resolver conflitos de sincronização com automação inteligente: **$ARGUMENTS**

## Estado Atual do Conflito

- Banco de dados de sincronização: @.sync-state.json ou arquivos de estado de sincronização com possíveis conflitos
- Histórico de conflitos: !`find . -name "*conflict*" -o -name "*sync-errors*" | wc -l` logs de conflito
- Regras de resolução: @conflict-rules.json ou configuração de resolução existente
- Conflitos ativos: Conflitos de sincronização não resolvidos que requerem atenção

## Tarefa

Implementar resolução abrangente de conflitos com automação inteligente:

**Ação de Resolução**: Use $ARGUMENTS para especificar detecção de conflitos, resolver usando estratégias, analisar padrões, configurar regras ou gerar relatórios

**Framework de Resolução de Conflitos**:
1. **Detecção de Conflitos** - Verificar itens sincronizados, comparar versões de campos, identificar conflitos de tempo, sinalizar problemas estruturais
2. **Resolução Inteligente** - Aplicar estratégias de resolução, lidar com mesclagem de campos, preservar dados críticos, manter relacionamentos
3. **Análise de Padrões** - Estudar tendências de conflitos, identificar problemas frequentes, sugerir melhorias de processo, otimizar estratégias
4. **Gerenciamento de Configuração** - Definir preferências de resolução, definir prioridades de campo, configurar regras de mesclagem, salvar configurações de automação
5. **Relatórios e Análises** - Gerar relatórios de conflitos, rastrear sucesso de resolução, analisar padrões de equipe, fornecer insights
6. **Prevenção Automatizada** - Implementar mecanismos de bloqueio, otimizar tempo de sincronização, habilitar notificações de alteração, reduzir conflitos

**Estratégias de Resolução**: Mais recente vence, mesclagem inteligente em nível de campo, resolução interativa manual, resolução com prioridade de sistema, resolução baseada em regras personalizadas.

**Garantia de Qualidade**: Backup antes da resolução, validação após alterações, capacidades de reversão, trilhas de auditoria abrangentes.

**Saída**: Conflitos resolvidos com relatórios de resolução detalhados, estado de sincronização atualizado, insights de análise de padrões e estratégias otimizadas de prevenção de conflitos.