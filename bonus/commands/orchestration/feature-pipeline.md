# Pipeline de Funcionalidades

Execute tarefas de implementação de um documento de design usando checkboxes em markdown.

## Detecção de Entrada

`$ARGUMENTS` deve ser um caminho para um arquivo markdown de design (ex: `docs/designs/xxx.md`)

Se vazio ou pouco claro, solicite ao usuário o caminho do arquivo de design.

## Fase 1: Inicialização

1. Leia o arquivo de design
2. Execute `python3 .claude/skills/task-execution-engine/scripts/task_manager.py status --file <design.md>` para mostrar o progresso atual
3. Se todas as tarefas foram concluídas, reporte e encerre
4. Caso contrário, prossiga para a execução

## Fase 2: Loop de Execução

**MODO AUTOMÁTICO - Sem perguntas, sem pausas**

```
LOOP:
  1. EXECUTE: task_manager.py next --file <design.md> --json
  2. SE nenhuma tarefa disponível → SAÍDA para Fase 3
  3. LEIA detalhes da tarefa (arquivos, critérios)
  4. IMPLEMENTE a tarefa
     - Crie/modifique arquivos conforme especificado
     - Siga padrões do repositório
     - Execute testes se aplicável
  5. VERIFIQUE critérios de aceitação
  6. ATUALIZE status:
     - Sucesso: task_manager.py done --file <design.md> --task "Título"
     - Falha: task_manager.py fail --file <design.md> --task "Título" --reason "..."
  7. EXIBA resumo do resultado
  8. CONTINUE (volte ao passo 1)
```

## Fase 3: Conclusão

1. Execute `task_manager.py status --file <design.md>` para mostrar resumo final
2. Reporte:
   - Tarefas concluídas
   - Tarefas falhadas (com motivos)
   - Arquivos modificados
3. Pergunte se o usuário deseja fazer commit das mudanças no git

## Diretrizes de Implementação

### Para Cada Tarefa:

1. **Leia arquivos** especificados na linha `files:` da tarefa
2. **Entenda os critérios** a partir dos itens do checkbox
3. **Implemente** seguindo padrões existentes no repositório
4. **Verifique** se cada critério de aceitação foi atendido
5. **Atualize** o arquivo de design com o status de conclusão

### Tomada de Decisão:

- Se não estiver claro sobre detalhes de implementação → use padrões do repositório
- Se bloqueado por dependência ausente → marque como falho, continue
- Se precisar de recurso externo → marque como falho com motivo, continue

### Atualizações de Status:

```bash
# Marque tarefa concluída (atualiza checkbox para [x] ✅)
python3 .claude/skills/task-execution-engine/scripts/task_manager.py done \
  --file <design.md> --task "Título da Tarefa"

# Marque tarefa falhada (atualiza checkbox para [x] ❌ com motivo)
python3 .claude/skills/task-execution-engine/scripts/task_manager.py fail \
  --file <design.md> --task "Título da Tarefa" --reason "Descrição do erro"
```

## Formato de Saída

Após cada tarefa:

```
---TASK RESULT---
task: Título da Tarefa
status: completed|failed
files: [lista de arquivos modificados]
notes: Descrição breve
---END TASK RESULT---
```

## Tratamento de Erros

| Erro | Ação |
|-------|--------|
| Falha na implementação da tarefa | Marque como falho, continue para próxima |
| Erro de script | Registre erro, tente novamente uma vez, então marque como falho |
| Nenhuma tarefa disponível | Saia do loop, mostre resumo |
| Arquivo não encontrado | Solicite ao usuário o caminho correto |

## Regras Principais

1. **NUNCA interrompa** no meio do loop de execução
2. **NUNCA faça perguntas** durante a execução
3. **SEMPRE atualize** o status da tarefa no arquivo de design
4. **Saía apenas** quando nenhuma tarefa pendente permanecer