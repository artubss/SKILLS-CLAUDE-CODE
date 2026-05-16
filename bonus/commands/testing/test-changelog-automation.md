---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [automation-type] | --changelog | --workflow-demo | --ci-integration | --validation
description: Automatizar fluxo de testes de changelog com integração CI e validação
---

# Automação de Testes de Changelog

Automatizar fluxo de testes de changelog com integração CI abrangente: **$ARGUMENTS**

## Contexto Atual de Automação

- Arquivos de changelog: !`find . -name "CHANGELOG*" -o -name "changelog*" | head -1 || echo "No changelog detected"`
- Sistema CI: !`find . -name ".github" -o -name ".gitlab-ci.yml" -o -name "Jenkinsfile" | head -1 || echo "No CI detected"`
- Controle de versão: !`git status >/dev/null 2>&1 && echo "Git repository" || echo "No git repository"`
- Processo de release: Análise de automação de release existente e versionamento

## Tarefa

Implementar automação abrangente de changelog com fluxos de testes e validação:

**Tipo de Automação**: Use $ARGUMENTS para focar em automação de changelog, demonstração de workflow, integração CI ou testes de validação

**Framework de Automação de Changelog**:

1. **Setup de Automação** - Configurar geração de changelog, setup de integração com controle de versão, implementar atualizações automatizadas, projetar regras de validação
2. **Integração de Workflow** - Projetar integração CI/CD, configurar triggers automatizados, implementar checks de validação, otimizar performance de execução
3. **Estratégia de Testes** - Criar testes de validação de changelog, implementar verificação de formato, projetar validação de conteúdo, setup de testes de regressão
4. **Garantia de Qualidade** - Configurar formatação automatizada, implementar checks de consistência, setup de validação de conteúdo, otimizar fluxos de manutenção
5. **Framework de Validação** - Projetar regras de validação automatizada, implementar verificação de conformidade, configurar relatório de erros, otimizar feedback loops
6. **Integração CI** - Setup de execução automatizada, configurar triggers de deployment, implementar sistemas de notificação, otimizar performance do pipeline

**Recursos Avançados**: Geração automatizada de notas de release, integração de versionamento semântico, atualizações automatizadas de documentação, validação de conformidade.

**Métricas de Qualidade**: Precisão do changelog, confiabilidade da automação, efetividade da validação, eficiência de manutenção.

**Output**: Automação completa de changelog com fluxos de testes, integração CI, regras de validação e procedimentos de manutenção.