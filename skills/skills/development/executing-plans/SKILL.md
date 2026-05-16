---
name: executing-plans
description: Use quando você tiver um plano de implementação escrito para executar em uma sessão separada com pontos de verificação
---

# Executando Planos

## Visão Geral

Carregue o plano, revise criticamente, execute tarefas em lotes, relate para revisão entre lotes.

**Princípio central:** Execução em lotes com pontos de verificação para revisão do arquiteto.

**Anuncie no início:** "Estou usando a skill executing-plans para implementar este plano."

## O Processo

### Passo 1: Carregar e Revisar Plano
1. Leia o arquivo do plano
2. Revise criticamente - identifique dúvidas ou preocupações sobre o plano
3. Se houver preocupações: Levante-as com seu parceiro humano antes de começar
4. Se não houver preocupações: Crie TodoWrite e prossiga

### Passo 2: Executar Lote
**Padrão: Primeiras 3 tarefas**

Para cada tarefa:
1. Marque como in_progress
2. Siga cada passo exatamente (o plano possui passos pequenos e bem definidos)
3. Execute verificações conforme especificado
4. Marque como concluída

### Passo 3: Relatar
Quando o lote estiver completo:
- Mostre o que foi implementado
- Mostre resultado da verificação
- Diga: "Pronto para feedback."

### Passo 4: Continuar
Com base no feedback:
- Aplique mudanças se necessário
- Execute próximo lote
- Repita até conclusão

### Passo 5: Completar Desenvolvimento

Após todas as tarefas serem concluídas e verificadas:
- Anuncie: "Estou usando a skill finishing-a-development-branch para completar este trabalho."
- **SUB-SKILL OBRIGATÓRIA:** Use superpowers:finishing-a-development-branch
- Siga essa skill para verificar testes, apresentar opções, executar escolha

## Quando Parar e Pedir Ajuda

**PARE a execução imediatamente quando:**
- Encontre um bloqueador durante o lote (dependência ausente, teste falha, instrução confusa)
- O plano tiver lacunas críticas impedindo o início
- Você não entender uma instrução
- Verificação falhar repetidamente

**Peça esclarecimento em vez de adivinhar.**

## Quando Revisitar Passos Anteriores

**Volte para Revisão (Passo 1) quando:**
- Seu parceiro atualizar o plano baseado em seu feedback
- A abordagem fundamental precisar ser repensada

**Não force através de bloqueadores** - pare e pergunte.

## Lembre-se
- Revise o plano criticamente primeiro
- Siga os passos do plano exatamente
- Não pule verificações
- Referencie skills quando o plano disser para fazer
- Entre lotes: apenas relate e aguarde
- Pare quando bloqueado, não adivinhe