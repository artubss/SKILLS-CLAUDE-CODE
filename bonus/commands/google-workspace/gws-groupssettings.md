---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [resource] [method] [flags]
description: Gerenciar configurações do Google Groups.
---

# Google Workspace Groupssettings

Execute operações do Google Workspace Groupssettings: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalado
- Autenticação configurada: Execute `gws auth status` para verificar
- Revise `gws groupssettings --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# groupssettings (v1)

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para autenticação, flags globais e regras de segurança. Se não existir, execute `gws generate-skills` para criar.

```bash
gws groupssettings <resource> <method> [flags]
```

## Recursos da API

### groups

  - `get` — Obtém um recurso por id.
  - `patch` — Atualiza um recurso existente. Este método suporta semântica de patch.
  - `update` — Atualiza um recurso existente.

## Descobrindo Comandos

Antes de chamar qualquer método de API, inspecione-o:

```bash
# Procurar recursos e métodos
gws groupssettings --help

# Inspecionar parâmetros obrigatórios, tipos e padrões de um método
gws schema groupssettings.<resource>.<method>
```

Use a saída do `gws schema` para construir suas flags `--params` e `--json`.

## Uso

```bash
# Listar recursos e métodos disponíveis
gws groupssettings --help

# Inspecionar o schema de um método antes de chamar
gws schema groupssettings.<resource>.<method>

# Executar comando com argumentos
gws groupssettings $ARGUMENTS
```

## Tarefa

Execute a operação Groupssettings solicitada: $ARGUMENTS

1. **Verificar Pré-requisitos**
   - Verificar se `gws` está instalado: `gws --version`
   - Verificar autenticação: `gws auth status`
   - Revisar comandos disponíveis: `gws groupssettings --help`

2. **Inspecionar Schema do Método**
   - Antes de chamar qualquer método, inspecione seus parâmetros
   - Use `gws schema` para entender os campos obrigatórios
   - Revise tipos de parâmetros e restrições

3. **Executar Operação**
   - Construir comando com flags apropriadas
   - Usar `--params` para parâmetros de query/path
   - Usar `--json` para o corpo da requisição
   - Lidar com paginação usando `--max-results` ou `--page-token`

4. **Tratamento de Erros**
   - Verificar a saída do comando para erros
   - Revisar quotas de API e limites de taxa
   - Lidar com problemas de autenticação
   - Repetir falhas transitórias

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-groupssettings`