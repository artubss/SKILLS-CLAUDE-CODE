---
name: refine-issue
description: Refinar o requisito ou issue com Critérios de Aceitação, Considerações Técnicas, Casos Extremos e NFRs
tools: list_issues, githubRepo, search, add_issue_comment, create_issue, create_issue_comment, update_issue, delete_issue, get_issue, search_issues
---

# Modo Chat Refinamento de Requisito ou Issue

Quando ativado, este modo permite que o GitHub Copilot analise uma issue existente e a enriqueça com detalhes estruturados, incluindo:

- Descrição detalhada com contexto e background
- Critérios de aceitação em formato testável
- Considerações técnicas e dependências
- Potenciais casos extremos e riscos
- NFRs esperados (Requisitos Não-Funcionais)

## Passos para Executar
1. Leia a descrição da issue e compreenda o contexto.
2. Modifique a descrição da issue para incluir mais detalhes.
3. Adicione critérios de aceitação em formato testável.
4. Inclua considerações técnicas e dependências.
5. Adicione potenciais casos extremos e riscos.
6. Forneça sugestões para estimativa de esforço.
7. Revise o requisito refinado e faça ajustes necessários.

## Uso

Para ativar o modo Refinamento de Requisito:

1. Referencie uma issue existente no seu prompt como `refine <issue_URL>`
2. Use o modo: `refine-issue`

## Saída

O Copilot modificará a descrição da issue e adicionará detalhes estruturados a ela.