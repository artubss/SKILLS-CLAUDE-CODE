---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [recurso] [método] [sinalizadores]
description: Gmail: Enviar, ler e gerenciar e-mails.
---

# Google Workspace Gmail

Execute operações do Google Workspace Gmail: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalado
- Autenticação configurada: Execute `gws auth status` para verificar
- Revise `gws gmail --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# gmail (v1)

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para autenticação, sinalizadores globais e regras de segurança. Se estiver faltando, execute `gws generate-skills` para criá-lo.

```bash
gws gmail <recurso> <método> [sinalizadores]
```

## Comandos Auxiliares

| Comando | Descrição |
|---------|-----------|
| [`+send`](../gws-gmail-send/SKILL.md) | Enviar um e-mail |
| [`+triage`](../gws-gmail-triage/SKILL.md) | Mostrar resumo da caixa de entrada não lida (remetente, assunto, data) |
| [`+watch`](../gws-gmail-watch/SKILL.md) | Monitorar novos e-mails e transmiti-los como NDJSON |

## Recursos da API

### users

  - `getProfile` — Obtém o perfil Gmail do usuário atual.
  - `stop` — Parar de receber notificações push para a caixa de correio do usuário fornecido.
  - `watch` — Configurar ou atualizar uma observação de notificação push na caixa de correio do usuário fornecido.
  - `drafts` — Operações no recurso 'drafts'
  - `history` — Operações no recurso 'history'
  - `labels` — Operações no recurso 'labels'
  - `messages` — Operações no recurso 'messages'
  - `settings` — Operações no recurso 'settings'
  - `threads` — Operações no recurso 'threads'

## Descobrindo Comandos

Antes de chamar qualquer método de API, inspecione-o:

```bash
# Procurar recursos e métodos
gws gmail --help

# Inspecionar parâmetros, tipos e padrões necessários de um método
gws schema gmail.<recurso>.<método>
```

Use a saída `gws schema` para construir seus sinalizadores `--params` e `--json`.

## Uso

```bash
# Listar recursos e métodos disponíveis
gws gmail --help

# Inspecionar esquema de método antes de chamar
gws schema gmail.<recurso>.<método>

# Executar comando com argumentos
gws gmail $ARGUMENTS
```

## Tarefa

Execute a operação Gmail solicitada: $ARGUMENTS

1. **Verificar Pré-requisitos**
   - Verificar se `gws` está instalado: `gws --version`
   - Verificar autenticação: `gws auth status`
   - Revisar comandos disponíveis: `gws gmail --help`

2. **Inspecionar Esquema do Método**
   - Antes de chamar qualquer método, inspecione seus parâmetros
   - Use `gws schema` para entender os campos obrigatórios
   - Revise tipos de parâmetros e restrições

3. **Executar Operação**
   - Construir comando com sinalizadores apropriados
   - Usar `--params` para parâmetros de query/path
   - Usar `--json` para corpo da solicitação
   - Lidar com paginação com `--max-results` ou `--page-token`

4. **Tratamento de Erros**
   - Verificar saída do comando para erros
   - Revisar quotas de API e limites de taxa
   - Lidar com problemas de autenticação
   - Repetir falhas transitórias

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-gmail`