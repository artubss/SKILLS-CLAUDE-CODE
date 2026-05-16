---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [language] | --javascript | --java | --python | --rust | --go | --csharp
description: Configurar testes de mutação abrangentes com seleção de framework e integração com CI
---

# Adicionar Testes de Mutação

Configure framework de testes de mutação com métricas de qualidade e integração com CI: **$ARGUMENTS**

## Contexto Atual de Testes

- Linguagem: !`find . -name "*.js" -o -name "*.ts" | head -1 >/dev/null && echo "JavaScript/TypeScript" || find . -name "*.py" | head -1 >/dev/null && echo "Python" || find . -name "*.java" | head -1 >/dev/null && echo "Java" || echo "Multi-linguagem"`
- Cobertura de testes: !`find . -name "coverage" -o -name ".nyc_output" | head -1 || echo "Sem dados de cobertura"`
- Framework de testes: !`grep -l "jest\\|mocha\\|pytest\\|junit" package.json pom.xml setup.py 2>/dev/null | head -1 || echo "Detectar dos testes"`
- Sistema CI: !`find . -name ".github" -o -name ".gitlab-ci.yml" -o -name "Jenkinsfile" | head -1 || echo "Nenhum CI detectado"`

## Tarefa

Implemente testes de mutação abrangentes com otimização de framework e quality gates:

**Foco em Linguagem**: Use $ARGUMENTS para especificar JavaScript, Java, Python, Rust, Go, C#, ou detecção automática da base de código

**Framework de Testes de Mutação**:

1. **Seleção e Configuração de Ferramenta** - Escolha framework (Stryker, PIT, mutmut, cargo-mutants), instale dependências, configure definições básicas, valide instalação
2. **Configuração de Operadores de Mutação** - Configure operadores aritméticos, operadores relacionais, operadores lógicos, limites condicionais, mutações de declaração
3. **Otimização de Desempenho** - Configure execução paralela, configure testes incrementais, otimize filtragem de arquivos, implemente estratégias de cache
4. **Métricas de Qualidade** - Configure cálculo de mutation score, configure análise de sobrevivência, implemente imposição de limites, acompanhe tendências de efetividade
5. **Integração CI/CD** - Automatize triggers de execução, configure monitoramento de desempenho, configure relatórios de resultados, implemente gates de deployment
6. **Análise de Resultados** - Configure dashboards de visualização, configure análise de mutantes sobreviventes, implemente workflows de remediação, acompanhe padrões de regressão

**Funcionalidades Avançadas**: Testes de mutação seletivos, profiling de desempenho, sugestões automatizadas de melhoria de testes, análise de tendências de mutação, integração com quality gates.

**Suporte a Frameworks**: Otimizações específicas por linguagem, integração com ecossistema de ferramentas, ajuste de desempenho, customização de relatórios.

**Output**: Setup completo de testes de mutação com framework configurado, integração com CI, limites de qualidade e workflows de análise.