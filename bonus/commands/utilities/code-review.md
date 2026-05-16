---
allowed-tools: Read, Bash, Grep, Glob
argument-hint: [file-path] | [commit-hash] | --full
description: Revisão abrangente de qualidade de código com análise de segurança, desempenho e arquitetura
---

# Revisão de Qualidade de Código

Realize uma revisão abrangente de qualidade de código: $ARGUMENTS

## Estado Atual

- Status Git: !`git status --porcelain`
- Mudanças recentes: !`git diff --stat HEAD~5`
- Informações do repositório: !`git log --oneline -5`
- Status de build: !`npm run build --dry-run 2>/dev/null || echo "No build script"`

## Tarefa

Siga estas etapas para conduzir uma revisão de código minuciosa:

1. **Análise do Repositório**
   - Examine a estrutura do repositório e identifique a linguagem/framework principal
   - Verifique arquivos de configuração (package.json, requirements.txt, Cargo.toml, etc.)
   - Analise README e documentação para contexto

2. **Avaliação de Qualidade de Código**
   - Procure por code smells, anti-patterns e bugs potenciais
   - Verifique consistência de estilo de código e convenções de nomenclatura
   - Identifique imports não utilizados, variáveis ou código morto
   - Revise práticas de tratamento de erros e logging

3. **Revisão de Segurança**
   - Procure por vulnerabilidades de segurança comuns (SQL injection, XSS, etc.)
   - Verifique secrets hardcodeados, chaves de API ou senhas
   - Revise lógica de autenticação e autorização
   - Examine validação e sanitização de entrada

4. **Análise de Desempenho**
   - Identifique gargalos potenciais de desempenho
   - Verifique algoritmos ineficientes ou queries de banco de dados
   - Revise padrões de uso de memória e vazamentos potenciais
   - Analise tamanho de bundle e oportunidades de otimização

5. **Arquitetura e Design**
   - Avalie organização de código e separação de responsabilidades
   - Verifique abstração e modularidade apropriadas
   - Revise gerenciamento de dependências e acoplamento
   - Avalie escalabilidade e manutenibilidade

6. **Cobertura de Testes**
   - Verifique cobertura de testes existente e qualidade
   - Identifique áreas com testes inadequados
   - Revise estrutura e organização de testes
   - Sugira cenários de teste adicionais

7. **Revisão de Documentação**
   - Avalie comentários de código e documentação inline
   - Verifique completude da documentação de API
   - Revise README e instruções de setup
   - Identifique áreas que precisam de melhor documentação

8. **Recomendações**
   - Priorize problemas por severidade (crítico, alto, médio, baixo)
   - Forneça recomendações específicas e acionáveis
   - Sugira ferramentas e práticas para melhoria
   - Crie um relatório resumido com próximos passos

Lembre-se de ser construtivo e forneça exemplos específicos com caminhos de arquivo e números de linha quando aplicável.