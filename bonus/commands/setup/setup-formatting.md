---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [language] | --javascript | --typescript | --python | --multi-language
description: Configure ferramentas abrangentes de formatação de código com aplicação de estilo consistente
---

# Configurar Formatação de Código

Configure formatação abrangente de código com aplicação consistente de estilo: **$ARGUMENTS**

## Estado Atual do Projeto

- Linguagens detectadas: !`find . -name "*.js" -o -name "*.ts" -o -name "*.py" -o -name "*.rs" | head -5`
- Formatadores existentes: @.prettierrc or @pyproject.toml or @rustfmt.toml
- Gerenciador de pacotes: @package.json or @requirements.txt or @Cargo.toml
- Config de IDE: @.vscode/settings.json or @.editorconfig

## Tarefa

Configure um sistema abrangente de formatação de código com aplicação automatizada e consistência em equipe:

**Foco em Linguagem**: Use $ARGUMENTS para configurar formatação de JavaScript/TypeScript, Python, Rust ou multi-linguagem

**Configuração de Formatação**:
1. **Instalação de Ferramentas** - Prettier, Black, rustfmt, formatadores específicos de linguagem e plugins
2. **Configuração** - Regras de estilo, comprimento de linha, indentação, aspas, vírgulas finais, opções específicas de linguagem
3. **Integração com IDE** - Extensões de editor, formatação ao salvar, atalhos de teclado, configurações de workspace
4. **Automação** - Git hooks de pré-commit, verificações de formatação em CI/CD, scripts de formatação automatizados
5. **Sincronização em Equipe** - Configurações compartilhadas, guias de estilo, políticas de aplicação, documentação de onboarding
6. **Validação** - Verificação de formatação, integração com CI, monitoramento de conformidade da equipe

**Recursos Avançados**: Regras customizadas, formatação específica de framework, otimização de desempenho, formatação incremental.

**Consistência**: Compatibilidade multiplataforma, padronização em equipe, estratégias de migração de código legado.

**Saída**: Sistema completo de formatação com aplicação automatizada, configurações em equipe e monitoramento de conformidade de estilo.