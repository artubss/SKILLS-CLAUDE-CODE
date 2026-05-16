---
description: "Transforme ideias em designs e especificações totalmente formados através de diálogo colaborativo natural. Use antes de implementar novas funcionalidades ou fazer mudanças significativas."
argument-hint: Descrição opcional da funcionalidade
allowed-tools: Read, Write, Grep, Glob, Bash, TodoWrite, AskUserQuestion, Skill, Task
---

## Fase 1: Descoberta

**Objetivo**: Entender o que precisa ser construído

Solicitação inicial: $ARGUMENTS

**Ações**:
1. Criar lista de tarefas com todas as fases
2. Se a funcionalidade não estiver clara, perguntar ao usuário:
   - Qual problema eles estão resolvendo?
   - O que a funcionalidade deve fazer?
   - Há restrições ou requisitos?
3. Resumir o entendimento e confirmar com o usuário

---

## Fase 2: Executar com Feature Analyzer Skill

Use a ferramenta Skill para invocar a skill "feature-design-assistant" e siga seu processo completo.