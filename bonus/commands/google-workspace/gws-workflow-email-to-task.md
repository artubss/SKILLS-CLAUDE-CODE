---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [resource] [method] [flags]
description: Google Workflow: Converter uma mensagem do Gmail em uma entrada do Google Tasks.
---

# Google Workspace Workflow Email To Task

Execute operações do Google Workspace Workflow Email To Task: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalado
- Autenticação configurada: Execute `gws auth status` para verificar
- Revise `gws workflow-email-to-task --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# workflow +email-to-task

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para autenticação, flags globais e regras de segurança. Se não existir, execute `gws generate-skills` para criá-lo.

Converter uma mensagem do Gmail em uma entrada do Google Tasks

## Uso

```bash
gws workflow +email-to-task --message-id <ID>
```

## Flags

| Flag | Obrigatório | Padrão | Descrição |
|------|-------------|--------|-----------|
| `--message-id` | ✓ | — | ID da mensagem do Gmail a converter |
| `--tasklist` | — | @default | ID da lista de tarefas (padrão: @default) |

## Exemplos

```bash
gws workflow +email-to-task --message-id MSG_ID
gws workflow +email-to-task --message-id MSG_ID --tasklist LIST_ID
```

## Dicas

- Lê o assunto do email como título da tarefa e snippet como notas.
- Cria uma nova tarefa — confirme com o usuário antes de executar.

## Veja Também

- [gws-shared](../gws-shared/SKILL.md) — Flags globais e autenticação
- [gws-workflow](../gws-workflow/SKILL.md) — Todos os comandos de workflows de produtividade entre serviços

## Uso

```bash
# Listar recursos e métodos disponíveis
gws workflow-email-to-task --help

# Inspecionar schema do método antes de chamar
gws schema workflow-email-to-task.<resource>.<method>

# Executar comando com argumentos
gws workflow-email-to-task $ARGUMENTS
```

## Tarefa

Execute a operação solicitada do Workflow Email To Task: $ARGUMENTS

1. **Verificar Pré-requisitos**
   - Verificar se `gws` está instalado: `gws --version`
   - Verificar autenticação: `gws auth status`
   - Revisar comandos disponíveis: `gws workflow-email-to-task --help`

2. **Inspecionar Schema do Método**
   - Antes de chamar qualquer método, inspecione seus parâmetros
   - Use `gws schema` para entender campos obrigatórios
   - Revise tipos de parâmetros e restrições

3. **Executar Operação**
   - Construir comando com flags apropriadas
   - Use `--params` para parâmetros de query/path
   - Use `--json` para body da requisição
   - Trate paginação com `--max-results` ou `--page-token`

4. **Tratamento de Erros**
   - Verificar output do comando para erros
   - Revisar quotas de API e rate limits
   - Tratar problemas de autenticação
   - Tentar novamente em caso de falhas transitórias

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-workflow-email-to-task`