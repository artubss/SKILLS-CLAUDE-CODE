---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [resource] [method] [flags]
description: Google Apps Script: Fazer upload de arquivos locais para um projeto Apps Script.
---

# Google Workspace Apps Script Push

Execute operações Google Workspace Apps Script Push: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalado
- Autenticação configurada: Execute `gws auth status` para verificar
- Revise `gws apps-script-push --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# apps-script +push

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para autenticação, flags globais e regras de segurança. Se ausente, execute `gws generate-skills` para criá-lo.

Fazer upload de arquivos locais para um projeto Apps Script

## Uso

```bash
gws apps-script +push --script <ID>
```

## Flags

| Flag | Obrigatório | Padrão | Descrição |
|------|----------|---------|-------------|
| `--script` | ✓ | — | ID do Projeto Script |
| `--dir` | — | — | Diretório contendo arquivos de script (usa diretório atual por padrão) |

## Exemplos

```bash
gws script +push --script SCRIPT_ID
gws script +push --script SCRIPT_ID --dir ./src
```

## Dicas

- Suporta arquivos .gs, .js, .html e appsscript.json.
- Ignora automaticamente arquivos ocultos e node_modules.
- Isso substitui TODOS os arquivos do projeto.

> [!CAUTION]
> Este é um comando de **escrita** — confirme com o usuário antes de executar.

## Veja Também

- [gws-shared](../gws-shared/SKILL.md) — Flags globais e autenticação
- [gws-apps-script](../gws-apps-script/SKILL.md) — Todos os comandos de gerenciar e executar projetos apps script

## Uso

```bash
# Listar recursos e métodos disponíveis
gws apps-script-push --help

# Inspecionar schema do método antes de chamar
gws schema apps-script-push.<resource>.<method>

# Executar comando com argumentos
gws apps-script-push $ARGUMENTS
```

## Tarefa

Execute a operação Apps Script Push solicitada: $ARGUMENTS

1. **Verificar Pré-requisitos**
   - Verificar se `gws` está instalado: `gws --version`
   - Verificar autenticação: `gws auth status`
   - Revisar comandos disponíveis: `gws apps-script-push --help`

2. **Inspecionar Schema do Método**
   - Antes de chamar qualquer método, inspecione seus parâmetros
   - Use `gws schema` para entender campos obrigatórios
   - Revise tipos de parâmetros e restrições

3. **Executar Operação**
   - Construir comando com flags apropriadas
   - Use `--params` para parâmetros de query/path
   - Use `--json` para corpo da requisição
   - Lidar com paginação usando `--max-results` ou `--page-token`

4. **Tratamento de Erros**
   - Verificar output do comando para erros
   - Revisar quotas de API e rate limits
   - Lidar com problemas de autenticação
   - Tentar novamente falhas transitórias

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-apps-script-push`