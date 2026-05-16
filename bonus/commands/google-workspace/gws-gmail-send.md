---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [resource] [method] [flags]
description: Gmail: Enviar um email.
---

# Google Workspace Gmail Send

Execute operações Google Workspace Gmail Send: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalado
- Autenticação configurada: Execute `gws auth status` para verificar
- Revise `gws gmail-send --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# gmail +send

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para autenticação, flags globais e regras de segurança. Se estiver ausente, execute `gws generate-skills` para criá-lo.

Enviar um email

## Uso

```bash
gws gmail +send --to <EMAIL> --subject <SUBJECT> --body <TEXT>
```

## Flags

| Flag | Obrigatório | Padrão | Descrição |
|------|-------------|--------|-----------|
| `--to` | ✓ | — | Endereço de email do destinatário |
| `--subject` | ✓ | — | Assunto do email |
| `--body` | ✓ | — | Corpo do email (texto simples) |

## Exemplos

```bash
gws gmail +send --to alice@example.com --subject 'Olá' --body 'Oi Alice!'
```

## Dicas

- Formata automaticamente RFC 2822 e realiza codificação base64.
- Para corpos HTML, anexos ou CC/BCC, use a API raw:
- gws gmail users messages send --json '...'

> [!CAUTION]
> Este é um comando de **escrita** — confirme com o usuário antes de executar.

## Veja Também

- [gws-shared](../gws-shared/SKILL.md) — Flags globais e autenticação
- [gws-gmail](../gws-gmail/SKILL.md) — Todos os comandos de enviar, ler e gerenciar email

## Uso

```bash
# Listar recursos e métodos disponíveis
gws gmail-send --help

# Inspecionar schema do método antes de chamar
gws schema gmail-send.<resource>.<method>

# Executar comando com argumentos
gws gmail-send $ARGUMENTS
```

## Tarefa

Execute a operação Gmail Send solicitada: $ARGUMENTS

1. **Verifique Pré-requisitos**
   - Verifique se `gws` está instalado: `gws --version`
   - Confirme autenticação: `gws auth status`
   - Revise comandos disponíveis: `gws gmail-send --help`

2. **Inspecione o Schema do Método**
   - Antes de chamar qualquer método, inspecione seus parâmetros
   - Use `gws schema` para entender os campos obrigatórios
   - Revise tipos de parâmetros e restrições

3. **Execute a Operação**
   - Construa o comando com as flags apropriadas
   - Use `--params` para parâmetros de query/path
   - Use `--json` para body da requisição
   - Trate paginação com `--max-results` ou `--page-token`

4. **Tratamento de Erros**
   - Verifique a saída do comando para erros
   - Revise quotas de API e limites de taxa
   - Trate problemas de autenticação
   - Tente novamente falhas transitórias

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-gmail-send`