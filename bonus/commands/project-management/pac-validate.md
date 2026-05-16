---
allowed-tools: Read, Bash
argument-hint: [scope] | --file | --epic | --fix | --pre-commit
description: Validar a estrutura do projeto Product as Code e os arquivos quanto à conformidade com a especificação PAC
---

# Validar Estrutura PAC

Validar a estrutura do projeto Product as Code e os arquivos quanto à conformidade com a especificação PAC: **$ARGUMENTS**

## Estado Atual do PAC

- Diretório PAC: !`ls -la .pac/ 2>/dev/null || echo "Nenhum diretório .pac encontrado"`
- Configuração: @.pac/pac.config.yaml (se existir)
- Contagem de epics: !`find .pac/epics/ -name "*.yaml" 2>/dev/null | wc -l`
- Contagem de tickets: !`find .pac/tickets/ -name "*.yaml" 2>/dev/null | wc -l`

## Tarefa

Validação abrangente da estrutura do projeto PAC e conformidade com a especificação:

**Escopo de Validação**: Use $ARGUMENTS para arquivos/epics específicos ou valide toda a estrutura PAC

**Verificações de Validação**:
1. **Validação de Estrutura** - Estrutura de diretórios e arquivos obrigatórios
2. **Conformidade de Configuração** - Formato e valores do arquivo de config PAC
3. **Validação de Epics** - Sintaxe YAML, campos obrigatórios e conformidade com spec
4. **Validação de Tickets** - Formato, metadados e referências de epics
5. **Integridade de Referências Cruzadas** - Relacionamentos epic-ticket e dependências
6. **Consistência de Dados** - Timestamps, transições de status e unicidade de IDs

**Saída**: Relatório de validação detalhado com status de conformidade, problemas encontrados e recomendações específicas para correções. Use --fix para resolver automaticamente problemas comuns.

**Códigos de Saída**: 0 (válido), 1 (erros encontrados), 2 (problemas de configuração)