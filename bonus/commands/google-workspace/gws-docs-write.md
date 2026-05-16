---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [resource] [method] [flags]
description: Google Docs: Acrescentar texto a um documento.
---

# Google Workspace Docs Write

Execute operações Google Workspace Docs Write: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalado
- Autenticação configurada: Execute `gws auth status` para verificar
- Consulte `gws docs-write --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# docs +write

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para auth, flags globais e regras de segurança. Se estiver faltando, execute `gws generate-skills` para criá-lo.

Acrescentar texto a um documento

## Uso

```bash
gws docs +write --document <ID> --text <TEXT>
```

## Flags

| Flag | Obrigatória | Padrão | Descrição |
|------|----------|---------|-------------|
| `--document` | ✓ | — | ID do documento |
| `--text` | ✓ | — | Texto a acrescentar (texto simples) |

## Exemplos

```bash
gws docs +write --document DOC_ID --text 'Olá, mundo!'
```

## Dicas

- O texto é inserido ao final do corpo do documento.
- Para formatação avançada, use a API batchUpdate bruta em vez disso.

> [!CAUTION]
> Este é um comando **write** — confirme com o usuário antes de executar.

## Ver Também

- [gws-shared](../gws-shared/SKILL.md) — Flags globais e autenticação
- [gws-docs](../gws-docs/SKILL.md) — Todos os comandos de leitura e escrita do Google Docs

## Uso

```bash
# Listar recursos e métodos disponíveis
gws docs-write --help

# Inspecionar esquema de método antes de chamar
gws schema docs-write.<resource>.<method>

# Executar comando com argumentos
gws docs-write $ARGUMENTS
```

## Tarefa

Execute a operação Docs Write solicitada: $ARGUMENTS

1. **Verificar Pré-requisitos**
   - Verificar se `gws` está instalado: `gws --version`
   - Verificar autenticação: `gws auth status`
   - Consultar comandos disponíveis: `gws docs-write --help`

2. **Inspecionar Esquema de Método**
   - Antes de chamar qualquer método, inspecione seus parâmetros
   - Use `gws schema` para entender campos obrigatórios
   - Revise tipos de parâmetros e restrições

3. **Executar Operação**
   - Construir comando com flags apropriadas
   - Usar `--params` para parâmetros de query/path
   - Usar `--json` para corpo da requisição
   - Lidar com paginação com `--max-results` ou `--page-token`

4. **Tratamento de Erros**
   - Verificar saída do comando para erros
   - Revisar quotas de API e limites de taxa
   - Lidar com problemas de autenticação
   - Tentar novamente falhas transientes

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-docs-write`