---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [resource] [method] [flags]
description: Google Slides: Ler e escrever apresentações.
---

# Google Workspace Slides

Execute operações do Google Workspace Slides: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalado
- Autenticação configurada: Execute `gws auth status` para verificar
- Revise `gws slides --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# slides (v1)

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para autenticação, flags globais e regras de segurança. Se estiver faltando, execute `gws generate-skills` para criá-lo.

```bash
gws slides <resource> <method> [flags]
```

## Recursos da API

### presentations

  - `batchUpdate` — Aplica uma ou mais atualizações à apresentação. Cada requisição é validada antes de ser aplicada. Se alguma requisição não for válida, toda a requisição falhará e nada será aplicado. Algumas requisições têm respostas para fornecer informações sobre como foram aplicadas. Outras requisições não precisam retornar informações; cada uma retorna uma resposta vazia. A ordem das respostas corresponde à ordem das requisições.
  - `create` — Cria uma apresentação em branco usando o título fornecido na requisição. Se um `presentationId` for fornecido, ele será usado como ID da nova apresentação. Caso contrário, um novo ID será gerado. Outros campos na requisição, incluindo qualquer conteúdo fornecido, serão ignorados. Retorna a apresentação criada.
  - `get` — Obtém a versão mais recente da apresentação especificada.
  - `pages` — Operações no recurso 'pages'

## Descobrindo Comandos

Antes de chamar qualquer método da API, inspecione-o:

```bash
# Navegue por recursos e métodos
gws slides --help

# Inspecione os parâmetros obrigatórios, tipos e padrões de um método
gws schema slides.<resource>.<method>
```

Use a saída de `gws schema` para construir seus flags `--params` e `--json`.

## Uso

```bash
# Liste recursos e métodos disponíveis
gws slides --help

# Inspecione o schema do método antes de chamá-lo
gws schema slides.<resource>.<method>

# Execute o comando com argumentos
gws slides $ARGUMENTS
```

## Tarefa

Execute a operação de Slides solicitada: $ARGUMENTS

1. **Verifique Pré-requisitos**
   - Verifique se `gws` está instalado: `gws --version`
   - Verifique autenticação: `gws auth status`
   - Revise comandos disponíveis: `gws slides --help`

2. **Inspecione o Schema do Método**
   - Antes de chamar qualquer método, inspecione seus parâmetros
   - Use `gws schema` para entender campos obrigatórios
   - Revise tipos de parâmetros e restrições

3. **Execute a Operação**
   - Construa o comando com flags apropriadas
   - Use `--params` para parâmetros de query/path
   - Use `--json` para o corpo da requisição
   - Gerencie paginação com `--max-results` ou `--page-token`

4. **Tratamento de Erros**
   - Verifique a saída do comando para erros
   - Revise quotas de API e limites de taxa
   - Gerencie problemas de autenticação
   - Tente novamente falhas transitórias

---

**Licença**: Apache License 2.0
**Origem**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-slides`