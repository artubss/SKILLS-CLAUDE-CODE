---
allowed-tools: Read, Write, Edit, Glob
argument-hint: [escopo] | --claude-md | --documentation | --outdated-patterns | --implementation-sync
description: Limpar e organizar arquivos de memória do projeto com sincronização de implementação e atualizações de padrões
---

# Limpeza de Memória Primaveril

Limpar e sincronizar memória do projeto com padrões de implementação atuais: **$ARGUMENTS**

## Contexto Atual de Memória

- Arquivos de memória: !`find . -name "CLAUDE*.md" | wc -l` arquivos CLAUDE.md no projeto
- Documentação: !`find . -name "README*" -o -name "*.md" | wc -l` arquivos de documentação total
- Última atualização: !`find . -name "CLAUDE.md" -exec stat -c "%y" {} \; 2>/dev/null | head -1 || echo "Nenhum CLAUDE.md encontrado"`
- Divergência de implementação: Análise de padrões documentados vs reais

## Tarefa

Executar limpeza abrangente de memória com sincronização de implementação:

**Escopo de Limpeza**: Use $ARGUMENTS para focar em arquivos CLAUDE.md, documentação geral, identificação de padrões obsoletos ou sincronização de implementação

**Framework de Limpeza de Memória**:
1. **Descoberta de Arquivos de Memória** - Localizar todos os arquivos CLAUDE.md e documentação, avaliar hierarquia e organização, identificar conteúdo redundante
2. **Análise de Implementação** - Comparar padrões documentados com código real, identificar divergência de implementação, avaliar lacunas de precisão
3. **Validação de Padrões** - Verificar convenções documentadas, validar exemplos de código, verificar precisão de dependências, avaliar alinhamento da pilha de tecnologia
4. **Otimização de Conteúdo** - Remover informações obsoletas, consolidar conteúdo duplicado, melhorar estrutura de organização, aumentar clareza
5. **Atualizações de Sincronização** - Atualizar comandos de desenvolvimento, atualizar referências da pilha de tecnologia, sincronizar padrões arquiteturais, validar workflows
6. **Garantia de Qualidade** - Garantir consistência entre arquivos, validar formatação markdown, verificar integridade de links, manter alinhamento de versão

**Recursos Avançados**: Detecção automática de padrões, análise de divergência de implementação, validação de referências cruzadas, pontuação de saúde de documentação.

**Saúde de Memória**: Métricas de frescor de conteúdo, validação de precisão, análise de padrão de uso, recomendações de agendamento de manutenção.

**Output**: Arquivos de memória limpos e sincronizados com padrões atualizados, implementações validadas e recomendações de manutenção.