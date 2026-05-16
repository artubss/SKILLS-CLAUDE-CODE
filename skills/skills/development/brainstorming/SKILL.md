---
name: brainstorming
description: "Você DEVE usar isto antes de qualquer trabalho criativo - criar features, construir componentes, adicionar funcionalidade ou modificar comportamento. Explora a intenção do usuário, requisitos e design antes da implementação."
---

# Transformando Ideias em Designs

## Visão Geral

Ajude a transformar ideias em designs e especificações bem formados através de diálogo colaborativo natural.

Comece entendendo o contexto atual do projeto, depois faça perguntas uma por vez para refinar a ideia. Quando você entender o que está sendo construído, apresente o design em pequenas seções (200-300 palavras), validando após cada seção se está correto até agora.

## O Processo

**Entendendo a ideia:**
- Verifique o estado atual do projeto primeiro (arquivos, docs, commits recentes)
- Faça perguntas uma por vez para refinar a ideia
- Prefira perguntas de múltipla escolha quando possível, mas abertas também são válidas
- Apenas uma pergunta por mensagem - se um tópico precisa de mais exploração, divida em múltiplas perguntas
- Foque em entender: propósito, restrições, critérios de sucesso

**Explorando abordagens:**
- Proponha 2-3 abordagens diferentes com trade-offs
- Apresente opções conversacionalmente com sua recomendação e raciocínio
- Comece com sua opção recomendada e explique o porquê

**Apresentando o design:**
- Quando você acreditar que entende o que está sendo construído, apresente o design
- Divida em seções de 200-300 palavras
- Pergunte após cada seção se está correto até agora
- Cubra: arquitetura, componentes, fluxo de dados, tratamento de erros, testes
- Esteja pronto para voltar e esclarecer se algo não fizer sentido

## Depois do Design

**Documentação:**
- Escreva o design validado em `docs/plans/YYYY-MM-DD-<topico>-design.md`
- Use a skill elements-of-style:writing-clearly-and-concisely se disponível
- Faça commit do documento de design no git

**Implementação (se continuar):**
- Pergunte: "Pronto para configurar a implementação?"
- Use superpowers:using-git-worktrees para criar workspace isolado
- Use superpowers:writing-plans para criar plano de implementação detalhado

## Princípios-Chave

- **Uma pergunta por vez** - Não sobrecarregue com múltiplas perguntas
- **Múltipla escolha preferida** - Mais fácil responder do que aberta quando possível
- **YAGNI sem piedade** - Remova features desnecessárias de todos os designs
- **Explore alternativas** - Sempre proponha 2-3 abordagens antes de se estabelecer
- **Validação incremental** - Apresente design em seções, valide cada uma
- **Seja flexível** - Volte e esclareça quando algo não fizer sentido