---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [resource] [method] [flags]
description: Google Workspace Admin SDK: Logs de auditoria e relatórios de uso.
---

# Google Workspace Admin Reports

Execute operações do Google Workspace Admin Reports: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalado
- Autenticação configurada: Execute `gws auth status` para verificar
- Revise `gws admin-reports --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# admin-reports (reports_v1)

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para autenticação, flags globais e regras de segurança. Se estiver ausente, execute `gws generate-skills` para criá-lo.

```bash
gws admin-reports <resource> <method> [flags]
```

## Recursos da API

### activities

  - `list` — Recupera uma lista de atividades da conta de um cliente específico e aplicação, como a aplicação do Console Admin ou a aplicação Google Drive. Para mais informações, consulte os guias para relatórios de atividade do administrador e do Google Drive. Para mais informações sobre os parâmetros do relatório de atividade, consulte os guias de referência dos parâmetros de atividade.
  - `watch` — Comece a receber notificações de atividades da conta. Para mais informações, consulte Recebendo Notificações por Push.

### channels

  - `stop` — Pare de observar recursos através deste canal.

### customerUsageReports

  - `get` — Recupera um relatório que é uma coleção de propriedades e estatísticas da conta de um cliente específico. Para mais informações, consulte o guia Relatório de Uso de Clientes. Para mais informações sobre os parâmetros do relatório do cliente, consulte os guias de referência dos parâmetros de Uso de Clientes.

### entityUsageReports

  - `get` — Recupera um relatório que é uma coleção de propriedades e estatísticas de entidades usadas por usuários na conta. Para mais informações, consulte o guia Relatório de Uso de Entidades. Para mais informações sobre os parâmetros do relatório de entidades, consulte os guias de referência dos parâmetros de Uso de Entidades.

### userUsageReport

  - `get` — Recupera um relatório que é uma coleção de propriedades e estatísticas de um conjunto de usuários na conta. Para mais informações, consulte o guia Relatório de Uso de Usuários. Para mais informações sobre os parâmetros do relatório do usuário, consulte os guias de referência dos parâmetros de Uso de Usuários.

## Descobrindo Comandos

Antes de chamar qualquer método da API, inspecione-o:

```bash
# Navegue pelos recursos e métodos
gws admin-reports --help

# Inspecione os parâmetros obrigatórios, tipos e padrões de um método
gws schema admin-reports.<resource>.<method>
```

Use a saída de `gws schema` para criar seus flags `--params` e `--json`.

## Uso

```bash
# Liste recursos e métodos disponíveis
gws admin-reports --help

# Inspecione o schema do método antes de chamar
gws schema admin-reports.<resource>.<method>

# Execute o comando com argumentos
gws admin-reports $ARGUMENTS
```

## Tarefa

Execute a operação solicitada de Admin Reports: $ARGUMENTS

1. **Verifique os Pré-requisitos**
   - Verifique se `gws` está instalado: `gws --version`
   - Verifique autenticação: `gws auth status`
   - Revise comandos disponíveis: `gws admin-reports --help`

2. **Inspecione o Schema do Método**
   - Antes de chamar qualquer método, inspecione seus parâmetros
   - Use `gws schema` para entender os campos obrigatórios
   - Revise tipos de parâmetros e restrições

3. **Execute a Operação**
   - Construa o comando com flags apropriados
   - Use `--params` para parâmetros de query/path
   - Use `--json` para corpo da requisição
   - Trate paginação com `--max-results` ou `--page-token`

4. **Tratamento de Erros**
   - Verifique a saída do comando para erros
   - Revise quotas e limites de taxa da API
   - Trate problemas de autenticação
   - Tente novamente falhas transitórias

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-admin-reports`