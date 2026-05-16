---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [target-code] | [test-type] | --rust | --inline | --refactoring-suggestions
description: Criar plano abrangente de teste de integração com testes inline e recomendações de refatoração
---

# Plano de Teste de Integração

Criar plano de teste de integração com estratégia de teste inline e sugestões de refatoração: **$ARGUMENTS**

## Contexto Atual de Testes

- Tipo de projeto: !`[ -f Cargo.toml ] && echo "Projeto Rust" || [ -f package.json ] && echo "Projeto Node.js" || echo "Projeto multilíngue"`
- Framework de teste: !`find . -name "*.test.*" -o -name "*.spec.*" | head -3` testes existentes
- Código alvo: Análise de $ARGUMENTS para avaliação de testabilidade
- Complexidade de integração: Avaliação de interações de componentes e dependências

## Tarefa

Execute plano abrangente de teste de integração com análise de testabilidade:

**Foco de Planejamento**: Use $ARGUMENTS para especificar código alvo, requisitos de tipo de teste, teste inline em Rust, ou sugestões de refatoração

**Framework de Teste de Integração**:

1. **Análise de Testabilidade de Código** - Analise estrutura do código alvo, identifique desafios de teste, avalie níveis de acoplamento, avalie injeção de dependência
2. **Design de Estratégia de Teste** - Projete abordagem de teste de integração, planeje testes inline versus arquivos de teste separados, identifique limites de teste, otimize isolamento de testes
3. **Avaliação de Refatoração** - Identifique melhorias de testabilidade, sugira injeção de dependência, recomende abstrações de interface, otimize limites de componentes
4. **Planejamento de Casos de Teste** - Projete cenários de integração, identifique caminhos críticos, planeje teste de fluxo de dados, avalie cobertura de tratamento de erros
5. **Estratégia de Mock** - Planeje mock de dependências externas, projete test doubles, identifique limites de integração, otimize desempenho de testes
6. **Planejamento de Execução** - Projete ordem de execução de testes, planeje gerenciamento de dados de teste, otimize setup de ambiente de teste, garanta isolamento de testes

**Recursos Avançados**: Teste inline em estilo Rust, testes de integração baseados em propriedades, teste de contrato, virtualização de serviço, integração de chaos engineering.

**Garantia de Qualidade**: Manutenibilidade de testes, desempenho de execução, otimização de cobertura, eficiência de loop de feedback.

**Saída**: Plano abrangente de teste de integração com especificações de caso de teste, recomendações de refatoração, estratégia de implementação e métricas de qualidade.