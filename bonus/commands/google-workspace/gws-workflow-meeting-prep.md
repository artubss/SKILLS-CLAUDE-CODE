---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [resource] [method] [flags]
description: Google Workflow: Prepare para sua próxima reunião: agenda, participantes e documentos vinculados.
---

# Google Workspace Workflow Meeting Prep

Execute operações do Google Workspace Workflow Meeting Prep: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalado
- Autenticação configurada: Execute `gws auth status` para verificar
- Revise `gws workflow-meeting-prep --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# workflow +meeting-prep

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para autenticação, flags globais e regras de segurança. Se estiver faltando, execute `gws generate-skills` para criá-lo.

Prepare para sua próxima reunião: agenda, participantes e documentos vinculados

## Uso

```bash
gws workflow +meeting-prep
```

## Flags

| Flag | Obrigatório | Padrão | Descrição |
|------|----------|---------|-------------|
| `--calendar` | — | primary | ID do calendário (padrão: primary) |
| `--format` | — | — | Formato de saída: json (padrão), table, yaml, csv |

## Exemplos

```bash
gws workflow +meeting-prep
gws workflow +meeting-prep --calendar Work
```

## Dicas

- Somente leitura — nunca modifica dados.
- Mostra o próximo evento com participantes e descrição.

## Veja também

- [gws-shared](../gws-shared/SKILL.md) — Flags globais e autenticação
- [gws-workflow](../gws-workflow/SKILL.md) — Todos os comandos de workflows de produtividade entre serviços

## Uso

```bash
# Listar recursos e métodos disponíveis
gws workflow-meeting-prep --help

# Inspecionar schema do método antes de chamar
gws schema workflow-meeting-prep.<resource>.<method>

# Executar comando com argumentos
gws workflow-meeting-prep $ARGUMENTS
```

## Tarefa

Execute a operação solicitada do Workflow Meeting Prep: $ARGUMENTS

1. **Verificar Pré-requisitos**
   - Verificar se `gws` está instalado: `gws --version`
   - Verificar autenticação: `gws auth status`
   - Revisar comandos disponíveis: `gws workflow-meeting-prep --help`

2. **Inspecionar Schema do Método**
   - Antes de chamar qualquer método, inspecione seus parâmetros
   - Use `gws schema` para entender os campos obrigatórios
   - Revise tipos de parâmetros e restrições

3. **Executar Operação**
   - Construir comando com flags apropriadas
   - Use `--params` para parâmetros de query/path
   - Use `--json` para corpo da requisição
   - Lidar com paginação com `--max-results` ou `--page-token`

4. **Tratamento de Erros**
   - Verificar a saída do comando para erros
   - Revisar quotas de API e limites de taxa
   - Lidar com problemas de autenticação
   - Tentar novamente falhas transitórias

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-workflow-meeting-prep`