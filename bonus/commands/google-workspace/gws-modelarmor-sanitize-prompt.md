---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [resource] [method] [flags]
description: Google Model Armor: Sanitizar um prompt do usuário através de um template Model Armor.
---

# Google Workspace Modelarmor Sanitizar Prompt

Execute operações Google Workspace Modelarmor Sanitizar Prompt: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalado
- Autenticação configurada: Execute `gws auth status` para verificar
- Revise `gws modelarmor-sanitize-prompt --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# modelarmor +sanitize-prompt

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para autenticação, flags globais e regras de segurança. Se não encontrado, execute `gws generate-skills` para criar.

Sanitizar um prompt do usuário através de um template Model Armor

## Uso

```bash
gws modelarmor +sanitize-prompt --template <NAME>
```

## Flags

| Flag | Obrigatória | Padrão | Descrição |
|------|----------|--------|-------------|
| `--template` | ✓ | — | Nome completo do recurso template (projects/PROJECT/locations/LOCATION/templates/TEMPLATE) |
| `--text` | — | — | Conteúdo de texto a sanitizar |
| `--json` | — | — | Corpo completo da requisição JSON (sobrescreve --text) |

## Exemplos

```bash
gws modelarmor +sanitize-prompt --template projects/P/locations/L/templates/T --text 'user input'
echo 'prompt' | gws modelarmor +sanitize-prompt --template ...
```

## Dicas

- Se nem --text nem --json forem fornecidos, lê a partir de stdin.
- Para segurança de saída, use +sanitize-response em seu lugar.

## Veja Também

- [gws-shared](../gws-shared/SKILL.md) — Flags globais e autenticação
- [gws-modelarmor](../gws-modelarmor/SKILL.md) — Todos os comandos para filtrar conteúdo gerado pelo usuário por questões de segurança

## Uso

```bash
# Listar recursos e métodos disponíveis
gws modelarmor-sanitize-prompt --help

# Inspecionar schema do método antes de chamar
gws schema modelarmor-sanitize-prompt.<resource>.<method>

# Executar comando com argumentos
gws modelarmor-sanitize-prompt $ARGUMENTS
```

## Tarefa

Execute a operação Modelarmor Sanitizar Prompt solicitada: $ARGUMENTS

1. **Verificar Pré-requisitos**
   - Verificar se `gws` está instalado: `gws --version`
   - Verificar autenticação: `gws auth status`
   - Revisar comandos disponíveis: `gws modelarmor-sanitize-prompt --help`

2. **Inspecionar Schema do Método**
   - Antes de chamar qualquer método, inspecione seus parâmetros
   - Use `gws schema` para entender campos obrigatórios
   - Revise tipos e restrições de parâmetros

3. **Executar Operação**
   - Construa comando com flags apropriadas
   - Use `--params` para parâmetros de query/path
   - Use `--json` para corpo da requisição
   - Manipule paginação com `--max-results` ou `--page-token`

4. **Tratamento de Erros**
   - Verifique saída do comando para erros
   - Revise quotas e limites de taxa da API
   - Trate problemas de autenticação
   - Retente falhas transitórias

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-modelarmor-sanitize-prompt`