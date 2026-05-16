---
allowed-tools: Read, Bash, Grep
argument-hint: [escopo] | --security | --licenses | --updates | --all
description: Audita dependências em busca de vulnerabilidades de segurança, conformidade de licenças e recomendações de atualização
---

# Auditoria de Dependências

Audita dependências em busca de vulnerabilidades de segurança e conformidade: **$ARGUMENTS**

## Dependências Atuais

- Arquivos de pacote: @package.json ou @requirements.txt ou @Cargo.toml ou @pom.xml
- Arquivos de lock: @package-lock.json ou @poetry.lock ou @Cargo.lock
- Verificação de segurança: !`npm audit --audit-level=moderate 2>/dev/null || pip check 2>/dev/null || cargo audit 2>/dev/null || echo "No security scanner available"`
- Pacotes desatualizados: !`npm outdated 2>/dev/null || pip list --outdated 2>/dev/null || echo "Check manually"`

## Tarefa

Realize uma auditoria abrangente de segurança e conformidade de dependências:

**Escopo de Auditoria**: Use $ARGUMENTS para focar em segurança, licenças, atualizações ou auditoria completa

**Áreas de Análise**:
1. **Verificação de Vulnerabilidades** - CVEs conhecidas, avisos de segurança, disponibilidade de exploits
2. **Análise de Versão** - Pacotes desatualizados, mudanças incompatíveis, recomendações de atualização
3. **Conformidade de Licenças** - Compatibilidade de licenças, restrições, obrigações legais
4. **Segurança da Cadeia de Suprimentos** - Autenticidade do pacote, status do mantenedor, dependências suspeitas
5. **Impacto no Desempenho** - Tamanho do bundle, dependências não utilizadas, oportunidades de otimização

**Saída**: Relatório de segurança priorizado com vulnerabilidades críticas, ações recomendadas e status de conformidade.