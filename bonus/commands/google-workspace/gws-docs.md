---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [resource] [method] [flags]
description: Ler e escrever no Google Docs.
---

# Google Workspace Docs

Execute operações do Google Workspace Docs: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalado
- Autenticação configurada: Execute `gws auth status` para verificar
- Revise `gws docs --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# docs (v1)

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para autenticação, flags globais e regras de segurança. Se estiver ausente, execute `gws generate-skills` para criá-lo.

```bash
gws docs <resource> <method> [flags]
```

## Comandos Auxiliares

| Comando | Descrição |
|---------|-----------|
| [`+write`](../gws-docs-write/SKILL.md) | Anexar texto a um documento |

## Recursos de API

### documents

  - `batchUpdate` — Aplica uma ou mais atualizações ao documento. Cada solicitação é validada antes de ser aplicada. Se alguma solicitação não for válida, toda a solicitação falhará e nada será aplicado. Algumas solicitações têm respostas para fornecer informações sobre como são aplicadas. Outras solicitações não precisam retornar informações; cada uma retorna uma resposta vazia. A ordem das respostas corresponde à ordem das solicitações.
  - `create` — Cria um documento em branco usando o título fornecido na solicitação. Outros campos na solicitação, incluindo qualquer conteúdo fornecido, são ignorados. Retorna o documento criado.
  - `get` — Obtém a versão mais recente do documento especificado.

## Descobrindo Comandos

Antes de chamar qualquer método de API, inspecione-o:

```bash
# Procure recursos e métodos
gws docs --help

# Inspecione os parâmetros obrigatórios, tipos e padrões de um método
gws schema docs.<resource>.<method>
```

Use a saída de `gws schema` para construir seus flags `--params` e `--json`.

## Uso

```bash
# Listar recursos e métodos disponíveis
gws docs --help

# Inspecionar esquema do método antes de chamar
gws schema docs.<resource>.<method>

# Executar comando com argumentos
gws docs $ARGUMENTS
```

## Tarefa

Execute a operação de Docs solicitada: $ARGUMENTS

1. **Verificar Pré-requisitos**
   - Verificar se `gws` está instalado: `gws --version`
   - Verificar autenticação: `gws auth status`
   - Revisar comandos disponíveis: `gws docs --help`

2. **Inspecionar Esquema do Método**
   - Antes de chamar qualquer método, inspecione seus parâmetros
   - Use `gws schema` para entender os campos obrigatórios
   - Revise tipos de parâmetros e restrições

3. **Executar Operação**
   - Construir comando com flags apropriados
   - Usar `--params` para parâmetros de query/path
   - Usar `--json` para corpo da solicitação
   - Gerenciar paginação com `--max-results` ou `--page-token`

4. **Tratamento de Erros**
   - Verificar a saída do comando para erros
   - Revisar quotas de API e limites de taxa
   - Lidar com problemas de autenticação
   - Tentar novamente falhas transitórias

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-docs`