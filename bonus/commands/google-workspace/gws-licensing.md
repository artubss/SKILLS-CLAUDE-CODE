---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [resource] [method] [flags]
description: Gerenciador de Licenças Google Workspace Enterprise: Gerencie licenças de produtos.
---

# Licenciamento Google Workspace

Execute operações de Licenciamento Google Workspace: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalado
- Autenticação configurada: Execute `gws auth status` para verificar
- Revise `gws licensing --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# licensing (v1)

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para autenticação, flags globais e regras de segurança. Se estiver ausente, execute `gws generate-skills` para criar.

```bash
gws licensing <resource> <method> [flags]
```

## Recursos de API

### licenseAssignments

  - `delete` — Revogar uma licença.
  - `get` — Obter a licença de um usuário específico por SKU do produto.
  - `insert` — Atribuir uma licença.
  - `listForProduct` — Listar todos os usuários com licenças atribuídas para um SKU de produto específico.
  - `listForProductAndSku` — Listar todos os usuários com licenças atribuídas para um SKU de produto específico.
  - `patch` — Reatribuir o SKU do produto de um usuário com um SKU diferente no mesmo produto. Este método suporta semântica de patch.
  - `update` — Reatribuir o SKU do produto de um usuário com um SKU diferente no mesmo produto.

## Descobrindo Comandos

Antes de chamar qualquer método de API, inspecione-o:

```bash
# Navegar por recursos e métodos
gws licensing --help

# Inspecionar os parâmetros, tipos e padrões exigidos de um método
gws schema licensing.<resource>.<method>
```

Use a saída de `gws schema` para construir seus flags `--params` e `--json`.

## Uso

```bash
# Listar recursos e métodos disponíveis
gws licensing --help

# Inspecionar o schema do método antes de chamar
gws schema licensing.<resource>.<method>

# Executar comando com argumentos
gws licensing $ARGUMENTS
```

## Tarefa

Execute a operação de Licenciamento solicitada: $ARGUMENTS

1. **Verificar Pré-requisitos**
   - Verificar se `gws` está instalado: `gws --version`
   - Verificar autenticação: `gws auth status`
   - Revisar comandos disponíveis: `gws licensing --help`

2. **Inspecionar o Schema do Método**
   - Antes de chamar qualquer método, inspecione seus parâmetros
   - Use `gws schema` para entender os campos obrigatórios
   - Revise tipos de parâmetros e restrições

3. **Executar Operação**
   - Construa o comando com flags apropriados
   - Use `--params` para parâmetros de query/path
   - Use `--json` para corpo da requisição
   - Gerencie paginação com `--max-results` ou `--page-token`

4. **Tratamento de Erros**
   - Verificar a saída do comando para erros
   - Revisar quotas de API e limites de taxa
   - Tratar problemas de autenticação
   - Repetir falhas transitórias

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-licensing`