---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [recurso] [método] [sinalizadores]
description: Google Calendar: Criar um novo evento.
---

# Google Workspace Calendar Insert

Execute operações de Google Workspace Calendar Insert: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalado
- Autenticação configurada: Execute `gws auth status` para verificar
- Revise `gws calendar-insert --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# calendar +insert

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para autenticação, sinalizadores globais e regras de segurança. Se não existir, execute `gws generate-skills` para criá-lo.

criar um novo evento

## Uso

```bash
gws calendar +insert --summary <TEXT> --start <TIME> --end <TIME>
```

## Sinalizadores

| Sinalizador | Obrigatório | Padrão | Descrição |
|-------------|-------------|--------|-----------|
| `--calendar` | — | primary | ID do calendário (padrão: primary) |
| `--summary` | ✓ | — | Resumo/título do evento |
| `--start` | ✓ | — | Hora de início (ISO 8601, ex: 2024-01-01T10:00:00Z) |
| `--end` | ✓ | — | Hora de término (ISO 8601) |
| `--location` | — | — | Local do evento |
| `--description` | — | — | Descrição/corpo do evento |
| `--attendee` | — | — | Email do participante (pode ser usado múltiplas vezes) |

## Exemplos

```bash
gws calendar +insert --summary 'Standup' --start '2026-06-17T09:00:00-07:00' --end '2026-06-17T09:30:00-07:00'
gws calendar +insert --summary 'Review' --start ... --end ... --attendee alice@example.com
```

## Dicas

- Use formato RFC3339 para horários (ex: 2026-06-17T09:00:00-07:00).
- Para eventos recorrentes ou links de conferência, use a API bruta.

> [!CAUTION]
> Este é um comando de **escrita** — confirme com o usuário antes de executar.

## Veja Também

- [gws-shared](../gws-shared/SKILL.md) — Sinalizadores globais e autenticação
- [gws-calendar](../gws-calendar/SKILL.md) — Todos os comandos para gerenciar calendários e eventos

## Uso

```bash
# Listar recursos e métodos disponíveis
gws calendar-insert --help

# Inspecionar schema do método antes de chamar
gws schema calendar-insert.<recurso>.<método>

# Executar comando com argumentos
gws calendar-insert $ARGUMENTS
```

## Tarefa

Execute a operação de Calendar Insert solicitada: $ARGUMENTS

1. **Verificar Pré-requisitos**
   - Verifique se `gws` está instalado: `gws --version`
   - Confirme autenticação: `gws auth status`
   - Revise comandos disponíveis: `gws calendar-insert --help`

2. **Inspecionar Schema do Método**
   - Antes de chamar qualquer método, inspecione seus parâmetros
   - Use `gws schema` para entender campos obrigatórios
   - Revise tipos de parâmetros e restrições

3. **Executar Operação**
   - Construa comando com sinalizadores apropriados
   - Use `--params` para parâmetros de query/path
   - Use `--json` para corpo da requisição
   - Trate paginação com `--max-results` ou `--page-token`

4. **Tratamento de Erros**
   - Verifique erros no output do comando
   - Revise quotas de API e limites de taxa
   - Trate problemas de autenticação
   - Tente novamente falhas transitórias

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-calendar-insert`