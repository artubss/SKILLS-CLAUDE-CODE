---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [resource] [method] [flags]
description: Google Workspace Events: Inscreva-se em eventos do Workspace e transmita-os como NDJSON.
---

# Google Workspace Events Subscribe

Execute operações de Google Workspace Events Subscribe: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalada
- Autenticação configurada: Execute `gws auth status` para verificar
- Revise `gws events-subscribe --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# events +subscribe

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para autenticação, flags globais e regras de segurança. Se estiver ausente, execute `gws generate-skills` para criá-lo.

Inscreva-se em eventos do Workspace e transmita-os como NDJSON

## Uso

```bash
gws events +subscribe
```

## Flags

| Flag | Obrigatória | Padrão | Descrição |
|------|----------|--------|-------------|
| `--target` | — | — | URI do recurso Workspace (ex: //chat.googleapis.com/spaces/SPACE_ID) |
| `--event-types` | — | — | Tipos CloudEvents separados por vírgula para inscrição |
| `--project` | — | — | ID do projeto GCP para recursos Pub/Sub |
| `--subscription` | — | — | Nome da assinatura Pub/Sub existente (pular configuração) |
| `--max-messages` | — | 10 | Máximo de mensagens por batch (padrão: 10) |
| `--poll-interval` | — | 5 | Segundos entre pulls (padrão: 5) |
| `--once` | — | — | Pull uma vez e sair |
| `--cleanup` | — | — | Deletar recursos Pub/Sub criados ao sair |
| `--no-ack` | — | — | Não reconhecer automaticamente mensagens |
| `--output-dir` | — | — | Escrever cada evento em arquivo JSON separado neste diretório |

## Exemplos

```bash
gws events +subscribe --target '//chat.googleapis.com/spaces/SPACE' --event-types 'google.workspace.chat.message.v1.created' --project my-project
gws events +subscribe --subscription projects/p/subscriptions/my-sub --once
gws events +subscribe ... --cleanup --output-dir ./events
```

## Dicas

- Sem --cleanup, recursos Pub/Sub persistem para reconexão.
- Pressione Ctrl-C para parar graciosamente.

> [!CAUTION]
> Este é um comando de **escrita** — confirme com o usuário antes de executar.

## Veja Também

- [gws-shared](../gws-shared/SKILL.md) — Flags globais e autenticação
- [gws-events](../gws-events/SKILL.md) — Todos os comandos de inscrição em eventos do Workspace

## Uso

```bash
# Listar recursos e métodos disponíveis
gws events-subscribe --help

# Inspecionar schema do método antes de chamar
gws schema events-subscribe.<resource>.<method>

# Executar comando com argumentos
gws events-subscribe $ARGUMENTS
```

## Tarefa

Execute a operação Events Subscribe solicitada: $ARGUMENTS

1. **Verificar Pré-requisitos**
   - Verificar se `gws` está instalada: `gws --version`
   - Verificar autenticação: `gws auth status`
   - Revisar comandos disponíveis: `gws events-subscribe --help`

2. **Inspecionar Schema do Método**
   - Antes de chamar qualquer método, inspecione seus parâmetros
   - Use `gws schema` para entender campos obrigatórios
   - Revise tipos de parâmetro e restrições

3. **Executar Operação**
   - Construir comando com flags apropriadas
   - Use `--params` para parâmetros de query/path
   - Use `--json` para corpo da requisição
   - Gerencie paginação com `--max-results` ou `--page-token`

4. **Tratamento de Erros**
   - Verificar output do comando para erros
   - Revisar quotas de API e limites de taxa
   - Tratar problemas de autenticação
   - Tentar novamente falhas transitórias

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-events-subscribe`