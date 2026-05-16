---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [recurso] [método] [flags]
description: Google Workflow: Workflows de produtividade entre serviços.
---

# Google Workspace Workflow

Execute operações de Google Workspace Workflow: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalado
- Autenticação configurada: Execute `gws auth status` para verificar
- Revise `gws workflow --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# workflow (v1)

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para autenticação, flags globais e regras de segurança. Se ausente, execute `gws generate-skills` para criar.

```bash
gws workflow <recurso> <método> [flags]
```

## Comandos Auxiliares

| Comando | Descrição |
|---------|-----------|
| [`+standup-report`](../gws-workflow-standup-report/SKILL.md) | Reuniões de hoje + tarefas abertas como resumo de standup |
| [`+meeting-prep`](../gws-workflow-meeting-prep/SKILL.md) | Prepare-se para sua próxima reunião: agenda, participantes e docs relacionados |
| [`+email-to-task`](../gws-workflow-email-to-task/SKILL.md) | Converta uma mensagem do Gmail em uma entrada de Google Tasks |
| [`+weekly-digest`](../gws-workflow-weekly-digest/SKILL.md) | Resumo semanal: reuniões desta semana + contagem de emails não lidos |
| [`+file-announce`](../gws-workflow-file-announce/SKILL.md) | Anuncie um arquivo do Drive em um espaço Chat |

## Descobrindo Comandos

Antes de chamar qualquer método de API, inspecione-o:

```bash
# Procure recursos e métodos
gws workflow --help

# Inspecione os parâmetros obrigatórios, tipos e padrões de um método
gws schema workflow.<recurso>.<método>
```

Use a saída de `gws schema` para construir seus flags `--params` e `--json`.

## Uso

```bash
# Liste recursos e métodos disponíveis
gws workflow --help

# Inspecione o schema do método antes de chamar
gws schema workflow.<recurso>.<método>

# Execute comando com argumentos
gws workflow $ARGUMENTS
```

## Tarefa

Execute a operação de Workflow solicitada: $ARGUMENTS

1. **Verifique Pré-requisitos**
   - Verifique se `gws` está instalado: `gws --version`
   - Verifique autenticação: `gws auth status`
   - Revise comandos disponíveis: `gws workflow --help`

2. **Inspecione o Schema do Método**
   - Antes de chamar qualquer método, inspecione seus parâmetros
   - Use `gws schema` para entender campos obrigatórios
   - Revise tipos de parâmetros e restrições

3. **Execute a Operação**
   - Construa comando com flags apropriadas
   - Use `--params` para parâmetros de query/path
   - Use `--json` para corpo da solicitação
   - Manipule paginação com `--max-results` ou `--page-token`

4. **Tratamento de Erros**
   - Verifique a saída do comando para erros
   - Revise quotas de API e limites de taxa
   - Trate problemas de autenticação
   - Tente novamente falhas transitórias

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-workflow`