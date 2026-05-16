# Comando Localizar Tarefa

Pesquise e localize tarefas em todas as orquestrações usando vários critérios.

## Uso

```
/task-find [search-term] [options]
```

## Descrição

Funcionalidade de busca poderosa para localizar rapidamente tarefas por ID, conteúdo, status, dependências ou qualquer outro critério. Suporta regex, fuzzy matching e consultas complexas.

## Busca Básica

### Por ID da Tarefa
```
/task-find TASK-001
/task-find TASK-*
```

### Por Título/Conteúdo
```
/task-find "authentication"
/task-find "payment processing"
```

### Por Status
```
/task-find --status in_progress
/task-find --status qa,completed
```

## Busca Avançada

### Expressão Regular
```
/task-find --regex "JWT|OAuth"
/task-find --regex "TASK-0[0-9]{2}"
```

### Busca Difusa
```
/task-find --fuzzy "autentication"  # encontra "authentication"
/task-find --fuzzy "paymnt"         # encontra "payment"
```

### Múltiplos Critérios
```
/task-find --status todos --priority high --type feature
/task-find --agent dev-backend --created-after yesterday
```

## Operadores de Busca

### Operadores Booleanos
```
/task-find "auth AND login"
/task-find "payment OR billing"
/task-find "security NOT test"
```

### Busca Específica por Campo
```
/task-find title:"user authentication"
/task-find description:"security vulnerability"
/task-find agent:dev-frontend
/task-find blocks:TASK-001
```

### Intervalos de Data
```
/task-find --created "2024-03-10..2024-03-15"
/task-find --modified "last 3 days"
/task-find --completed "this week"
```

## Formatos de Saída

### Visualização de Lista Padrão
```
Found 3 tasks matching "authentication":

TASK-001: Implement JWT authentication
  Status: in_progress | Agent: dev-frontend | Created: 2024-03-15
  Location: /task-orchestration/03_15_2024/auth_system/tasks/in_progress/

TASK-004: Add OAuth2 authentication  
  Status: todos | Priority: high | Blocked by: TASK-001
  Location: /task-orchestration/03_15_2024/auth_system/tasks/todos/

TASK-007: Authentication middleware tests
  Status: todos | Type: test | Depends on: TASK-001
  Location: /task-orchestration/03_15_2024/auth_system/tasks/todos/
```

### Visualização Detalhada
```
/task-find TASK-001 --detailed
```
Mostra o conteúdo completo da tarefa, incluindo descrição, notas de implementação e histórico.

### Visualização em Árvore
```
/task-find --tree --root TASK-001
```
Mostra a tarefa e todas as suas dependências em formato de árvore.

## Opções de Filtro

### Por Orquestração
```
/task-find --orchestration "03_15_2024/payment_system"
/task-find --orchestration "*/auth_*"
```

### Por Propriedades
```
/task-find --has-dependencies
/task-find --no-dependencies
/task-find --blocking-others
/task-find --effort ">4h"
```

### Por Relacionamentos
```
/task-find --depends-on TASK-001
/task-find --blocks TASK-005
/task-find --related-to TASK-003
```

## Buscas Especiais

### Localizar Dependências Circulares
```
/task-find --circular-deps
```

### Localizar Tarefas Órfãs
```
/task-find --orphaned
```

### Localizar Tarefas Duplicadas
```
/task-find --duplicates
```

### Localizar Tarefas Obsoletas
```
/task-find --stale --days 7
```

## Filtros Rápidos

### Pronto para Iniciar
```
/task-find --ready
```
Mostra tarefas em todos com nenhuma dependência bloqueante.

### Caminho Crítico
```
/task-find --critical-path
```
Mostra tarefas no caminho crítico.

### Alto Impacto
```
/task-find --high-impact
```
Mostra tarefas que bloqueiam várias outras.

## Opções de Exportação

### Copiar Resultados
```
/task-find "auth" --copy
```
Copia resultados para a área de transferência.

### Exportar Paths
```
/task-find --status todos --export paths
```
Exporta caminhos de arquivo para operações em lote.

### Gerar Relatório
```
/task-find --report
```
Cria relatório detalhado de busca.

## Exemplos

### Exemplo 1: Localizar Trabalho para um Agent
```
/task-find --status todos --suitable-for dev-frontend --ready
```

### Exemplo 2: Localizar Problemas Bloqueantes
```
/task-find --status on_hold --show-blockers
```

### Exemplo 3: Auditoria de Segurança
```
/task-find "security OR auth OR permission" --type "feature,bugfix"
```

### Exemplo 4: Planejamento de Sprint
```
/task-find --status todos --effort "<4h" --no-dependencies
```

## Atalhos de Busca

### Tarefas Recentes
```
/task-find --recent 10
```

### Minhas Tarefas
```
/task-find --mine  # Usa contexto atual do agent
```

### Modificadas Hoje
```
/task-find --modified today
```

## Consultas Complexas

### Busca Composta
```
/task-find '(title:"auth" OR description:"security") AND status:todos AND -blocks:*'
```

### Buscas Salvas
```
/task-find --save "security-todos"
/task-find --load "security-todos"
```

## Dicas de Performance

1. **Use Índices**: Buscas por Status e ID são mais rápidas
2. **Restrinja o Escopo**: Especifique orquestração quando possível
3. **Armazene em Cache**: Use `--cache` para buscas repetidas
4. **Limite Resultados**: Use `--limit 20` para grandes conjuntos de resultados

## Integração

### Com Outros Comandos
```
/task-find "payment" --status todos | /task-move in_progress
```

### Operações em Lote
```
/task-find --filter "priority:low" | /task-update priority:medium
```

## Notas

- Pesquisa em todos os arquivos de tarefas em task-orchestration/
- Insensível a maiúsculas e minúsculas por padrão (use --case para sensível a maiúsculas)
- Resultados ordenados por relevância, a menos que especificado de outra forma
- Suporta encadeamento de comandos com operador pipe
- Índice de busca atualizado automaticamente em mudanças de arquivo