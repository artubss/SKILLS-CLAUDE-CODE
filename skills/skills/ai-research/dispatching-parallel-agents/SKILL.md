---
name: dispatching-parallel-agents
description: Use quando enfrenta 2+ tarefas independentes que podem ser trabalhadas sem estado compartilhado ou dependências sequenciais
---

# Despachando Agentes em Paralelo

## Visão Geral

Quando você tem múltiplas falhas não relacionadas (arquivos de teste diferentes, subsistemas diferentes, bugs diferentes), investigá-las sequencialmente desperdiça tempo. Cada investigação é independente e pode acontecer em paralelo.

**Princípio central:** Despache um agente por domínio de problema independente. Deixe-os trabalhar simultaneamente.

## Quando Usar

```dot
digraph when_to_use {
    "Múltiplas falhas?" [shape=diamond];
    "São independentes?" [shape=diamond];
    "Um agente investiga tudo" [shape=box];
    "Um agente por domínio" [shape=box];
    "Podem trabalhar em paralelo?" [shape=diamond];
    "Agentes sequenciais" [shape=box];
    "Despacho em paralelo" [shape=box];

    "Múltiplas falhas?" -> "São independentes?" [label="sim"];
    "São independentes?" -> "Um agente investiga tudo" [label="não - relacionadas"];
    "São independentes?" -> "Podem trabalhar em paralelo?" [label="sim"];
    "Podem trabalhar em paralelo?" -> "Despacho em paralelo" [label="sim"];
    "Podem trabalhar em paralelo?" -> "Agentes sequenciais" [label="não - estado compartilhado"];
}
```

**Use quando:**
- 3+ arquivos de teste falhando com causas raiz diferentes
- Múltiplos subsistemas quebrados independentemente
- Cada problema pode ser entendido sem contexto dos outros
- Sem estado compartilhado entre investigações

**Não use quando:**
- Falhas são relacionadas (corrigir uma pode corrigir outras)
- Precisa entender o estado completo do sistema
- Agentes interfeririam um com o outro

## O Padrão

### 1. Identifique Domínios Independentes

Agrupe falhas pelo que está quebrado:
- Testes do Arquivo A: Fluxo de aprovação de ferramenta
- Testes do Arquivo B: Comportamento de conclusão em lote
- Testes do Arquivo C: Funcionalidade de abortar

Cada domínio é independente – corrigir aprovação de ferramenta não afeta testes de aborto.

### 2. Crie Tarefas de Agente Focadas

Cada agente recebe:
- **Escopo específico:** Um arquivo de teste ou subsistema
- **Objetivo claro:** Fazer esses testes passarem
- **Restrições:** Não altere outro código
- **Output esperado:** Resumo do que você encontrou e corrigiu

### 3. Despache em Paralelo

```typescript
// Em Claude Code / ambiente AI
Task("Corrigir falhas em agent-tool-abort.test.ts")
Task("Corrigir falhas em batch-completion-behavior.test.ts")
Task("Corrigir falhas em tool-approval-race-conditions.test.ts")
// Os três rodam concorrentemente
```

### 4. Revise e Integre

Quando agentes retornam:
- Leia cada resumo
- Verifique se os corrigios não entram em conflito
- Execute a suite de testes completa
- Integre todas as mudanças

## Estrutura de Prompt de Agente

Bons prompts de agente são:
1. **Focados** - Um domínio de problema claro
2. **Autossuficientes** - Todo contexto necessário para entender o problema
3. **Específicos sobre output** - O que o agente deve retornar?

```markdown
Corrija os 3 testes falhando em src/agents/agent-tool-abort.test.ts:

1. "should abort tool with partial output capture" - espera 'interrupted at' na mensagem
2. "should handle mixed completed and aborted tools" - ferramenta rápida foi abortada em vez de concluída
3. "should properly track pendingToolCount" - espera 3 resultados mas recebe 0

Estes são problemas de timing/race condition. Sua tarefa:

1. Leia o arquivo de teste e entenda o que cada teste verifica
2. Identifique a causa raiz – problemas de timing ou bugs reais?
3. Corrija:
   - Substituindo timeouts arbitrários por espera baseada em eventos
   - Corrigindo bugs na implementação de aborto se encontrados
   - Ajustando expectativas de teste se o comportamento mudou

Não apenas aumente timeouts – encontre o problema real.

Retorne: Resumo do que você encontrou e do que você corrigiu.
```

## Erros Comuns

**❌ Muito amplo:** "Corrija todos os testes" - agente fica perdido
**✅ Específico:** "Corrija agent-tool-abort.test.ts" - escopo focado

**❌ Sem contexto:** "Corrija a race condition" - agente não sabe onde
**✅ Contexto:** Cole as mensagens de erro e nomes de testes

**❌ Sem restrições:** Agente pode refatorar tudo
**✅ Restrições:** "NÃO altere código de produção" ou "Corrija apenas testes"

**❌ Output vago:** "Corrija" - você não sabe o que mudou
**✅ Específico:** "Retorne resumo da causa raiz e mudanças"

## Quando NÃO Usar

**Falhas relacionadas:** Corrigir uma pode corrigir outras - investigue junto primeiro
**Precisa de contexto completo:** Entender requer ver todo o sistema
**Debugging exploratório:** Você não sabe o que está quebrado ainda
**Estado compartilhado:** Agentes interfeririam (editando mesmos arquivos, usando mesmos recursos)

## Exemplo Real de Sessão

**Cenário:** 6 falhas de teste em 3 arquivos após refatoração maior

**Falhas:**
- agent-tool-abort.test.ts: 3 falhas (problemas de timing)
- batch-completion-behavior.test.ts: 2 falhas (ferramentas não executando)
- tool-approval-race-conditions.test.ts: 1 falha (contagem de execução = 0)

**Decisão:** Domínios independentes - lógica de aborto separada de conclusão em lote separada de race conditions

**Despacho:**
```
Agente 1 → Corrigir agent-tool-abort.test.ts
Agente 2 → Corrigir batch-completion-behavior.test.ts
Agente 3 → Corrigir tool-approval-race-conditions.test.ts
```

**Resultados:**
- Agente 1: Substituiu timeouts por espera baseada em eventos
- Agente 2: Corrigiu bug de estrutura de evento (threadId no lugar errado)
- Agente 3: Adicionou espera para conclusão de execução assíncrona de ferramenta

**Integração:** Todos os corrigios independentes, sem conflitos, suite completa verde

**Tempo economizado:** 3 problemas resolvidos em paralelo vs sequencialmente

## Benefícios Principais

1. **Paralelização** - Múltiplas investigações acontecem simultaneamente
2. **Foco** - Cada agente tem escopo estreito, menos contexto para rastrear
3. **Independência** - Agentes não interferem um com o outro
4. **Velocidade** - 3 problemas resolvidos no tempo de 1

## Verificação

Depois que agentes retornam:
1. **Revise cada resumo** - Entenda o que mudou
2. **Verifique conflitos** - Agentes editaram o mesmo código?
3. **Execute suite completa** - Verifique se todos os corrigios funcionam juntos
4. **Verificação pontual** - Agentes podem cometer erros sistemáticos

## Impacto no Mundo Real

Da sessão de debugging (2025-10-03):
- 6 falhas em 3 arquivos
- 3 agentes despachados em paralelo
- Todas as investigações completadas concorrentemente
- Todos os corrigios integrados com sucesso
- Zero conflitos entre mudanças de agentes