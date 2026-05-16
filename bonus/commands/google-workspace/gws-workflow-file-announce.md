---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [resource] [method] [flags]
description: Google Workflow: Anunciar um arquivo do Drive em um espaço Chat.
---

# Google Workspace Workflow File Announce

Execute operações Google Workspace Workflow File Announce: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalado
- Autenticação configurada: Execute `gws auth status` para verificar
- Revise `gws workflow-file-announce --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# workflow +file-announce

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para autenticação, flags globais e regras de segurança. Se estiver ausente, execute `gws generate-skills` para criá-lo.

Anuncie um arquivo do Drive em um espaço Chat

## Uso

```bash
gws workflow +file-announce --file-id <ID> --space <SPACE>
```

## Flags

| Flag | Obrigatório | Padrão | Descrição |
|------|----------|---------|-------------|
| `--file-id` | ✓ | — | ID do arquivo do Drive a anunciar |
| `--space` | ✓ | — | Nome do espaço Chat (ex: spaces/SPACE_ID) |
| `--message` | — | — | Mensagem de anúncio personalizada |
| `--format` | — | — | Formato de saída: json (padrão), table, yaml, csv |

## Exemplos

```bash
gws workflow +file-announce --file-id FILE_ID --space spaces/ABC123
gws workflow +file-announce --file-id FILE_ID --space spaces/ABC123 --message 'Confira isso!'
```

## Dicas

- Este é um comando de escrita — envia uma mensagem Chat.
- Use `gws drive +upload` primeiro para fazer upload do arquivo e então o anuncie aqui.
- Busca o nome do arquivo no Drive para construir o anúncio.

## Veja também

- [gws-shared](../gws-shared/SKILL.md) — Flags globais e autenticação
- [gws-workflow](../gws-workflow/SKILL.md) — Todos os comandos de workflows de produtividade entre serviços

## Uso

```bash
# Listar recursos e métodos disponíveis
gws workflow-file-announce --help

# Inspecionar schema do método antes de chamar
gws schema workflow-file-announce.<resource>.<method>

# Executar comando com argumentos
gws workflow-file-announce $ARGUMENTS
```

## Tarefa

Execute a operação Workflow File Announce solicitada: $ARGUMENTS

1. **Verificar Pré-requisitos**
   - Verificar se `gws` está instalado: `gws --version`
   - Verificar autenticação: `gws auth status`
   - Revisar comandos disponíveis: `gws workflow-file-announce --help`

2. **Inspecionar Schema do Método**
   - Antes de chamar qualquer método, inspecione seus parâmetros
   - Use `gws schema` para entender campos obrigatórios
   - Revise tipos de parâmetros e restrições

3. **Executar Operação**
   - Construa o comando com flags apropriadas
   - Use `--params` para parâmetros de query/path
   - Use `--json` para corpo da requisição
   - Trate paginação com `--max-results` ou `--page-token`

4. **Tratamento de Erros**
   - Verifique a saída do comando em busca de erros
   - Revise quotas de API e limites de taxa
   - Trate problemas de autenticação
   - Retentar falhas transitórias

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-workflow-file-announce`