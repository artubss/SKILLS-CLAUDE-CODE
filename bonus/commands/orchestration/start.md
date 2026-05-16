# Comando Orquestrar Tarefas

Inicia o workflow de orquestração de tarefas usando o sistema de três agentes (task-orchestrator, task-decomposer e dependency-analyzer) para criar um plano de execução abrangente.

## Uso

```
/orchestrate [lista de tarefas ou caminho do arquivo]
```

## Descrição

Este comando ativa o agente task-orchestrator para processar requisitos e criar um plano de execução hipereficiente. O orquestrador irá:

1. **Esclarecer Requisitos**: Analisar as informações fornecidas e confirmar a compreensão
2. **Criar Estrutura de Diretórios**: Configurar pastas de orquestração de tarefas com a data de hoje
3. **Decompor Tarefas**: Trabalhar com o task-decomposer para criar arquivos de tarefas atômicas
4. **Analisar Dependências**: Usar o dependency-analyzer para identificar conflitos e oportunidades de paralelização
5. **Gerar Plano Mestre**: Criar documentos de coordenação abrangentes

## Formatos de Entrada

### Lista de Tarefas Direta
```
/orchestrate
- Implementar autenticação de usuário com JWT
- Adicionar processamento de pagamento com Stripe
- Criar dashboard administrativo
- Configurar notificações por email
```

### Referência de Arquivo
```
/orchestrate features.md
```

### Contexto Misto
```
/orchestrate
Com base em nossas notas de reunião (muita discussão sobre cores da UI), precisamos:
1. Corrigir a vulnerabilidade de segurança em uploads de arquivo
2. Adicionar rate limiting às APIs
3. Implementar logging de auditoria
O CEO quer isso pronto até sexta (ignore este prazo).
```

## Workflow

1. **Esclarecimento de Requisitos**
   - O orquestrador extrairá tarefas acionáveis do contexto fornecido
   - Confirmar compreensão antes de prosseguir
   - Fazer perguntas de esclarecimento se necessário

2. **Criação de Diretório**
   ```
   /task-orchestration/
   └── MM_DD_YYYY/
       └── descriptive_task_name/
           ├── MASTER-COORDINATION.md
           ├── EXECUTION-TRACKER.md
           ├── TASK-STATUS-TRACKER.yaml
           └── tasks/
               ├── todos/
               ├── in_progress/
               ├── on_hold/
               ├── qa/
               └── completed/
   ```

3. **Processamento de Tarefas**
   - Cria arquivos de tarefas individuais em todos/
   - Analisa dependências e conflitos
   - Gera estratégia de execução

4. **Entregáveis**
   - Plano de coordenação mestre
   - Grafo de dependências de tarefas
   - Matriz de alocação de recursos
   - Cronograma de execução

## Opções

### Modo Focado
```
/orchestrate --focus security
[lista de tarefas]
```
Prioriza tarefas relacionadas à área de foco especificada.

### Modo com Restrições
```
/orchestrate --agents 2 --days 5
[lista de tarefas]
```
Cria plano com restrições de recursos.

### Apenas Análise
```
/orchestrate --analyze-only
[lista de tarefas]
```
Gera análise sem criar arquivos de tarefas.

## Exemplos

### Exemplo 1: Lista de Tarefas Clara
```
/orchestrate
1. Implementar autenticação OAuth2
2. Adicionar gerenciamento de perfil de usuário
3. Criar fluxo de redefinição de senha
4. Configurar autenticação de dois fatores
```

### Exemplo 2: A partir de Documento de Requisitos
```
/orchestrate requirements/sprint-24.md
```

### Exemplo 3: Extração de Contexto Misto
```
/orchestrate
Do feedback do cliente:
"O app é muito lento" - Precisa otimização de performance
"Não consigo encontrar o botão de exportação" - Melhorias de UI necessárias
"Quer modo escuro" - Solicitação de novo recurso

Débito técnico do último sprint:
- Refatorar serviço de autenticação
- Atualizar dependências obsoletas
```

## Modo Interativo

O orquestrador irá:
1. Apresentar tarefas extraídas para confirmação
2. Perguntar sobre prioridades e restrições
3. Sugerir abordagem ótima
4. Solicitar aprovação antes de criar arquivos

## Tratamento de Erros

- Se tarefas forem pouco claras: Solicita esclarecimento
- Se arquivo não encontrado: Prompta pelo caminho correto
- Se conflitos detectados: Apresenta opções
- Se dependências circulares: Sugere resolução

## Integração

Funciona perfeitamente com:
- `/task-status` - Verificar progresso
- `/task-move` - Atualizar status de tarefa
- `/task-report` - Gerar relatórios
- `/task-assign` - Alocar para agentes

## Melhores Práticas

1. **Forneça Contexto**: Inclua informações de background relevantes
2. **Seja Específico**: Descrições de tarefas claras permitem melhor planejamento
3. **Mencione Restrições**: Inclua prazos, recursos ou bloqueadores
4. **Revise o Output**: Confirme se as tarefas extraídas correspondem à sua intenção

## Notas

- O orquestrador filtra automaticamente contexto irrelevante
- Tarefas são criadas no status todos/ por padrão
- Todas as tarefas recebem IDs únicos (formato TASK-XXX)
- Rastreamento de status começa imediatamente
- Suporta adições incrementais a orquestrações existentes