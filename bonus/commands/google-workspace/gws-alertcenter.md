---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [resource] [method] [flags]
description: Google Workspace Alert Center: Gerenciar alertas de segurança do Workspace.
---

# Google Workspace Alertcenter

Execute operações do Google Workspace Alertcenter: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalado
- Autenticação configurada: Execute `gws auth status` para verificar
- Revise `gws alertcenter --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# alertcenter (v1beta1)

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para autenticação, flags globais e regras de segurança. Se estiver faltando, execute `gws generate-skills` para criá-lo.

```bash
gws alertcenter <resource> <method> [flags]
```

## Recursos da API

### alerts

  - `batchDelete` — Executa operação de exclusão em lote em alertas.
  - `batchUndelete` — Executa operação de restauração em lote em alertas.
  - `delete` — Marca o alerta especificado para exclusão. Um alerta marcado para exclusão é removido do Alert Center após 30 dias. Marcar um alerta para exclusão não afeta um alerta que já foi marcado para exclusão. Tentar marcar um alerta inexistente para exclusão resulta em erro `NOT_FOUND`.
  - `get` — Obtém o alerta especificado. Tentar obter um alerta inexistente retorna erro `NOT_FOUND`.
  - `getMetadata` — Retorna os metadados de um alerta. Tentar obter metadados de um alerta inexistente retorna erro `NOT_FOUND`.
  - `list` — Lista os alertas.
  - `undelete` — Restaura ou "cancela a exclusão" de um alerta que foi marcado para exclusão nos últimos 30 dias. Tentar cancelar a exclusão de um alerta que foi marcado para exclusão há mais de 30 dias (que foi removido do banco de dados do Alert Center) ou de um alerta inexistente retorna erro `NOT_FOUND`. Tentar cancelar a exclusão de um alerta que não foi marcado para exclusão não tem efeito.
  - `feedback` — Operações no recurso 'feedback'

### v1beta1

  - `getSettings` — Retorna configurações no nível do cliente.
  - `updateSettings` — Atualiza as configurações no nível do cliente.

## Descobrindo Comandos

Antes de chamar qualquer método da API, inspecione-o:

```bash
# Navegue por recursos e métodos
gws alertcenter --help

# Inspecione os parâmetros obrigatórios, tipos e padrões de um método
gws schema alertcenter.<resource>.<method>
```

Use a saída de `gws schema` para construir suas flags `--params` e `--json`.

## Uso

```bash
# Liste recursos e métodos disponíveis
gws alertcenter --help

# Inspecione o schema do método antes de chamá-lo
gws schema alertcenter.<resource>.<method>

# Execute o comando com argumentos
gws alertcenter $ARGUMENTS
```

## Tarefa

Execute a operação Alertcenter solicitada: $ARGUMENTS

1. **Verifique os Pré-requisitos**
   - Verifique se `gws` está instalado: `gws --version`
   - Verifique autenticação: `gws auth status`
   - Revise comandos disponíveis: `gws alertcenter --help`

2. **Inspecione o Schema do Método**
   - Antes de chamar qualquer método, inspecione seus parâmetros
   - Use `gws schema` para entender campos obrigatórios
   - Revise tipos de parâmetros e restrições

3. **Execute a Operação**
   - Construa o comando com flags apropriadas
   - Use `--params` para parâmetros de query/path
   - Use `--json` para o corpo da requisição
   - Trate paginação com `--max-results` ou `--page-token`

4. **Tratamento de Erros**
   - Verifique a saída do comando para erros
   - Revise quotas de API e limites de taxa
   - Trate problemas de autenticação
   - Tente novamente falhas transitórias

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-alertcenter`