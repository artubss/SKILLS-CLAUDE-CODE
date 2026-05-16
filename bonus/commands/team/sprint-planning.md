---
allowed-tools: Read, WebSearch
argument-hint: [sprint-duration] | [start-date] [duration]
description: Planejar e organizar fluxos de trabalho de sprint com integração Linear e análise de capacidade
---

# Planejamento de Sprint

Planejar e organizar sprint: $ARGUMENTS

## Contexto Atual do Sprint

- Sprint atual: Verificar milestones do Linear ou GitHub
- Velocidade do time: Analisar desempenho de sprints recentes
- Issues abertas: !`gh issue list --limit 10 --state open` (se GitHub CLI disponível)
- Estrutura do projeto: @README.md ou @.github/ (se existir)

## Instruções

1. **Verificar Integração Linear**
Primeiro, verifique se o servidor Linear MCP está conectado:
- Se conectado: Prosseguir com integração completa
- Se não conectado: Solicitar ao usuário instalar o servidor Linear MCP de https://github.com/modelcontextprotocol/servers
- Fallback: Usar issues do GitHub e entrada manual

2. **Coletar Contexto do Sprint**
Colete as seguintes informações:
- Duração do sprint (ex: 2 semanas)
- Data de início do sprint
- Membros do time envolvidos
- Objetivos/temas do sprint
- Velocidade de sprint anterior (se disponível)

3. **Analisar Estado Atual**

#### Com Linear Conectado:
```
1. Buscar todos os itens do backlog do Linear
2. Obter tarefas em andamento e seu status
3. Analisar prioridades de tarefas e dependências
4. Verificar atribuições de membros do time e capacidade
5. Revisar tarefas bloqueadas e impedimentos
```

#### Sem Linear (Fallback):
```
1. Analisar issues do GitHub por labels e milestones
2. Revisar pull requests abertos e seu status
3. Verificar atividade de commit recente
4. Solicitar ao usuário contexto adicional sobre tarefas
```

4. **Análise de Planejamento de Sprint**

Gerar um plano de sprint abrangente incluindo:

```markdown
# Relatório de Planejamento de Sprint - [Nome do Sprint]

## Visão Geral do Sprint
- Duração: [Data de Início] até [Data de Término]
- Membros do Time: [Lista]
- Objetivo do Sprint: [Descrição]

## Análise de Capacidade
- Horas Disponíveis Totais: [Cálculo]
- Velocidade do Sprint Anterior: [Pontos/Horas]
- Capacidade Recomendada: [80-85% do total]

## Backlog Proposto do Sprint

### Tarefas de Alta Prioridade
1. [ID da Tarefa] - [Título]
   - Estimativa: [Pontos/Horas]
   - Responsável: [Nome]
   - Dependências: [Lista]
   - Critérios de Aceitação: [Detalhes]

### Tarefas de Média Prioridade
[Formato similar]

### Tarefas Desejáveis
[Formato similar]

## Avaliação de Riscos
- Riscos Técnicos: [Lista]
- Riscos de Recursos: [Lista]
- Riscos de Dependência: [Lista]

## Recomendações
1. [Recomendações específicas baseadas em análise]
2. [Sugestões de alocação de recursos]
3. [Estratégias de mitigação de riscos]

## Métricas para Acompanhamento
- Velocidade
- Taxa de burndown
- Tempo bloqueado
- Cycle time
```

5. **Sessão de Planejamento Interativa**

Ofertar ajuda com:
1. Decomposição e estimativa de tarefas
2. Mapeamento de dependências
3. Balanceamento de recursos
4. Planejamento de mitigação de riscos
5. Finalização do backlog do sprint

## Tratamento de Erros

### Problemas de Conexão Linear
```
"Percebi que o servidor Linear MCP não está conectado. Para obter o máximo valor do planejamento de sprint:

1. Instale o servidor Linear MCP:
   npm install -g @modelcontextprotocol/server-linear

2. Configure com sua chave de API Linear
3. Reinicie esta sessão com o Linear conectado

Por enquanto, vou prosseguir usando dados do GitHub e entrada manual."
```

### Dados Faltando
```
"Preciso de mais informações para criar um plano de sprint efetivo:
- [Listar dados específicos faltando]
- [Fornecer templates para entrada manual]
```

## Exemplo de Uso

### Entrada do Usuário:
"Me ajude a planejar nosso próximo sprint de 2 semanas começando segunda"

### Resposta do Assistente:
```
Vou ajudá-lo a planejar seu sprint. Deixe-me verificar sua integração Linear primeiro...

[Se conectado]: Ótimo! Consigo ver seu workspace Linear. Deixe-me analisar seu backlog...

[Resultados da análise]

Baseado na minha análise, aqui está seu plano de sprint proposto:

**Sprint 24-01 (08 a 19 de janeiro)**

**Escopo Recomendado (32 story points):**
1. ENG-123: Implementar autenticação de usuário (8 pts) - João
2. ENG-124: Scripts de migração de banco de dados (5 pts) - Sara
3. ENG-125: Rate limiting de API (5 pts) - Mike
...

**Riscos Principais:**
- ENG-123 bloqueia 3 outras tarefas
- Sara tem 20% de alocação para suporte

Gostaria que eu:
1. Ajustasse o escopo baseado em prioridades diferentes?
2. Criasse uma visualização de dependências?
3. Gerasse agenda da reunião de planejamento de sprint?
```

## Boas Práticas

1. **Sempre verificar capacidade**: Não sobrecarregue o time
2. **Incluir tempo de buffer**: Planejar para 80-85% de capacidade
3. **Considerar dependências**: Mapear relacionamentos de tarefas
4. **Equilibrar carga de trabalho**: Distribuir tarefas uniformemente
5. **Definir objetivos claros**: Garantir que o sprint tenha focos bem definidos
6. **Planejar para desconhecidos**: Incluir tempo para spike/investigação

## Pontos de Integração

- Linear: Gerenciamento e rastreamento de tarefas
- GitHub: Repositório de código e PRs
- Slack: Comunicação do time (se MCP disponível)
- Calendário: Disponibilidade do time (se acessível)

## Formatos de Saída

Oferecer múltiplas opções de saída:
1. Relatório Markdown (padrão)
2. CSV para importação em planilha
3. JSON para ferramentas de automação
4. Formato compatível com Linear para importação direta