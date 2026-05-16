---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [recurso] [método] [flags]
description: Gerenciar conferências do Google Meet.
---

# Google Workspace Meet

Execute operações do Google Workspace Meet: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalado
- Autenticação configurada: Execute `gws auth status` para verificar
- Consulte `gws meet --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# meet (v2)

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para autenticação, flags globais e regras de segurança. Se estiver ausente, execute `gws generate-skills` para criar.

```bash
gws meet <recurso> <método> [flags]
```

## Recursos de API

### conferenceRecords

  - `get` — Obtém um registro de conferência por ID de conferência.
  - `list` — Lista os registros de conferência. Por padrão, ordenados por hora de início em ordem decrescente.
  - `participants` — Operações no recurso 'participants'
  - `recordings` — Operações no recurso 'recordings'
  - `transcripts` — Operações no recurso 'transcripts'

### spaces

  - `create` — Cria um espaço.
  - `endActiveConference` — Encerra uma conferência ativa (se houver). Para um exemplo, consulte [Encerrar conferência ativa](https://developers.google.com/workspace/meet/api/guides/meeting-spaces#end-active-conference).
  - `get` — Obtém detalhes sobre um espaço de reunião. Para um exemplo, consulte [Obter um espaço de reunião](https://developers.google.com/workspace/meet/api/guides/meeting-spaces#get-meeting-space).
  - `patch` — Atualiza detalhes sobre um espaço de reunião. Para um exemplo, consulte [Atualizar um espaço de reunião](https://developers.google.com/workspace/meet/api/guides/meeting-spaces#update-meeting-space).

## Descobrindo Comandos

Antes de chamar qualquer método de API, inspecione-o:

```bash
# Navegue por recursos e métodos
gws meet --help

# Inspecione parâmetros obrigatórios, tipos e padrões de um método
gws schema meet.<recurso>.<método>
```

Use a saída de `gws schema` para construir seus flags `--params` e `--json`.

## Uso

```bash
# Lista recursos e métodos disponíveis
gws meet --help

# Inspecione o schema do método antes de chamar
gws schema meet.<recurso>.<método>

# Execute comando com argumentos
gws meet $ARGUMENTS
```

## Tarefa

Execute a operação Meet solicitada: $ARGUMENTS

1. **Verifique Pré-requisitos**
   - Verifique se `gws` está instalado: `gws --version`
   - Verifique autenticação: `gws auth status`
   - Revise comandos disponíveis: `gws meet --help`

2. **Inspecione o Schema do Método**
   - Antes de chamar qualquer método, inspecione seus parâmetros
   - Use `gws schema` para compreender campos obrigatórios
   - Revise tipos de parâmetro e restrições

3. **Execute a Operação**
   - Construa comando com flags apropriados
   - Use `--params` para parâmetros de query/path
   - Use `--json` para corpo da solicitação
   - Manipule paginação com `--max-results` ou `--page-token`

4. **Tratamento de Erros**
   - Verifique erros na saída do comando
   - Revise quotas de API e limites de taxa
   - Trate problemas de autenticação
   - Repita falhas transitórias

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-meet`