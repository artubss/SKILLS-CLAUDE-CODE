---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [resource] [method] [flags]
description: Google Chat: Enviar uma mensagem para um espaço.
---

# Google Workspace Chat Send

Execute operações do Google Workspace Chat Send: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalado
- Autenticação configurada: Execute `gws auth status` para verificar
- Revise `gws chat-send --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# chat +send

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para autenticação, flags globais e regras de segurança. Se estiver faltando, execute `gws generate-skills` para criá-lo.

Enviar uma mensagem para um espaço

## Uso

```bash
gws chat +send --space <NAME> --text <TEXT>
```

## Flags

| Flag | Obrigatório | Padrão | Descrição |
|------|----------|---------|-------------|
| `--space` | ✓ | — | Nome do espaço (ex: spaces/AAAA...) |
| `--text` | ✓ | — | Texto da mensagem (texto simples) |

## Exemplos

```bash
gws chat +send --space spaces/AAAAxxxx --text 'Hello team!'
```

## Dicas

- Use 'gws chat spaces list' para encontrar nomes de espaços.
- Para cards ou respostas em thread, use a API raw.

> [!CAUTION]
> Este é um comando de **escrita** — confirme com o usuário antes de executar.

## Veja Também

- [gws-shared](../gws-shared/SKILL.md) — Flags globais e autenticação
- [gws-chat](../gws-chat/SKILL.md) — Todos os comandos para gerenciar espaços e mensagens de chat

## Uso

```bash
# Listar recursos e métodos disponíveis
gws chat-send --help

# Inspecionar schema do método antes de chamar
gws schema chat-send.<resource>.<method>

# Executar comando com argumentos
gws chat-send $ARGUMENTS
```

## Tarefa

Execute a operação de Chat Send solicitada: $ARGUMENTS

1. **Verificar Pré-requisitos**
   - Verificar se `gws` está instalado: `gws --version`
   - Verificar autenticação: `gws auth status`
   - Revisar comandos disponíveis: `gws chat-send --help`

2. **Inspecionar Schema do Método**
   - Antes de chamar qualquer método, inspecione seus parâmetros
   - Use `gws schema` para entender os campos obrigatórios
   - Revise os tipos de parâmetro e restrições

3. **Executar Operação**
   - Construir comando com flags apropriadas
   - Use `--params` para parâmetros de query/path
   - Use `--json` para corpo da requisição
   - Trate paginação com `--max-results` ou `--page-token`

4. **Tratamento de Erros**
   - Verifique erros na saída do comando
   - Revise quotas de API e limites de taxa
   - Trate problemas de autenticação
   - Tente novamente falhas transitórias

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-chat-send`