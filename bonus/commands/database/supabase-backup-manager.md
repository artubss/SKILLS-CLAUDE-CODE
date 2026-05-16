---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [operação] | --backup | --restore | --schedule | --validate | --cleanup
description: Gerenciar backups de banco de dados Supabase com agendamento automatizado e procedimentos de recuperação
---

# Gerenciador de Backup Supabase

Gerenciar backups abrangentes de banco de dados Supabase com agendamento automatizado e validação de recuperação: **$ARGUMENTS**

## Contexto Atual do Backup

- Projeto Supabase: Integração MCP para operações de backup e monitoramento de status
- Armazenamento de backup: Configuração de backup atual e capacidade de armazenamento
- Teste de recuperação: Última validação de backup e verificação de procedimento de recuperação
- Status de automação: !`find . -name "*.yml" -o -name "*.json" | xargs grep -l "backup\|cron" 2>/dev/null | head -3` configuração de backup agendado

## Tarefa

Executar gerenciamento abrangente de backup com procedimentos automatizados e validação de recuperação:

**Operação de Backup**: Use $ARGUMENTS para especificar criação de backup, restauração de dados, gerenciamento de agendamento, validação de backup ou procedimentos de limpeza

**Framework de Gerenciamento de Backup**:
1. **Estratégia de Backup** - Projetar agendas de backup, implementar políticas de retenção, configurar backups incrementais, otimizar uso de armazenamento
2. **Backup Automatizado** - Criar snapshots de banco de dados, exportar schema e dados, validar integridade do backup, monitorar conclusão do backup
3. **Procedimentos de Recuperação** - Testar processos de restauração, validar integridade de dados, implementar recuperação point-in-time, otimizar tempo de recuperação
4. **Gerenciamento de Agendamento** - Configurar agendas de backup automatizadas, implementar monitoramento de backup, configurar notificações de falha, otimizar janelas de backup
5. **Otimização de Armazenamento** - Gerenciar armazenamento de backup, implementar estratégias de compressão, arquivar backups antigos, monitorar custos de armazenamento
6. **Recuperação de Desastres** - Planejar procedimentos de recuperação de desastres, testar cenários de recuperação, documentar processos de recuperação, validar continuidade de negócios

**Recursos Avançados**: Validação automatizada de backup, otimização de tempo de recuperação, replicação de backup entre regiões, criptografia de backup, relatórios de conformidade.

**Integração de Monitoramento**: Monitoramento de sucesso de backup, alerta de falhas, rastreamento de uso de armazenamento, medição de tempo de recuperação, relatórios de conformidade.

**Saída**: Sistema completo de gerenciamento de backup com agendas automatizadas, procedimentos de recuperação, relatórios de validação e planejamento de recuperação de desastres.