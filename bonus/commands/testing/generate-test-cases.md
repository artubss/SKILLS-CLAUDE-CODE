---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [target] | [scope] | --unit | --integration | --edge-cases | --automatic
description: Gera casos de teste abrangentes com análise automática e otimização de cobertura
---

# Gerar Casos de Teste

Gera casos de teste abrangentes com análise automática e cobertura inteligente: **$ARGUMENTS**

## Contexto Atual de Geração de Testes

- Código alvo: Análise de $ARGUMENTS para requisitos de geração de casos de teste
- Framework de testes: !`find . -name "jest.config.*" -o -name "*.test.*" | head -1 && echo "Jest/Vitest detectado" || echo "Detectar framework"`
- Complexidade do código: !`find . -name "*.js" -o -name "*.ts" -o -name "*.py" | xargs wc -l 2>/dev/null | tail -1 | awk '{print $1}' || echo "0"` linhas de código
- Padrões existentes: !`find . -name "*.test.*" -o -name "*.spec.*" | head -3` padrões de arquivo de teste

## Tarefa

Execute geração inteligente de casos de teste com cobertura abrangente e otimização:

**Escopo de Geração**: Use $ARGUMENTS para especificar arquivo alvo, testes unitários, testes de integração, casos extremos ou geração automática abrangente

**Framework de Geração de Casos de Teste**:

1. **Análise de Estrutura de Código** - Analise assinaturas de função, analise fluxo de controle, identifique caminhos de ramificação, avalie métricas de complexidade
2. **Reconhecimento de Padrões de Teste** - Analise padrões de teste existentes, identifique convenções de testes, extraia padrões reutilizáveis, otimize consistência
3. **Análise do Espaço de Entrada** - Identifique domínios de parâmetros, analise condições de limite, descubra casos extremos, avalie condições de erro
4. **Design de Casos de Teste** - Gere casos de teste positivos, casos de teste negativos, testes de valor limite, testes de classe de equivalência
5. **Planejamento de Estratégia de Mock** - Identifique dependências externas, projete implementações mock, crie fábricas de dados de teste, otimize isolamento de testes
6. **Otimização de Cobertura** - Garanta cobertura de caminho, otimize eficiência de teste, elimine redundância, maximize valor de testes

**Recursos Avançados**: Descoberta automática de casos extremos, geração inteligente de entrada, síntese de dados de teste, análise de lacunas de cobertura, geração de testes de performance.

**Garantia de Qualidade**: Manutenibilidade de testes, performance de execução, qualidade de asserções, efetividade de depuração.

**Saída**: Suite de casos de teste abrangente com cobertura otimizada, mocking inteligente, asserções apropriadas e diretrizes de manutenção.