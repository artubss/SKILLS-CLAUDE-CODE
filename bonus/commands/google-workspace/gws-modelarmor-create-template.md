---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [recurso] [método] [sinalizadores]
description: Google Model Armor: Criar um novo modelo de proteção.
---

# Google Workspace Modelarmor Criar Modelo

Execute operações Google Workspace Modelarmor Criar Modelo: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalada
- Autenticação configurada: Execute `gws auth status` para verificar
- Revise `gws modelarmor-create-template --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# modelarmor +create-template

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para autenticação, sinalizadores globais e regras de segurança. Se estiver faltando, execute `gws generate-skills` para criar.

Criar um novo modelo de proteção

## Uso

```bash
gws modelarmor +create-template --project <PROJECT> --location <LOCATION> --template-id <ID>
```

## Sinalizadores

| Sinalizador | Obrigatório | Padrão | Descrição |
|------|----------|---------|-------------|
| `--project` | ✓ | — | ID do projeto GCP |
| `--location` | ✓ | — | Localização GCP (ex: us-central1) |
| `--template-id` | ✓ | — | ID do modelo a criar |
| `--preset` | — | — | Usar um modelo predefinido: jailbreak |
| `--json` | — | — | Corpo JSON para a configuração do modelo (sobrescreve --preset) |

## Exemplos

```bash
gws modelarmor +create-template --project P --location us-central1 --template-id my-tmpl --preset jailbreak
gws modelarmor +create-template --project P --location us-central1 --template-id my-tmpl --json '{...}'
```

## Dicas

- Usa o preset jailbreak por padrão se nem --preset nem --json forem fornecidos.
- Use o nome do modelo resultante com +sanitize-prompt e +sanitize-response.

> [!CAUTION]
> Este é um comando de **escrita** — confirme com o usuário antes de executar.

## Veja Também

- [gws-shared](../gws-shared/SKILL.md) — Sinalizadores globais e autenticação
- [gws-modelarmor](../gws-modelarmor/SKILL.md) — Todos os comandos para filtrar conteúdo gerado por usuários para segurança

## Uso

```bash
# Listar recursos e métodos disponíveis
gws modelarmor-create-template --help

# Inspecionar schema do método antes de chamar
gws schema modelarmor-create-template.<resource>.<method>

# Executar comando com argumentos
gws modelarmor-create-template $ARGUMENTS
```

## Tarefa

Execute a operação Modelarmor Criar Modelo solicitada: $ARGUMENTS

1. **Verificar Pré-requisitos**
   - Verifique se `gws` está instalada: `gws --version`
   - Verifique a autenticação: `gws auth status`
   - Revise os comandos disponíveis: `gws modelarmor-create-template --help`

2. **Inspecionar Schema do Método**
   - Antes de chamar qualquer método, inspecione seus parâmetros
   - Use `gws schema` para entender os campos obrigatórios
   - Revise os tipos de parâmetros e restrições

3. **Executar Operação**
   - Construa o comando com os sinalizadores apropriados
   - Use `--params` para parâmetros de query/path
   - Use `--json` para o corpo da requisição
   - Lidar com paginação com `--max-results` ou `--page-token`

4. **Tratamento de Erros**
   - Verifique a saída do comando para erros
   - Revise quotas de API e limites de taxa
   - Trate problemas de autenticação
   - Tente novamente falhas transitórias

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-modelarmor-create-template`