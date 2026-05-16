---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [resource] [method] [flags]
description: Google Vault: Gerenciar holds de eDiscovery e exports.
---

# Google Workspace Vault

Execute operações do Google Workspace Vault: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalada
- Autenticação configurada: Execute `gws auth status` para verificar
- Revise `gws vault --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# vault (v1)

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para autenticação, flags globais e regras de segurança. Se estiver ausente, execute `gws generate-skills` para criá-lo.

```bash
gws vault <resource> <method> [flags]
```

## Recursos da API

### matters

  - `addPermissions` — Adiciona uma conta como colaboradora de um caso.
  - `close` — Fecha o caso especificado. Retorna o caso com estado atualizado.
  - `count` — Conta as contas processadas pela consulta especificada.
  - `create` — Cria um caso com o nome e descrição fornecidos. O estado inicial é aberto e o proprietário é quem fez a chamada do método. Retorna o caso criado com view padrão.
  - `delete` — Deleta o caso especificado. Retorna o caso com estado atualizado.
  - `get` — Obtém o caso especificado.
  - `list` — Lista os casos aos quais o solicitante tem acesso.
  - `removePermissions` — Remove uma conta como colaboradora de um caso.
  - `reopen` — Reabre o caso especificado. Retorna o caso com estado atualizado.
  - `undelete` — Restaura o caso especificado. Retorna o caso com estado atualizado.
  - `update` — Atualiza o caso especificado. Atualiza apenas o nome e descrição do caso, identificado pelo ID do caso. Alterações em outros campos são ignoradas. Retorna a view padrão do caso.
  - `exports` — Operações no recurso 'exports'
  - `holds` — Operações no recurso 'holds'
  - `savedQueries` — Operações no recurso 'savedQueries'

### operations

  - `cancel` — Inicia o cancelamento assíncrono de uma operação de longa duração. O servidor faz melhor esforço para cancelar a operação, mas o sucesso não é garantido. Se o servidor não suportar este método, retorna `google.rpc.Code.UNIMPLEMENTED`. Clientes podem usar Operations.GetOperation ou outros métodos para verificar se o cancelamento foi bem-sucedido ou se a operação foi concluída apesar do cancelamento.
  - `delete` — Deleta uma operação de longa duração. Este método indica que o cliente não está mais interessado no resultado da operação. Não cancela a operação. Se o servidor não suportar este método, retorna `UNIMPLEMENTED`.
  - `get` — Obtém o estado mais recente de uma operação de longa duração. Clientes podem usar este método para consultar o resultado da operação em intervalos conforme recomendado pelo serviço de API.
  - `list` — Lista operações que correspondem ao filtro especificado na solicitação. Se o servidor não suportar este método, retorna `UNIMPLEMENTED`.

## Descobrindo Comandos

Antes de chamar qualquer método de API, inspecione-o:

```bash
# Navegar por recursos e métodos
gws vault --help

# Inspecionar parâmetros obrigatórios, tipos e padrões de um método
gws schema vault.<resource>.<method>
```

Use a saída de `gws schema` para construir seus flags `--params` e `--json`.

## Uso

```bash
# Listar recursos e métodos disponíveis
gws vault --help

# Inspecionar schema do método antes de chamar
gws schema vault.<resource>.<method>

# Executar comando com argumentos
gws vault $ARGUMENTS
```

## Tarefa

Execute a operação do Vault solicitada: $ARGUMENTS

1. **Verificar Pré-requisitos**
   - Verificar se `gws` está instalada: `gws --version`
   - Verificar autenticação: `gws auth status`
   - Revisar comandos disponíveis: `gws vault --help`

2. **Inspecionar Schema do Método**
   - Antes de chamar qualquer método, inspecione seus parâmetros
   - Use `gws schema` para entender os campos obrigatórios
   - Revise tipos de parâmetro e restrições

3. **Executar Operação**
   - Construir comando com flags apropriadas
   - Use `--params` para parâmetros de query/path
   - Use `--json` para corpo da solicitação
   - Tratar paginação com `--max-results` ou `--page-token`

4. **Tratamento de Erros**
   - Verificar erros na saída do comando
   - Revisar quotas de API e limites de taxa
   - Tratar problemas de autenticação
   - Tentar novamente falhas transitórias

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-vault`