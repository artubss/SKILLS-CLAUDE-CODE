---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [resource] [method] [flags]
description: Google Sheets: Ler valores de uma planilha.
---

# Leitura do Google Workspace Sheets

Execute operações de Leitura do Google Workspace Sheets: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalado
- Autenticação configurada: Execute `gws auth status` para verificar
- Revise `gws sheets-read --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# sheets +read

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para autenticação, flags globais e regras de segurança. Se não existir, execute `gws generate-skills` para criá-lo.

Ler valores de uma planilha

## Uso

```bash
gws sheets +read --spreadsheet <ID> --range <RANGE>
```

## Flags

| Flag | Obrigatória | Padrão | Descrição |
|------|----------|--------|-------------|
| `--spreadsheet` | ✓ | — | ID da planilha |
| `--range` | ✓ | — | Intervalo a ler (ex: 'Sheet1!A1:B2') |

## Exemplos

```bash
gws sheets +read --spreadsheet ID --range 'Sheet1!A1:D10'
gws sheets +read --spreadsheet ID --range Sheet1
```

## Dicas

- Somente leitura — nunca modifica a planilha.
- Para opções avançadas, use a API raw values.get.

## Veja também

- [gws-shared](../gws-shared/SKILL.md) — Flags globais e autenticação
- [gws-sheets](../gws-sheets/SKILL.md) — Todos os comandos de leitura e escrita de planilhas

## Uso

```bash
# Listar recursos e métodos disponíveis
gws sheets-read --help

# Inspecionar schema do método antes de chamar
gws schema sheets-read.<resource>.<method>

# Executar comando com argumentos
gws sheets-read $ARGUMENTS
```

## Tarefa

Execute a operação de Leitura do Sheets solicitada: $ARGUMENTS

1. **Verificar Pré-requisitos**
   - Verificar se `gws` está instalado: `gws --version`
   - Verificar autenticação: `gws auth status`
   - Revisar comandos disponíveis: `gws sheets-read --help`

2. **Inspecionar Schema do Método**
   - Antes de chamar qualquer método, inspecione seus parâmetros
   - Use `gws schema` para entender os campos obrigatórios
   - Revise tipos de parâmetros e restrições

3. **Executar Operação**
   - Construir comando com as flags apropriadas
   - Usar `--params` para parâmetros de query/path
   - Usar `--json` para corpo da requisição
   - Gerenciar paginação com `--max-results` ou `--page-token`

4. **Tratamento de Erros**
   - Verificar saída do comando para erros
   - Revisar quotas de API e limites de taxa
   - Gerenciar problemas de autenticação
   - Repetir falhas transitórias

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-sheets-read`