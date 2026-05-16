---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [linguagem] | --javascript | --python | --java | --haskell | --rust | --clojure
description: Implementar testes baseados em propriedades com seleção de framework e identificação de invariantes
---

# Adicionar Testes Baseados em Propriedades

Implementar framework de testes baseados em propriedades com análise de invariantes e geração automatizada de testes: **$ARGUMENTS**

## Contexto de Testes Atual

- Linguagem: !`find . -name "*.js" -o -name "*.ts" | head -1 >/dev/null && echo "JavaScript/TypeScript" || find . -name "*.py" | head -1 >/dev/null && echo "Python" || echo "Multi-linguagem"`
- Framework de testes: !`find . -name "jest.config.*" -o -name "pytest.ini" | head -1 || echo "Detectar framework"`
- Funções matemáticas: Análise da base de código para funções testáveis por propriedades
- Lógica de negócios: Identificação de invariantes e propriedades na lógica de domínio

## Tarefa

Implementar testes baseados em propriedades com análise de invariantes e geração automatizada de testes:

**Foco em Linguagem**: Use $ARGUMENTS para especificar JavaScript, Python, Java, Haskell, Rust, Clojure ou auto-detectar da base de código

**Framework de Testes Baseados em Propriedades**:

1. **Seleção de Framework** - Escolher ferramenta apropriada (fast-check, Hypothesis, QuickCheck, proptest), instalar dependências, configurar integração
2. **Identificação de Propriedades** - Analisar propriedades matemáticas, identificar invariantes de negócios, descobrir simetrias, avaliar propriedades de ida e volta
3. **Design de Geradores** - Criar geradores de dados customizados, implementar geração baseada em restrições, desenhar geradores compostos, otimizar estratégias de geração
4. **Implementação de Propriedades** - Escrever testes de propriedade, implementar pré-condições, desenhar pós-condições, criar verificações de invariantes
5. **Configuração de Encolhimento** - Configurar encolhimento de casos de teste, otimizar minimização de falhas, implementar encolhedores customizados, aprimorar depuração
6. **Integração e Relatório** - Integrar com suite de testes existente, configurar relatórios, setup de integração CI, otimizar performance de execução

**Funcionalidades Avançadas**: Testes de propriedade com estado, testes baseados em modelos, geradores customizados, execução paralela de propriedades, testes de performance de propriedades.

**Garantia de Qualidade**: Análise de completude de propriedades, cobertura de casos extremos, otimização de performance, avaliação de manutenibilidade.

**Saída**: Setup completo de testes baseados em propriedades com propriedades identificadas, geradores customizados, suite de testes integrada e otimização de performance.