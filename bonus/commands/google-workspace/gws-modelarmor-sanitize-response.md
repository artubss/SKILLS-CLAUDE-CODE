---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [resource] [method] [flags]
description: Google Model Armor: Sanitize uma resposta de modelo através de um template Model Armor.
---

# Google Workspace Modelarmor Sanitizar Resposta

Execute operações Google Workspace Modelarmor Sanitizar Resposta: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalado
- Autenticação configurada: Execute `gws auth status` para verificar
- Revise `gws modelarmor-sanitize-response --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# modelarmor +sanitize-response

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para autenticação, flags globais e regras de segurança. Se estiver faltando, execute `gws generate-skills` para criá-lo.

Sanitize uma resposta de modelo através de um template Model Armor

## Uso

```bash
gws modelarmor +sanitize-response --template <NAME>
```

## Flags

| Flag | Obrigatório | Padrão | Descrição |
|------|----------|--------|-------------|
| `--template` | ✓ | — | Nome completo do recurso de template (projects/PROJECT/locations/LOCATION/templates/TEMPLATE) |
| `--text` | — | — | Conteúdo de texto a sanitizar |
| `--json` | — | — | Corpo completo da solicitação JSON (substitui --text) |

## Exemplos

```bash
gws modelarmor +sanitize-response --template projects/P/locations/L/templates/T --text 'saída do modelo'
model_cmd | gws modelarmor +sanitize-response --template ...
```

## Dicas

- Use para segurança de saída (modelo -> usuário).
- Para segurança de entrada (usuário -> modelo), use +sanitize-prompt.

## Veja Também

- [gws-shared](../gws-shared/SKILL.md) — Flags globais e autenticação
- [gws-modelarmor](../gws-modelarmor/SKILL.md) — Todos os comandos de filtro de conteúdo gerado pelo usuário para segurança

## Uso

```bash
# Listar recursos e métodos disponíveis
gws modelarmor-sanitize-response --help

# Inspecionar schema do método antes de chamar
gws schema modelarmor-sanitize-response.<resource>.<method>

# Executar comando com argumentos
gws modelarmor-sanitize-response $ARGUMENTS
```

## Tarefa

Execute a operação Modelarmor Sanitizar Resposta solicitada: $ARGUMENTS

1. **Verificar Pré-requisitos**
   - Verificar se `gws` está instalado: `gws --version`
   - Verificar autenticação: `gws auth status`
   - Revisar comandos disponíveis: `gws modelarmor-sanitize-response --help`

2. **Inspecionar Schema do Método**
   - Antes de chamar qualquer método, inspecione seus parâmetros
   - Use `gws schema` para entender os campos obrigatórios
   - Revise tipos de parâmetro e restrições

3. **Executar Operação**
   - Construir comando com flags apropriadas
   - Use `--params` para parâmetros de query/path
   - Use `--json` para corpo da solicitação
   - Lidar com paginação com `--max-results` ou `--page-token`

4. **Tratamento de Erros**
   - Verificar saída do comando para erros
   - Revisar quotas da API e limites de taxa
   - Lidar com problemas de autenticação
   - Tentar novamente em caso de falhas transitórias

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-modelarmor-sanitize-response`