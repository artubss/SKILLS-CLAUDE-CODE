---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [resource] [method] [flags]
description: Google Model Armor: Filtrar conteúdo gerado por usuários para segurança.
---

# Google Workspace Modelarmor

Execute operações do Google Workspace Modelarmor: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalado
- Autenticação configurada: Execute `gws auth status` para verificar
- Revise `gws modelarmor --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# modelarmor (v1)

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para autenticação, flags globais e regras de segurança. Se ausente, execute `gws generate-skills` para criar.

```bash
gws modelarmor <resource> <method> [flags]
```

## Comandos Auxiliares

| Comando | Descrição |
|---------|-----------|
| [`+sanitize-prompt`](../gws-modelarmor-sanitize-prompt/SKILL.md) | Sanitizar um prompt do usuário através de um template do Model Armor |
| [`+sanitize-response`](../gws-modelarmor-sanitize-response/SKILL.md) | Sanitizar uma resposta do modelo através de um template do Model Armor |
| [`+create-template`](../gws-modelarmor-create-template/SKILL.md) | Criar um novo template do Model Armor |

## Descobrindo Comandos

Antes de chamar qualquer método de API, inspecione-o:

```bash
# Procurar recursos e métodos
gws modelarmor --help

# Inspecionar parâmetros obrigatórios, tipos e padrões de um método
gws schema modelarmor.<resource>.<method>
```

Use a saída de `gws schema` para construir seus flags `--params` e `--json`.

## Uso

```bash
# Listar recursos e métodos disponíveis
gws modelarmor --help

# Inspecionar schema do método antes de chamar
gws schema modelarmor.<resource>.<method>

# Executar comando com argumentos
gws modelarmor $ARGUMENTS
```

## Tarefa

Execute a operação Modelarmor solicitada: $ARGUMENTS

1. **Verificar Pré-requisitos**
   - Verificar se `gws` está instalado: `gws --version`
   - Verificar autenticação: `gws auth status`
   - Revisar comandos disponíveis: `gws modelarmor --help`

2. **Inspecionar Schema do Método**
   - Antes de chamar qualquer método, inspecione seus parâmetros
   - Use `gws schema` para entender os campos obrigatórios
   - Revise tipos de parâmetros e restrições

3. **Executar Operação**
   - Construir comando com flags apropriadas
   - Usar `--params` para parâmetros de query/path
   - Usar `--json` para corpo da requisição
   - Gerenciar paginação com `--max-results` ou `--page-token`

4. **Tratamento de Erros**
   - Verificar saída do comando para erros
   - Revisar quotas de API e limites de taxa
   - Tratar problemas de autenticação
   - Retornar falhas transitórias

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-modelarmor`