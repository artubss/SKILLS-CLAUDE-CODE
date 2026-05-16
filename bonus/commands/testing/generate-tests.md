---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [caminho-do-arquivo] | [nome-do-componente]
description: Gera um arquivo de testes completo para um arquivo de origem ou componente especificado. Use quando o usuário solicitar explicitamente escrever, criar ou gerar testes para um arquivo específico.
---

# Gerar Testes

Gera uma suite de testes abrangente para: $ARGUMENTS

## Configuração de Testes Atual

- Framework de testes: !`cat package.json 2>/dev/null | grep -E '"jest"|"vitest"|"mocha"|"jasmine"' | head -3 || cat jest.config.* vitest.config.* 2>/dev/null | head -5 || echo "Framework não detectado"`
- Testes existentes: !`find . -name "*.test.*" -o -name "*.spec.*" | head -5`
- Cobertura de testes: !`npm run test:coverage 2>/dev/null || echo "Sem script de cobertura"`
- Alvo: se $ARGUMENTS for um caminho de arquivo, leia-o com @$ARGUMENTS; se for um nome de componente, procure por ele com Grep antes de escrever testes

## Framework de Geração de Testes

1. **Analisar** a estrutura do arquivo/componente alvo — identificar todas as funções exportadas, classes, métodos e suas assinaturas
2. **Estratégia** — examinar padrões de testes existentes no projeto; escolher escopo unitário vs integração; identificar caminhos críticos e cenários de erro
3. **Design de Mocks** — mapear todas as dependências externas (I/O, APIs, timers, datas); criar factories para dados de teste; planejar limpeza para operações assíncronas
4. **Testes Unitários** — escrever testes isolados por função/método cobrindo caminho feliz, casos extremos e condições de erro; seguir padrão AAA (Arrange, Act, Assert)
5. **Testes de Integração** — testar interações de componentes, camadas de API com respostas mockadas e workflows ponta a ponta quando aplicável
6. **Verificação de Qualidade** — verificar se a nomenclatura descreve o comportamento, não a implementação; confirmar 80%+ de cobertura em lógica de negócio crítica; garantir isolamento de testes

## Orientação Específica do Framework

- **React**: Testes de componentes com React Testing Library; testar interações do usuário e renderização
- **Vue**: Testes de componentes com Vue Test Utils; testar props, eventos e slots
- **Angular**: Testes de componente e serviço com TestBed; testar injeção de dependência
- **Node.js**: Testes de endpoint de API e middleware; testar ciclos de request/response
- **Python**: `pytest` com fixtures, `unittest.mock` para patching, `pytest-cov` para cobertura
- **Go**: Testes orientados por tabela em arquivos `_test.go`, `testify/assert` para assertions, subtestes via `t.Run()`
- **Rust**: Módulos `#[cfg(test)]`, atributos `#[test]`, `mockall` para mocking

## Boas Práticas

- Seguir padrão AAA (Arrange, Act, Assert)
- 80%+ de cobertura; priorizar lógica de negócio crítica e caminhos de erro
- Mockar I/O externo; usar factories para dados de teste
- Nomenclatura: descrever o que a função faz, não detalhes de implementação