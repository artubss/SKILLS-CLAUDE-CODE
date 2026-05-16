---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [plataforma] | --github-actions | --gitlab-ci | --azure-pipelines | --jenkins
description: Configar pipeline de CI/CD abrangente com testes, deploy e monitoramento automatizados
---

# Configurar Pipeline de CI/CD

Configar pipeline de CI/CD abrangente com workflows automatizados e deployments: **$ARGUMENTS**

## Estado Atual do Repositório

- Controle de versão: !`git remote -v | head -1` (GitHub, GitLab, etc.)
- CI existente: !`find . -name ".github" -o -name ".gitlab-ci.yml" -o -name "azure-pipelines.yml" | wc -l`
- Framework de testes: @package.json ou detecção de arquivos de teste
- Config de deployment: @Dockerfile ou manifestos de deployment

## Tarefa

Implementar pipeline de CI/CD pronto para produção com automação abrangente e boas práticas:

**Escolha de Plataforma**: Use $ARGUMENTS para especificar GitHub Actions, GitLab CI, Azure Pipelines ou Jenkins

**Arquitetura do Pipeline**:
1. **Automação de Build** - Compilação de código, instalação de dependências, criação de artefatos
2. **Estratégia de Testes** - Testes unitários, testes de integração, testes e2e, relatório de cobertura de código
3. **Gates de Qualidade** - Linting, varredura de segurança, avaliação de vulnerabilidades, métricas de qualidade de código
4. **Automação de Deployment** - Deployment em staging, deployment em produção, mecanismos de rollback
5. **Gerenciamento de Ambiente** - Provisionamento de infraestrutura, gerenciamento de configuração, tratamento de secrets
6. **Integração de Monitoramento** - Monitoramento de performance, rastreamento de erros, notificações de deployment

**Recursos Avançados**: Execução de jobs em paralelo, matrix builds, estratégias de deployment (blue-green, canary) e suporte multi-ambiente.

**Segurança & Conformidade**: Gerenciamento seguro de credenciais, verificações de conformidade, trilhas de auditoria e workflows de aprovação.

**Output**: Pipeline de CI/CD completo com testes automatizados, deployments seguros, integração de monitoramento e documentação abrangente.