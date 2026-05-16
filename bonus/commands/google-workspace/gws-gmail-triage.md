---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [resource] [method] [flags]
description: Gmail: Mostrar resumo da caixa de entrada não lida (remetente, assunto, data).
---

# Google Workspace Gmail Triagem

Execute operações de Triagem Gmail do Google Workspace: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalado
- Autenticação configurada: Execute `gws auth status` para verificar
- Consulte `gws gmail-triage --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# gmail +triage

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para autenticação, flags globais e regras de segurança. Se ausente, execute `gws generate-skills` para criá-lo.

Mostrar resumo da caixa de entrada não lida (remetente, assunto, data)

## Uso

```bash
gws gmail +triage
```

## Flags

| Flag | Obrigatória | Padrão | Descrição |
|------|----------|---------|-------------|
| `--max` | — | 20 | Máximo de mensagens a exibir (padrão: 20) |
| `--query` | — | — | Consulta de busca do Gmail (padrão: is:unread) |
| `--labels` | — | — | Incluir nomes de rótulos na saída |

## Exemplos

```bash
gws gmail +triage
gws gmail +triage --max 5 --query 'from:boss'
gws gmail +triage --format json | jq '.[].subject'
gws gmail +triage --labels
```

## Dicas

- Somente leitura — nunca modifica sua caixa de correio.
- Padrão para formato de saída em tabela.

## Veja Também

- [gws-shared](../gws-shared/SKILL.md) — Flags globais e autenticação
- [gws-gmail](../gws-gmail/SKILL.md) — Todos os comandos de envio, leitura e gerenciamento de email

## Uso

```bash
# Listar recursos e métodos disponíveis
gws gmail-triage --help

# Inspecionar schema do método antes de chamar
gws schema gmail-triage.<resource>.<method>

# Executar comando com argumentos
gws gmail-triage $ARGUMENTS
```

## Tarefa

Execute a operação de Triagem Gmail solicitada: $ARGUMENTS

1. **Verificar Pré-requisitos**
   - Verificar se `gws` está instalado: `gws --version`
   - Verificar autenticação: `gws auth status`
   - Consultar comandos disponíveis: `gws gmail-triage --help`

2. **Inspecionar Schema do Método**
   - Antes de chamar qualquer método, inspecionar seus parâmetros
   - Usar `gws schema` para entender campos obrigatórios
   - Revisar tipos de parâmetros e restrições

3. **Executar Operação**
   - Construir comando com flags apropriadas
   - Usar `--params` para parâmetros de query/path
   - Usar `--json` para corpo da requisição
   - Lidar com paginação usando `--max-results` ou `--page-token`

4. **Tratamento de Erros**
   - Verificar erros na saída do comando
   - Revisar quotas de API e limites de taxa
   - Tratar problemas de autenticação
   - Repetir falhas transitórias

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-gmail-triage`