---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [resource] [method] [flags]
description: Google Calendar: Mostrar próximos eventos em todos os calendários.
---

# Agenda do Google Workspace Calendar

Execute operações de Agenda do Google Workspace Calendar: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalado
- Autenticação configurada: Execute `gws auth status` para verificar
- Revise `gws calendar-agenda --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# calendar +agenda

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para autenticação, flags globais e regras de segurança. Se não encontrar, execute `gws generate-skills` para criar.

Mostrar próximos eventos em todos os calendários

## Uso

```bash
gws calendar +agenda
```

## Flags

| Flag | Obrigatório | Padrão | Descrição |
|------|-------------|--------|-----------|
| `--today` | — | — | Mostrar eventos de hoje |
| `--tomorrow` | — | — | Mostrar eventos de amanhã |
| `--week` | — | — | Mostrar eventos desta semana |
| `--days` | — | — | Número de dias adiante a mostrar |
| `--calendar` | — | — | Filtrar por nome ou ID do calendário específico |

## Exemplos

```bash
gws calendar +agenda
gws calendar +agenda --today
gws calendar +agenda --week --format table
gws calendar +agenda --days 3 --calendar 'Work'
```

## Dicas

- Somente leitura — nunca modifica eventos.
- Consulta todos os calendários por padrão; use --calendar para filtrar.

## Veja Também

- [gws-shared](../gws-shared/SKILL.md) — Flags globais e autenticação
- [gws-calendar](../gws-calendar/SKILL.md) — Todos os comandos de gerenciamento de calendários e eventos

## Uso

```bash
# Listar recursos e métodos disponíveis
gws calendar-agenda --help

# Inspecionar esquema do método antes de chamar
gws schema calendar-agenda.<resource>.<method>

# Executar comando com argumentos
gws calendar-agenda $ARGUMENTS
```

## Tarefa

Execute a operação de Agenda do Calendar solicitada: $ARGUMENTS

1. **Verificar Pré-requisitos**
   - Verificar se `gws` está instalado: `gws --version`
   - Verificar autenticação: `gws auth status`
   - Revisar comandos disponíveis: `gws calendar-agenda --help`

2. **Inspecionar Esquema do Método**
   - Antes de chamar qualquer método, inspecione seus parâmetros
   - Use `gws schema` para entender os campos obrigatórios
   - Revise tipos de parâmetros e restrições

3. **Executar Operação**
   - Construir comando com flags apropriadas
   - Use `--params` para parâmetros de query/path
   - Use `--json` para corpo da solicitação
   - Lidar com paginação com `--max-results` ou `--page-token`

4. **Tratamento de Erros**
   - Verificar erros na saída do comando
   - Revisar quotas de API e limites de taxa
   - Lidar com problemas de autenticação
   - Repetir falhas transitórias

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-calendar-agenda`