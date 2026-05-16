---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [action] | audit | repair | map | validate | export
description: Gerenciar links de referência entre plataformas (GitHub e Linear) com verificação de integridade
---

# Gerenciador de Referências Cruzadas

Gerenciar links de referência abrangentes entre plataformas com validação de integridade: **$ARGUMENTS**

## Estado Atual de Referências

- GitHub CLI: !`gh --version 2>/dev/null && echo "✓ Disponível" || echo "⚠ Não disponível"`
- Linear MCP: Verificar conectividade do servidor Linear MCP e autenticação
- Banco de dados de referências: @.reference-mappings.json ou arquivos de estado de referência
- Integridade de links: !`find . -name "*sync*" -o -name "*reference*" | wc -l` arquivos de mapeamento encontrados

## Tarefa

Implementar gerenciamento abrangente de referências cruzadas para integração GitHub-Linear:

**Ação de Gerenciamento**: Use $ARGUMENTS para especificar operações de auditoria, reparo, mapeamento, validação ou exportação

**Framework de Gerenciamento de Referências**:
1. **Banco de Dados de Referências** - Inicializar armazenamento de mapeamento, rastrear links bidirecionais, manter histórico de sincronização
2. **Auditoria de Integridade** - Verificar referências cruzadas, identificar links órfãos, detectar incompatibilidades, validar consistência
3. **Reparo Inteligente** - Corrigir referências quebradas, atualizar links desatualizados, consolidar duplicatas, remover entradas inválidas
4. **Visualização de Mapeamento** - Exibir redes de referência, mostrar saúde de conexões, destacar problemas, fornecer estatísticas
5. **Validação Profunda** - Verificar funcionalidade de links, testar navegação bidirecional, verificar consistência de campos, garantir integridade de dados
6. **Exportação e Documentação** - Gerar relatórios de mapeamento, criar arquivos de backup, fornecer instruções de importação, manter trilhas de auditoria

**Recursos Avançados**: Detecção automatizada de órfãos, reconstrução inteligente de referências, consolidação de duplicatas, validação abrangente.

**Proteção de Dados**: Backup antes de modificações, operações baseadas em transações, capacidades de reversão, logging abrangente.

**Saída**: Sistema completo de gerenciamento de referências com relatórios de integridade, resumos de reparo, visualizações de mapeamento e manutenção abrangente de links entre plataformas.