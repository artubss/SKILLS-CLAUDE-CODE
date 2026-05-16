---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [resource] [method] [flags]
description: Google Apps Script: Gerenciar e executar projetos Apps Script.
---

# Google Workspace Apps Script

Execute operações do Google Workspace Apps Script: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalado
- Autenticação configurada: Execute `gws auth status` para verificar
- Revise `gws apps-script --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# apps-script (v1)

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para autenticação, flags globais e regras de segurança. Se estiver ausente, execute `gws generate-skills` para criá-lo.

```bash
gws apps-script <resource> <method> [flags]
```

## Comandos Auxiliares

| Comando | Descrição |
|---------|-----------|
| [`+push`](../gws-apps-script-push/SKILL.md) | Enviar arquivos locais para um projeto Apps Script |

## Recursos de API

### processes

  - `list` — Listar informações sobre processos feitos por um usuário ou em seu nome, como tipo de processo e status atual.
  - `listScriptProcesses` — Listar informações sobre processos executados de um script, como tipo de processo e status atual.

### projects

  - `create` — Criar um novo projeto script vazio, sem arquivos de script e com arquivo manifest base.
  - `get` — Obter metadados de um projeto script.
  - `getContent` — Obter o conteúdo do projeto script, incluindo a origem do código e metadados para cada arquivo script.
  - `getMetrics` — Obter dados de métricas para scripts, como número de execuções e usuários ativos.
  - `updateContent` — Atualizar o conteúdo do projeto script especificado. Este conteúdo é armazenado como versão HEAD e é usado quando o script é executado como trigger, no editor de scripts, em modo de visualização de add-on ou como web app ou API do Apps Script em modo de desenvolvimento. Isso limpa todos os arquivos existentes no projeto.
  - `deployments` — Operações no recurso 'deployments'
  - `versions` — Operações no recurso 'versions'

### scripts

  - `run` — 

## Descobrindo Comandos

Antes de chamar qualquer método de API, inspecione-o:

```bash
# Navegar por recursos e métodos
gws apps-script --help

# Inspecionar parâmetros obrigatórios, tipos e padrões de um método
gws schema apps-script.<resource>.<method>
```

Use a saída de `gws schema` para construir seus flags `--params` e `--json`.

## Uso

```bash
# Listar recursos e métodos disponíveis
gws apps-script --help

# Inspecionar schema do método antes de chamar
gws schema apps-script.<resource>.<method>

# Executar comando com argumentos
gws apps-script $ARGUMENTS
```

## Tarefa

Execute a operação Apps Script solicitada: $ARGUMENTS

1. **Verificar Pré-requisitos**
   - Verificar se `gws` está instalado: `gws --version`
   - Verificar autenticação: `gws auth status`
   - Revisar comandos disponíveis: `gws apps-script --help`

2. **Inspecionar Schema do Método**
   - Antes de chamar qualquer método, inspecione seus parâmetros
   - Use `gws schema` para entender campos obrigatórios
   - Revise tipos de parâmetros e restrições

3. **Executar Operação**
   - Construir comando com flags apropriados
   - Usar `--params` para parâmetros de query/path
   - Usar `--json` para corpo da requisição
   - Lidar com paginação com `--max-results` ou `--page-token`

4. **Tratamento de Erros**
   - Verificar saída do comando para erros
   - Revisar quotas de API e limites de taxa
   - Lidar com problemas de autenticação
   - Tentar novamente falhas transitórias

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-apps-script`