---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [resource] [method] [flags]
description: Google Drive: Fazer upload de um arquivo com metadados automáticos.
---

# Google Workspace Drive Upload

Execute operações de Google Workspace Drive Upload: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalado
- Autenticação configurada: Execute `gws auth status` para verificar
- Revise `gws drive-upload --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# drive +upload

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para autenticação, flags globais e regras de segurança. Se estiver faltando, execute `gws generate-skills` para criar.

Fazer upload de um arquivo com metadados automáticos

## Uso

```bash
gws drive +upload <file>
```

## Flags

| Flag | Obrigatório | Padrão | Descrição |
|------|------------|--------|-----------|
| `<file>` | ✓ | — | Caminho do arquivo a fazer upload |
| `--parent` | — | — | ID da pasta pai |
| `--name` | — | — | Nome do arquivo de destino (padrão: nome do arquivo de origem) |

## Exemplos

```bash
gws drive +upload ./report.pdf
gws drive +upload ./report.pdf --parent FOLDER_ID
gws drive +upload ./data.csv --name 'Sales Data.csv'
```

## Dicas

- O tipo MIME é detectado automaticamente.
- O nome do arquivo é inferido do caminho local, a menos que --name seja fornecido.

> [!CAUTION]
> Este é um comando de **escrita** — confirme com o usuário antes de executar.

## Veja Também

- [gws-shared](../gws-shared/SKILL.md) — Flags globais e autenticação
- [gws-drive](../gws-drive/SKILL.md) — Todos os comandos para gerenciar arquivos, pastas e shared drives

## Uso

```bash
# Listar recursos e métodos disponíveis
gws drive-upload --help

# Inspecionar schema do método antes de chamar
gws schema drive-upload.<resource>.<method>

# Executar comando com argumentos
gws drive-upload $ARGUMENTS
```

## Tarefa

Execute a operação de Drive Upload solicitada: $ARGUMENTS

1. **Verificar Pré-requisitos**
   - Verifique se `gws` está instalado: `gws --version`
   - Verifique autenticação: `gws auth status`
   - Revise comandos disponíveis: `gws drive-upload --help`

2. **Inspecionar Schema do Método**
   - Antes de chamar qualquer método, inspecione seus parâmetros
   - Use `gws schema` para entender campos obrigatórios
   - Revise tipos e restrições de parâmetros

3. **Executar Operação**
   - Construa o comando com flags apropriadas
   - Use `--params` para parâmetros de query/path
   - Use `--json` para o corpo da requisição
   - Processe paginação com `--max-results` ou `--page-token`

4. **Tratamento de Erros**
   - Verifique a saída do comando para erros
   - Revise quotas de API e limites de taxa
   - Lidar com problemas de autenticação
   - Tente novamente falhas transitórias

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-drive-upload`