---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [resource] [method] [flags]
description: Google Workspace Events: Renovar/reativar assinaturas de Workspace Events.
---

# Google Workspace Events Renew

Execute operações de renovação de Google Workspace Events: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalado
- Autenticação configurada: Execute `gws auth status` para verificar
- Revise `gws events-renew --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# events +renew

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para autenticação, flags globais e regras de segurança. Se não existir, execute `gws generate-skills` para criá-lo.

Renovar/reativar assinaturas de Workspace Events

## Uso

```bash
gws events +renew
```

## Flags

| Flag | Obrigatória | Padrão | Descrição |
|------|----------|---------|-------------|
| `--name` | — | — | Nome da assinatura a reativar (ex: subscriptions/SUB_ID) |
| `--all` | — | — | Renovar todas as assinaturas vencendo dentro da janela --within |
| `--within` | — | 1h | Janela de tempo para --all (ex: 1h, 30m, 2d) |

## Exemplos

```bash
gws events +renew --name subscriptions/SUB_ID
gws events +renew --all --within 2d
```

## Dicas

- Assinaturas expiram se não forem renovadas periodicamente.
- Use --all com uma tarefa cron para manter assinaturas ativas.

## Veja também

- [gws-shared](../gws-shared/SKILL.md) — Flags globais e autenticação
- [gws-events](../gws-events/SKILL.md) — Todos os comandos de assinatura a eventos do Google Workspace

## Uso

```bash
# Listar recursos e métodos disponíveis
gws events-renew --help

# Inspecionar schema do método antes de chamar
gws schema events-renew.<resource>.<method>

# Executar comando com argumentos
gws events-renew $ARGUMENTS
```

## Tarefa

Execute a operação de renovação de Events solicitada: $ARGUMENTS

1. **Verificar Pré-requisitos**
   - Verificar se `gws` está instalado: `gws --version`
   - Verificar autenticação: `gws auth status`
   - Revisar comandos disponíveis: `gws events-renew --help`

2. **Inspecionar Schema do Método**
   - Antes de chamar qualquer método, inspecione seus parâmetros
   - Use `gws schema` para entender os campos obrigatórios
   - Revise tipos de parâmetro e restrições

3. **Executar Operação**
   - Construir comando com flags apropriadas
   - Use `--params` para parâmetros de query/path
   - Use `--json` para corpo da requisição
   - Trate paginação com `--max-results` ou `--page-token`

4. **Tratamento de Erros**
   - Verifique a saída do comando para erros
   - Revise quotas de API e limites de taxa
   - Trate problemas de autenticação
   - Repita falhas transientes

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-events-renew`