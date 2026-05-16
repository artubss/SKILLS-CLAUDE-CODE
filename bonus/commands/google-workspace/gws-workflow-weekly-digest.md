---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [resource] [method] [flags]
description: Google Workflow: Resumo semanal: reuniões desta semana + contagem de emails não lidos.
---

# Google Workspace Workflow Weekly Digest

Execute operações Google Workspace Workflow Weekly Digest: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalado
- Autenticação configurada: Execute `gws auth status` para verificar
- Revise `gws workflow-weekly-digest --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# workflow +weekly-digest

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para autenticação, flags globais e regras de segurança. Se estiver faltando, execute `gws generate-skills` para criá-lo.

Resumo semanal: reuniões desta semana + contagem de emails não lidos

## Uso

```bash
gws workflow +weekly-digest
```

## Flags

| Flag | Obrigatório | Padrão | Descrição |
|------|----------|---------|-------------|
| `--format` | — | — | Formato de saída: json (padrão), table, yaml, csv |

## Exemplos

```bash
gws workflow +weekly-digest
gws workflow +weekly-digest --format table
```

## Dicas

- Somente leitura — nunca modifica dados.
- Combina agenda do calendário (semana) com resumo de triagem do gmail.

## Veja Também

- [gws-shared](../gws-shared/SKILL.md) — Flags globais e autenticação
- [gws-workflow](../gws-workflow/SKILL.md) — Todos os comandos de workflows de produtividade entre serviços

## Uso

```bash
# Listar recursos disponíveis e métodos
gws workflow-weekly-digest --help

# Inspecionar schema do método antes de chamar
gws schema workflow-weekly-digest.<resource>.<method>

# Executar comando com argumentos
gws workflow-weekly-digest $ARGUMENTS
```

## Tarefa

Execute a operação Workflow Weekly Digest solicitada: $ARGUMENTS

1. **Verificar Pré-requisitos**
   - Verificar se `gws` está instalado: `gws --version`
   - Verificar autenticação: `gws auth status`
   - Revisar comandos disponíveis: `gws workflow-weekly-digest --help`

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
   - Verificar saída do comando para erros
   - Revisar quotas de API e limites de taxa
   - Tratar problemas de autenticação
   - Tentar novamente falhas transitórias

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-workflow-weekly-digest`