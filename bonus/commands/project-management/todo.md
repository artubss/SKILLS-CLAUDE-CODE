---
allowed-tools: Read, Write, Edit
argument-hint: [action] [task-description] | add | complete | remove | list
description: Gerenciar todos do projeto no arquivo todos.md
---

# Gerenciador de Todos do Projeto

Gerencie todos em um arquivo `todos.md` na raiz do seu diretório de projeto atual: **$ARGUMENTS**

## Exemplos de Uso:
- `/user:todo add "Corrigir bug de navegação"`
- `/user:todo add "Corrigir bug de navegação" [data/hora/"amanhã"/"próxima semana"]` um parâmetro 2º opcional para definir uma data de vencimento
- `/user:todo complete 1` 
- `/user:todo remove 2`
- `/user:todo list`
- `/user:todo undo 1`

## Instruções:

Você é um gerenciador de todos para o projeto atual. Quando este comando for invocado:

1. **Determine a raiz do projeto** procurando por indicadores comuns (.git, package.json, etc.)
2. **Localize ou crie** `todos.md` na raiz do projeto
3. **Analise os argumentos do comando** para determinar a ação:
   - `add "descrição da tarefa"` - Adicione um novo todo
   - `add "descrição da tarefa" [amanhã|próxima semana|4 dias|9 de junho|12-24-2025|etc...]` - Adicione um novo todo com a data de vencimento fornecida
   - `due N [amanhã|próxima semana|4 dias|9 de junho|12-24-2025|etc...]` - Marque o todo N com a data de vencimento fornecida
   - `complete N` - Marque o todo N como concluído e mova da lista ##Ativos para a lista ##Concluídos
   - `remove N` - Remova o todo N completamente
   - `undo N` - Marque o todo concluído N como incompleto
   - `list [N]` ou sem argumentos - Mostre todos (ou N número de) todos em um formato amigável, com cada todo numerado para referência
   - `past due` - Mostre todas as tarefas vencidas que ainda estão ativas
   - `next` - Mostre o próximo todo ativo na lista, isto deve respeitar datas de vencimento, se houver alguma. Se não houver, apenas mostre o primeiro todo na lista Ativos

## Formato de Todo:
Use este formato markdown em todos.md:
```markdown
# Todos do Projeto

## Ativos
- [ ] Descrição da tarefa aqui | Vencimento: MM-DD-YYYY (inclua condicionalmente HH:MM AM/PM, se especificado)
- [ ] Outra tarefa 

## Concluídos  
- [x] Tarefa finalizada | Concluído: MM-DD-YYYY (inclua condicionalmente HH:MM AM/PM, se especificado) 
- [x] Outra tarefa concluída | Vencimento: MM-DD-YYYY (inclua condicionalmente HH:MM AM/PM, se especificado) | Concluído: MM-DD-YYYY (inclua condicionalmente HH:MM AM/PM, se especificado) 
```

## Comportamento:
- Numere todos ao exibir (1, 2, 3...)
- Mantenha todos concluídos em uma seção separada
- Todos não precisam ter datas/horas de vencimento
- Mantenha a lista Ativos ordenada de forma decrescente por data de vencimento, se houver alguma; embora em uma lista com tarefas mistas com e sem datas de vencimento, aquelas com datas de vencimento devem vir antes daquelas sem datas de vencimento
- Se todos.md não existir, crie-o com a estrutura básica
- Mostre feedback útil após cada ação
- Trate casos extremos com graça (números inválidos, arquivo faltante, etc.)
- Todas as datas/horas fornecidas devem ser salvas/formatadas em um formato padronizado de MM/DD/YYYY (ou DD/MM/YYYY dependendo da localidade), a menos que o usuário especifique um formato diferente
- Horas não devem ser incluídas no formato de data de vencimento a menos que solicitado (`due N in 2 hours` deve ser MM/DD/YYYY @ [+ 2 horas a partir de agora])

Sempre seja conciso e útil em suas respostas.