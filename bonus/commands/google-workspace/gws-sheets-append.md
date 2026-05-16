---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [recurso] [método] [flags]
description: Google Sheets: Adicionar uma linha a uma planilha.
---

# Google Workspace Sheets Append

Execute operações Google Workspace Sheets Append: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalado
- Autenticação configurada: Execute `gws auth status` para verificar
- Consulte `gws sheets-append --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# sheets +append

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para autenticação, flags globais e regras de segurança. Se estiver ausente, execute `gws generate-skills` para criá-lo.

Adicionar uma linha a uma planilha

## Uso

```bash
gws sheets +append --spreadsheet <ID>
```

## Flags

| Flag | Obrigatório | Padrão | Descrição |
|------|----------|---------|-------------|
| `--spreadsheet` | ✓ | — | ID da planilha |
| `--values` | — | — | Valores separados por vírgula (strings simples) |
| `--json-values` | — | — | Array JSON de linhas, ex: '[["a","b"],["c","d"]]' |

## Exemplos

```bash
gws sheets +append --spreadsheet ID --values 'Alice,100,true'
gws sheets +append --spreadsheet ID --json-values '[["a","b"],["c","d"]]'
```

## Dicas

- Use --values para anexos de linhas únicas simples.
- Use --json-values para inserções em massa de múltiplas linhas.

> [!CAUTION]
> Este é um comando de **escrita** — confirme com o usuário antes de executar.

## Veja Também

- [gws-shared](../gws-shared/SKILL.md) — Flags globais e autenticação
- [gws-sheets](../gws-sheets/SKILL.md) — Todos os comandos de leitura e escrita de planilhas

## Uso

```bash
# Listar recursos e métodos disponíveis
gws sheets-append --help

# Inspecionar schema do método antes de chamar
gws schema sheets-append.<recurso>.<método>

# Executar comando com argumentos
gws sheets-append $ARGUMENTS
```

## Tarefa

Execute a operação Sheets Append solicitada: $ARGUMENTS

1. **Verificar Pré-requisitos**
   - Verificar se `gws` está instalado: `gws --version`
   - Verificar autenticação: `gws auth status`
   - Revisar comandos disponíveis: `gws sheets-append --help`

2. **Inspecionar Schema do Método**
   - Antes de chamar qualquer método, inspecione seus parâmetros
   - Use `gws schema` para entender os campos obrigatórios
   - Revise tipos de parâmetros e restrições

3. **Executar Operação**
   - Construir comando com flags apropriadas
   - Use `--params` para parâmetros de query/path
   - Use `--json` para corpo da requisição
   - Manipule paginação com `--max-results` ou `--page-token`

4. **Tratamento de Erros**
   - Verifique saída do comando para erros
   - Revise quotas de API e limites de taxa
   - Trate problemas de autenticação
   - Tente novamente falhas transitórias

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-sheets-append`