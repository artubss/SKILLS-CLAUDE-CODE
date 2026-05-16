---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [recurso] [método] [sinalizadores]
description: Ler e escrever Google Forms.
---

# Google Workspace Forms

Execute operações do Google Workspace Forms: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalado
- Autenticação configurada: Execute `gws auth status` para verificar
- Revise `gws forms --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# forms (v1)

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para autenticação, sinalizadores globais e regras de segurança. Se estiver faltando, execute `gws generate-skills` para criar.

```bash
gws forms <recurso> <método> [sinalizadores]
```

## Recursos da API

### forms

  - `batchUpdate` — Altere o formulário com um lote de atualizações.
  - `create` — Crie um novo formulário usando o título fornecido na mensagem de formulário fornecida na solicitação. *Importante:* Apenas os campos form.info.title e form.info.document_title são copiados para o novo formulário. Todos os outros campos, incluindo descrição do formulário, itens e configurações, não são permitidos. Para criar um novo formulário e adicionar itens, você deve primeiro chamar forms.create para criar um formulário vazio com um título e (opcionalmente) título do documento e, em seguida, chamar forms.update para adicionar os itens.
  - `get` — Obtenha um formulário.
  - `setPublishSettings` — Atualiza as configurações de publicação de um formulário. Formulários herdados não são suportados porque não têm o campo `publish_settings`.
  - `responses` — Operações no recurso 'responses'
  - `watches` — Operações no recurso 'watches'

## Descobrindo Comandos

Antes de chamar qualquer método da API, inspecione-o:

```bash
# Navegue por recursos e métodos
gws forms --help

# Inspecione os parâmetros obrigatórios, tipos e padrões de um método
gws schema forms.<recurso>.<método>
```

Use a saída de `gws schema` para construir seus sinalizadores `--params` e `--json`.

## Uso

```bash
# Liste recursos e métodos disponíveis
gws forms --help

# Inspecione o schema do método antes de chamar
gws schema forms.<recurso>.<método>

# Execute comando com argumentos
gws forms $ARGUMENTS
```

## Tarefa

Execute a operação Forms solicitada: $ARGUMENTS

1. **Verifique os Pré-requisitos**
   - Verifique se `gws` está instalado: `gws --version`
   - Verifique autenticação: `gws auth status`
   - Revise comandos disponíveis: `gws forms --help`

2. **Inspecione o Schema do Método**
   - Antes de chamar qualquer método, inspecione seus parâmetros
   - Use `gws schema` para entender campos obrigatórios
   - Revise tipos de parâmetros e restrições

3. **Execute a Operação**
   - Construa comando com sinalizadores apropriados
   - Use `--params` para parâmetros de consulta/caminho
   - Use `--json` para corpo da solicitação
   - Manipule paginação com `--max-results` ou `--page-token`

4. **Tratamento de Erros**
   - Verifique a saída do comando para erros
   - Revise quotas de API e limites de taxa
   - Trate problemas de autenticação
   - Tente novamente falhas transitórias

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-forms`