---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Criar um espaço de reunião Google Meet e compartilhar o link de acesso.
---

# Criar Espaço Meet

Execute o workflow do Google Workspace: $ARGUMENTS

# Criar uma Conferência Google Meet

> **PRÉ-REQUISITO:** Carregue as seguintes skills para executar esta receita: `gws-meet`, `gws-gmail`

Crie um espaço de reunião Google Meet e compartilhe o link de acesso.

## Etapas

1. Criar espaço de reunião: `gws meet spaces create --json '{"config": {"accessType": "OPEN"}}'`
2. Copie o URI da reunião da resposta
3. Enviar por email o link: `gws gmail +send --to team@company.com --subject 'Junte-se à reunião' --body 'Acesse aqui: MEETING_URI'`

## Tarefa

Execute este workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-requisitos**
   - Verifique se a CLI `gws` está instalada: `gws --version`
   - Confirme a autenticação: `gws auth status`
   - Carregue as skills necessárias do GWS (verifique a seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Analise os parâmetros da tarefa em $ARGUMENTS
   - Valide as entradas obrigatórias
   - Prepare payloads JSON e flags

3. **Executar Etapas do Workflow**
   - Siga as etapas descritas acima
   - Substitua IDs de placeholder pelos valores reais
   - Trate erros e tentativas de reexecução
   - Registre progresso e resultados

4. **Verificar Resultados**
   - Confirme que cada etapa foi concluída com sucesso
   - Verifique as alterações no Google Workspace
   - Relate o status final e qualquer problema

## Dicas

- Use a flag `--dry-run` quando disponível para visualizar alterações
- Sempre inspecione schemas de API antes de chamar: `gws schema <service>.<resource>.<method>`
- Verifique a ajuda de comando para todas as flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `recipe-create-meet-space`