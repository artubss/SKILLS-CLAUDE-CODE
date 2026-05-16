---
allowed-tools: Read, Glob, Grep, Bash
argument-hint: [escopo] | --modules | --patterns | --dependencies | --security
description: Análise abrangente de arquitetura com análise de padrões de design e recomendações de melhorias
---

# Revisão de Arquitetura

Execute análise abrangente da arquitetura do sistema e planejamento de melhorias: **$ARGUMENTS**

## Contexto da Arquitetura Atual

- Estrutura do projeto: !`find . -name "*.js" -o -name "*.ts" -o -name "*.py" -o -name "*.go" | head -5 && echo "..."`
- Dependências do pacote: !`[ -f package.json ] && echo "Projeto Node.js" || [ -f requirements.txt ] && echo "Projeto Python" || [ -f go.mod ] && echo "Projeto Go" || echo "Múltiplas linguagens"`
- Framework de testes: !`find . -name "*.test.*" -o -name "*spec.*" | head -3 && echo "..." || echo "Nenhum arquivo de teste encontrado"`
- Documentação: !`find . -name "README*" -o -name "*.md" | wc -l` arquivos de documentação

## Tarefa

Execute análise arquitetural abrangente com recomendações de melhorias acionáveis:

**Escopo de Revisão**: Use $ARGUMENTS para focar em módulos específicos, padrões de design, análise de dependências ou arquitetura de segurança

**Framework de Análise de Arquitetura**:
1. **Avaliação da Estrutura do Sistema** - Mapear hierarquia de componentes, identificar padrões arquiteturais, analisar limites de módulos, avaliar design em camadas
2. **Avaliação de Padrões de Design** - Identificar padrões implementados, avaliar consistência de padrões, detectar anti-padrões, avaliar efetividade de padrões
3. **Arquitetura de Dependências** - Analisar níveis de acoplamento, detectar dependências circulares, avaliar injeção de dependência, avaliar limites arquiteturais
4. **Análise de Fluxo de Dados** - Rastrear fluxo de informações, avaliar gerenciamento de estado, avaliar estratégias de persistência de dados, validar padrões de transformação
5. **Escalabilidade & Desempenho** - Analisar capacidades de escalabilidade, avaliar estratégias de cache, identificar gargalos, revisar gerenciamento de recursos
6. **Arquitetura de Segurança** - Revisar limites de confiança, avaliar padrões de autenticação, analisar fluxos de autorização, avaliar proteção de dados

**Análise Avançada**: Testabilidade de componentes, gerenciamento de configuração, padrões de tratamento de erros, integração de monitoramento, avaliação de extensibilidade.

**Avaliação de Qualidade**: Organização de código, adequação da documentação, padrões de comunicação do time, avaliação de débito técnico.

**Output**: Avaliação detalhada da arquitetura com recomendações de melhorias específicas, estratégias de refatoração e roadmap de implementação.