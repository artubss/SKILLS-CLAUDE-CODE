---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [resource] [method] [flags]
description: Google Workflow: Reuniões de hoje + tarefas em aberto como resumo de standup.
---

# Relatório de Standup do Google Workspace Workflow

Execute operações do Relatório de Standup do Google Workspace Workflow: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalado
- Autenticação configurada: Execute `gws auth status` para verificar
- Consulte `gws workflow-standup-report --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# workflow +standup-report

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para autenticação, flags globais e regras de segurança. Se ausente, execute `gws generate-skills` para criar.

Reuniões de hoje + tarefas em aberto como resumo de standup

## Uso

```bash
gws workflow +standup-report
```

## Flags

| Flag | Obrigatório | Padrão | Descrição |
|------|----------|---------|-------------|
| `--format` | — | — | Formato de saída: json (padrão), table, yaml, csv |

## Exemplos

```bash
gws workflow +standup-report
gws workflow +standup-report --format table
```

## Dicas

- Somente leitura — nunca modifica dados.
- Combina agenda de calendário (hoje) com lista de tarefas.

## Veja Também

- [gws-shared](../gws-shared/SKILL.md) — Flags globais e autenticação
- [gws-workflow](../gws-workflow/SKILL.md) — Todos os comandos de workflows de produtividade entre serviços

## Uso

```bash
# Listar recursos e métodos disponíveis
gws workflow-standup-report --help

# Inspecionar schema do método antes de chamar
gws schema workflow-standup-report.<resource>.<method>

# Executar comando com argumentos
gws workflow-standup-report $ARGUMENTS
```

## Tarefa

Execute a operação de Relatório de Standup do Workflow solicitada: $ARGUMENTS

1. **Verificar Pré-requisitos**
   - Verificar se `gws` está instalado: `gws --version`
   - Verificar autenticação: `gws auth status`
   - Consultar comandos disponíveis: `gws workflow-standup-report --help`

2. **Inspecionar Schema do Método**
   - Antes de chamar qualquer método, inspecione seus parâmetros
   - Use `gws schema` para entender os campos obrigatórios
   - Revise tipos de parâmetro e restrições

3. **Executar Operação**
   - Construa o comando com as flags apropriadas
   - Use `--params` para parâmetros de query/path
   - Use `--json` para corpo da requisição
   - Trate paginação com `--max-results` ou `--page-token`

4. **Tratamento de Erros**
   - Verifique a saída do comando para erros
   - Revise quotas de API e limites de taxa
   - Trate problemas de autenticação
   - Tente novamente falhas transitórias

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-workflow-standup-report`