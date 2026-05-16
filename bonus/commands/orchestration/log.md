# Comando de Log de Orquestração

Registre trabalho de tarefas orquestradas em ferramentas externas de gerenciamento de projetos como Linear, Obsidian, Jira ou GitHub Issues.

## Uso

```
/orchestration/log [TASK-ID] [options]
```

## Descrição

Cria automaticamente logs de trabalho nas suas ferramentas conectadas de gerenciamento de projetos ou bases de conhecimento, transferindo dados de conclusão de tarefas, tempo gasto e notas de progresso para manter os sistemas externos sincronizados.

## Comandos Básicos

### Registrar Tarefa Atual
```
/orchestration/log
```
Registra a tarefa atualmente em andamento nas ferramentas disponíveis.

### Registrar Tarefa Específica
```
/orchestration/log TASK-003
```
Registra o trabalho de uma tarefa específica.

### Escolher Destino
```
/orchestration/log TASK-003 --choose
```
Seleciona manualmente onde registrar o trabalho.

## Seleção de Destino

Quando múltiplas ferramentas estão disponíveis ou nenhuma conexão óbvia existe:

```
Onde você gostaria de registrar este trabalho?

Destinos disponíveis:
1. Linear (ENG-1234 detectado)
2. Obsidian (Nota Diária)
3. Obsidian (Projeto: Autenticação)
4. GitHub Issue (#123)
5. Nenhum - Pular registro

Escolha o destino [1-5]: 
```

## Integração com Obsidian

### Registro em Nota Diária
```
/orchestration/log --obsidian-daily
```
Adiciona à nota diária de hoje:

```markdown
## Work Log - 15:30

### TASK-003: Implementação JWT ✅

**Tempo Gasto**: 4.5 horas (10:00 - 14:30)
**Status**: Concluído → QA

**O que fiz:**
- Implementei middleware de validação de token JWT
- Adicionei lógica de token de atualização
- Criei suite de testes abrangente
- Corrigi caso extremo com expiração de token

**Estatísticas de Código:**
- Arquivos: 8 modificados
- Linhas: +245 -23
- Cobertura: 95%

**Tarefas Relacionadas:**
- Próxima: [[TASK-005]] - API de Perfil de Usuário
- Bloqueada: [[TASK-007]] - Aguardando esta

**Commits:**
- `abc123`: feat(auth): implementar validação JWT
- `def456`: test(auth): adicionar testes de validação

#tasks/completed #project/authentication
```

### Registro em Nota de Projeto
```
/orchestration/log --obsidian-project "Sistema de Autenticação"
```
Cria ou adiciona a nota específica do projeto.

### Localização Customizada no Obsidian
```
/orchestration/log --obsidian-path "Projects/Sprint 24/Work Log"
```

## Integração com Linear
```
/orchestration/log TASK-003 --linear-issue ENG-1234
```
Cria comentário de log de trabalho no issue do Linear.

## Detecção Inteligente

O sistema detecta destinos disponíveis:

```
Analisando contexto da tarefa...

Conexões encontradas:
✓ Linear: ENG-1234 (do nome da branch)
✓ Obsidian: Nota de projeto existe
✓ GitHub: Sem referência de issue
✗ Jira: Não conectado

Sugestão: Linear ENG-1234
Usar sugestão? [S/n/escolher diferente]
```

## Formatos de Log de Trabalho

### Formato Obsidian
```markdown
## 📋 Tarefa: TASK-003 - Implementação JWT

### Resumo
- **Status**: 🟢 Concluído
- **Duração**: 4h 30m
- **Data**: 15/03/2024

### Detalhes do Progresso
- [x] Design da estrutura de token
- [x] Middleware de validação
- [x] Mecanismo de atualização
- [x] Cobertura de testes

### Notas Técnicas
- Usado algoritmo RS256 para assinatura
- Tokens expiram após 15 minutos
- Tokens de atualização duram 7 dias

### Links
- Linear: [ENG-1234](linear://issue/ENG-1234)
- PR: [#456](github.com/...)
- Docs: [[JWT Implementation Guide]]

### Próximas Ações
- [ ] Feedback da revisão de código
- [ ] Deploy em staging
- [ ] Atualizar documentação da API

---
*Registrado via Task Orchestration às 15:30*
```

### Formato Linear
```
Comentário de log de trabalho no Linear com detalhes da tarefa, rastreamento de tempo e atualizações de progresso.
```

## Registro em Múltiplos Destinos

```
/orchestration/log TASK-003 --multi

Selecione todos os destinos para registro:
[x] Linear - ENG-1234
[x] Obsidian - Nota Diária
[ ] Obsidian - Nota de Projeto
[ ] GitHub - Criar novo issue

Pressione Enter para confirmar, Espaço para alternar
```

## Operações em Lote

### Resumo Diário para Obsidian
```
/orchestration/log --daily-summary --obsidian

Cria resumo na nota diária:

## Resumo de Trabalho - 15/03/2024

### Tarefas Concluídas
- [[TASK-003]]: Implementação JWT (4.5h) ✅
- [[TASK-008]]: Atualizações da UI de Login (2h) ✅

### Em Progresso
- [[TASK-005]]: API de Perfil de Usuário (1.5h) 🔄

### Tempo Total: 8 horas

### Principais Conquistas
- Núcleo do sistema de autenticação completo
- Todos os testes passando
- Pronto para revisão de código

### Foco para Amanhã
- Completar endpoints de perfil de usuário
- Iniciar integração OAuth
```

### Relatório Semanal
```
/orchestration/log --weekly --obsidian-path "Weekly Reviews/Week 11"
```

## Templates

### Configurar Template do Obsidian
```yaml
obsidian_template:
  daily_note:
    heading: "## Work Log - {time}"
    include_stats: true
    add_tags: true
    link_tasks: true
  
  project_note:
    create_if_missing: true
    append_to_section: "## Task Progress"
    include_commits: true
```

### Configurar Template do Linear
```yaml
linear_template:
  include_time: true
  update_status: true
  add_labels: ["from-orchestration"]
```

## Modo Interativo

```
/orchestration/log --interactive

Tarefa: TASK-003 - Implementação JWT
Status: Concluído
Tempo: 4.5 horas

Onde registrar? (Espaço para selecionar, Enter para confirmar)
> [x] Linear (ENG-1234)
> [x] Obsidian Nota Diária
> [ ] Obsidian Nota de Projeto
> [ ] Novo GitHub Issue

Adicionar notas customizadas? [s/N]: s
> Implementado usando RS256, pronto para revisão

Registrando em 2 destinos...
✓ Linear: Comentário adicionado a ENG-1234
✓ Obsidian: Adicionado à nota diária

Visualizar logs? [s/N]: 
```

## Exemplos

### Exemplo 1: Registro de Fim de Dia
```
/orchestration/log --eod

Resumo de Fim de Dia:
- 3 tarefas trabalhadas
- 7.5 horas registradas
- 2 concluídas, 1 em andamento

Registrar em:
1. Obsidian Nota Diária (recomendado)
2. Linear (atualizar todos os 3 issues)
3. Ambos
4. Pular

Escolha [1]: 1

✓ Log de trabalho diário criado no Obsidian
```

### Exemplo 2: Revisão de Sprint
```
/orchestration/log --sprint-review --week 11

Coletando dados da semana 11...
- 15 tarefas concluídas
- 3 em andamento
- 52 horas registradas

Criar revisão de sprint em:
1. Obsidian - "Sprint Reviews/Sprint 24"
2. Linear - Ciclo Sprint 24
3. Ambos

Escolha [3]: 3

✓ Revisão de sprint criada em ambos os sistemas
```

### Exemplo 3: Nenhuma Conexão Encontrada
```
/orchestration/log TASK-009

Nenhum destino automático encontrado para TASK-009.

Onde você gostaria de registrar isto?
1. Obsidian - Nota Diária
2. Obsidian - Criar Nota de Projeto
3. Linear - Buscar issue
4. GitHub - Criar novo issue
5. Pular registro

Escolha: 2

Digite o nome do projeto: Auditoria de Segurança
✓ Nota "Auditoria de Segurança" criada com log de trabalho
```

## Configuração

### Destinos Padrão
```yaml
log_defaults:
  no_connection: "ask"  # ask|obsidian-daily|skip
  multi_connection: "ask"  # ask|all|first
  
  obsidian:
    default_location: "daily"  # daily|project|custom
    project_folder: "Projects"
    daily_folder: "Daily Notes"
  
  linear:
    auto_update_status: true
    include_commits: true
```

## Melhores Práticas

1. **Defina Preferências**: Configure destinos padrão
2. **Vincule Cedo**: Conecte tarefas às ferramentas de PM ao criar
3. **Use Notas Diárias**: Ótimo para rastreamento pessoal
4. **Notas de Projeto**: Melhor para colaboração em equipe
5. **Sincronizações Regulares**: Não deixe logs se acumularem

## Notas

- Respeita conexões e permissões do MCP
- Logs do Obsidian criam backlinks automaticamente
- Suporta múltiplos destinos simultâneos
- Preserva formatação entre sistemas
- Pode ser automatizado com mudanças de status de tarefas