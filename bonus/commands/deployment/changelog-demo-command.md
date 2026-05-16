---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [format] | --generate | --validate | --demo
description: Demonstrar recursos de automação de changelog com exemplos reais e validação
---

# Demo de Automação de Changelog

Demonstrar recursos de automação de changelog: $ARGUMENTS

## Estado Atual do Projeto

- Changelog existente: @CHANGELOG.md (se existir)
- Versão do pacote: @package.json ou @pyproject.toml ou @Cargo.toml (se existir)
- Commits recentes: !`git log --oneline -10`
- Tags Git: !`git tag -l | tail -5`

## Recursos da Demo

### 1. **Demo de Geração de Changelog**
- Gerar entradas de changelog de exemplo a partir de commits git
- Mostrar diferentes formatos de changelog (Keep a Changelog, conventional-changelog)
- Demonstrar categorização automática de mudanças
- Mostrar versionamento e semantic versioning

### 2. **Demo de Validação de Formato**
- Validar conformidade do formato do changelog existente
- Mostrar inconsistências de formato e sugestões
- Demonstrar correções de formatação automatizadas
- Mostrar integração com automação de release

### 3. **Testes de Integração**
- Testar automação de changelog sem afetar o workflow principal
- Validar pipeline de geração de changelog
- Testar diferentes padrões de mensagens de commit
- Mostrar tratamento de erros e recuperação

### 4. **Benchmarking de Performance**
- Medir velocidade de geração de changelog
- Testar com históricos de commits grandes
- Mostrar uso de memória e otimizações
- Fazer benchmark de diferentes estratégias de parsing