---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [recurso] [método] [sinalizadores]
description: Gmail: Monitore novos emails e transmita-os como NDJSON.
---

# Google Workspace Gmail Watch

Execute operações Google Workspace Gmail Watch: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalado
- Autenticação configurada: Execute `gws auth status` para verificar
- Revise `gws gmail-watch --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# gmail +watch

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para autenticação, sinalizadores globais e regras de segurança. Se não existir, execute `gws generate-skills` para criá-lo.

Monitore novos emails e transmita-os como NDJSON

## Uso

```bash
gws gmail +watch
```

## Sinalizadores

| Sinalizador | Obrigatório | Padrão | Descrição |
|------|----------|---------|-------------|
| `--project` | — | — | ID do projeto GCP para recursos Pub/Sub |
| `--subscription` | — | — | Nome de subscription Pub/Sub existente (ignore configuração) |
| `--topic` | — | — | Topic Pub/Sub existente com permissão push do Gmail já concedida |
| `--label-ids` | — | — | IDs de rótulo Gmail separados por vírgula para filtrar (ex: INBOX,UNREAD) |
| `--max-messages` | — | 10 | Máximo de mensagens por lote de pull |
| `--poll-interval` | — | 5 | Segundos entre pulls |
| `--msg-format` | — | full | Formato de mensagem Gmail: full, metadata, minimal, raw |
| `--once` | — | — | Faça pull uma vez e saia |
| `--cleanup` | — | — | Exclua recursos Pub/Sub criados ao sair |
| `--output-dir` | — | — | Escreva cada mensagem em um arquivo JSON separado neste diretório |

## Exemplos

```bash
gws gmail +watch --project my-gcp-project
gws gmail +watch --project my-project --label-ids INBOX --once
gws gmail +watch --subscription projects/p/subscriptions/my-sub
gws gmail +watch --project my-project --cleanup --output-dir ./emails
```

## Dicas

- Gmail watch expira após 7 dias — execute novamente para renovar.
- Sem --cleanup, recursos Pub/Sub persistem para reconexão.
- Pressione Ctrl-C para parar graciosamente.

## Veja Também

- [gws-shared](../gws-shared/SKILL.md) — Sinalizadores globais e autenticação
- [gws-gmail](../gws-gmail/SKILL.md) — Todos os comandos de envio, leitura e gerenciamento de email

## Uso

```bash
# Liste recursos e métodos disponíveis
gws gmail-watch --help

# Inspecione schema do método antes de chamar
gws schema gmail-watch.<resource>.<method>

# Execute comando com argumentos
gws gmail-watch $ARGUMENTS
```

## Tarefa

Execute a operação Gmail Watch solicitada: $ARGUMENTS

1. **Verifique Pré-requisitos**
   - Verifique se `gws` está instalado: `gws --version`
   - Verifique autenticação: `gws auth status`
   - Revise comandos disponíveis: `gws gmail-watch --help`

2. **Inspecione Schema do Método**
   - Antes de chamar qualquer método, inspecione seus parâmetros
   - Use `gws schema` para compreender campos obrigatórios
   - Revise tipos e restrições de parâmetros

3. **Execute Operação**
   - Construa comando com sinalizadores apropriados
   - Use `--params` para parâmetros de query/path
   - Use `--json` para corpo de requisição
   - Gerencie paginação com `--max-results` ou `--page-token`

4. **Tratamento de Erros**
   - Verifique saída de comando quanto a erros
   - Revise quotas de API e limites de taxa
   - Gerencie problemas de autenticação
   - Tente novamente falhas transitórias

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-gmail-watch`