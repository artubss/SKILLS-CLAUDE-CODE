---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [tipo-orquestração] | --parallel | --sequential | --condicional | --otimização-pipeline
description: Orquestre automação de testes abrangente com execução inteligente e otimização
---

# Orquestrador de Automação de Testes

Orquestre automação de testes inteligente com otimização de execução e gerenciamento de recursos: **$ARGUMENTS**

## Contexto de Orquestração Atual

- Suites de testes: !`find . -name "*.test.*" -o -name "*.spec.*" | wc -l` arquivos de teste em todo o projeto
- Frameworks de teste: !`find . -name "jest.config.*" -o -name "cypress.config.*" -o -name "playwright.config.*" | wc -l` frameworks configurados
- Sistema CI: !`find . -name ".github" -o -name ".gitlab-ci.yml" | head -1 || echo "No CI detected"`
- Uso de recursos: Análise de padrões de execução de testes atuais e desempenho

## Tarefa

Implemente orquestração inteligente de testes com otimização de execução e gerenciamento de recursos:

**Tipo de Orquestração**: Use $ARGUMENTS para focar em execução paralela, execução sequencial, testes condicionais ou otimização de pipeline

**Framework de Orquestração de Testes**:

1. **Descoberta e Classificação de Testes** - Analise suites de testes, classifique tipos de teste, avalie requisitos de execução, otimize categorização
2. **Design de Estratégia de Execução** - Projete estratégias de execução paralela, implemente batching inteligente, otimize alocação de recursos, configure execução condicional
3. **Gerenciamento de Dependências** - Analise dependências de testes, implemente ordenação de execução, configure validação de pré-requisitos, otimize resolução de dependências
4. **Otimização de Recursos** - Configure execução paralela, implemente pooling de recursos, otimize uso de memória, projete execução escalável
5. **Integração de Pipeline** - Projete integração CI/CD, implemente orquestração de estágios, configure tratamento de falhas, otimize loops de feedback
6. **Monitoramento e Análise** - Implemente monitoramento de execução, configure rastreamento de desempenho, projete análise de falhas, otimize relatórios

**Recursos Avançados**: Seleção de testes orientada por IA, otimização de execução preditiva, alocação dinâmica de recursos, recuperação inteligente de falhas, otimização de custos.

**Garantia de Qualidade**: Confiabilidade de execução, consistência de desempenho, eficiência de recursos, otimização de manutenibilidade.

**Saída**: Sistema completo de orquestração de testes com execução otimizada, gerenciamento inteligente de recursos, monitoramento abrangente e análise de desempenho.