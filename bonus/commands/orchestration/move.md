# Comando Mover Tarefa

Move tarefas entre pastas de status seguindo o protocolo de gerenciamento de tarefas.

## Uso

```
/task-move TASK-ID novo-status [motivo]
```

## Descrição

Atualiza o status da tarefa movendo arquivos entre pastas de status e atualizando informações de rastreamento. Segue todas as regras de protocolo incluindo validação e trilhas de auditoria.

## Comandos Básicos

### Começar a Trabalhar em uma Tarefa
```
/task-move TASK-001 in_progress
```
Move de todos → in_progress

### Completar Implementação
```
/task-move TASK-001 qa "Implementation complete, ready for testing"
```
Move de in_progress → qa

### Tarefa Passou em QA
```
/task-move TASK-001 completed "All tests passed"
```
Move de qa → completed

### Bloquear uma Tarefa
```
/task-move TASK-004 on_hold "Waiting for TASK-001 API completion"
```
Move para on_hold com motivo

### Desbloquear uma Tarefa
```
/task-move TASK-004 todos "Dependencies resolved"
```
Move de on_hold → todos

### QA Reprovado
```
/task-move TASK-001 in_progress "Failed integration test - fixing null pointer"
```
Move de qa → in_progress

## Operações em Massa

### Mover Múltiplas Tarefas
```
/task-move TASK-001,TASK-002,TASK-003 in_progress
```

### Mover por Filtro
```
/task-move --filter "priority:high status:todos" in_progress
```

### Mover com Padrão
```
/task-move TASK-00* qa "Batch testing ready"
```

## Regras de Validação

O comando impõe:
1. **Transições Válidas**: Apenas mudanças de status permitidas
2. **Uma Tarefa Por Agent**: Avisa se o agent tem tarefa em in_progress
3. **Verificação de Dependências**: Avisa se dependências não forem atendidas
4. **Existência de Arquivo**: Verifica se a tarefa existe antes de mover

## Mapa de Transição de Status

```
todos ──────→ in_progress ──────→ qa ──────→ completed
  ↓               ↓               ↓
  └───────────→ on_hold ←─────────┘
                  ↓
                todos/in_progress
```

## Opções

### Forçar Movimento
```
/task-move TASK-001 completed --force
```
Ignora validação (usar com cuidado)

### Simulação
```
/task-move TASK-001 qa --dry-run
```
Mostra o que aconteceria sem executar

### Com Atribuição
```
/task-move TASK-001 in_progress --assign dev-frontend
```
Atribui tarefa a um agent específico

### Com Estimativa de Tempo
```
/task-move TASK-001 in_progress --estimate 4h
```
Atualiza estimativa de tempo ao iniciar

## Tratamento de Erros

### Tarefa Não Encontrada
```
Error: TASK-999 not found in any status folder
Suggestion: Use /task-status to see available tasks
```

### Transição Inválida
```
Error: Cannot move from 'completed' to 'todos'
Valid transitions from completed: None (terminal state)
```

### Conflito de Agent
```
Warning: dev-frontend already has TASK-002 in progress
Continue? (y/n)
```

### Tarefa Bloqueada por Dependência
```
Warning: TASK-004 depends on TASK-001 (currently in_progress)
Moving to on_hold instead? (y/n)
```

## Automação

### Auto-mover ao Completar
```
/task-move TASK-001 --auto-progress
```
Move automaticamente para o próximo status quando condições forem atendidas

### Movimentos Agendados
```
/task-move TASK-005 in_progress --at "tomorrow 9am"
```

### Movimentos Condicionais
```
/task-move TASK-007 qa --when "TASK-006 completed"
```

## Exemplos

### Exemplo 1: Workflow do Desenvolvedor
```
# Começar trabalho
/task-move TASK-001 in_progress

# Completar e testar
/task-move TASK-001 qa "Implementation done, tests passing"

# Após revisão
/task-move TASK-001 completed "Code review approved"
```

### Exemplo 2: Tratando Bloqueios
```
# Bloquear por dependência
/task-move TASK-004 on_hold "Waiting for auth API from TASK-001"

# Desbloquear quando pronto
/task-move TASK-004 todos "TASK-001 now in QA, API available"
```

### Exemplo 3: Workflow de QA
```
# QA pega a tarefa
/task-move TASK-001 qa --assign qa-engineer

# Encontrados problemas
/task-move TASK-001 in_progress "Bug: handling empty responses"

# Corrigido e retestando
/task-move TASK-001 qa "Bug fixed, ready for retest"
```

## Detalhes da Atualização de Status

Cada movimento atualiza:
1. **Localização de Arquivo**: Movimento físico de arquivo
2. **Rastreador de Status**: Entrada TASK-STATUS-TRACKER.yaml
3. **Metadados da Tarefa**: Campo de status no arquivo de tarefa
4. **Rastreador de Execução**: Métricas gerais de progresso

## Melhores Práticas

1. **Sempre Forneça Motivos**: Especialmente para bloqueios e falhas
2. **Verifique Dependências**: Antes de mover para in_progress
3. **Atualize Estimativas**: Ao começar o trabalho
4. **Motivos de Bloqueio Claros**: Ajude outros a entender atrasos

## Integração

- Use após `/task-status` para ver tarefas disponíveis
- Atualizações refletidas em `/task-report`
- Dispara notificações se configurado
- Registra todos os movimentos para trilha de auditoria

## Notas

- Movimentos são atômicos — ou completam totalmente ou são revertidos
- Histórico de status é permanente e não pode ser editado
- Timestamp usa hora atual em formato ISO-8601
- Nome do agent é detectado automaticamente do contexto