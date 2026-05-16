---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Envie um comunicado da equipe via Gmail e Google Chat.
---

# Enviar Comunicado da Equipe

Execute workflow do Google Workspace: $ARGUMENTS

# Anunciar via Gmail e Google Chat

> **PRÉ-REQUISITO:** Carregue as seguintes skills para executar esta receita: `gws-gmail`, `gws-chat`

Envie um comunicado da equipe via Gmail e Google Chat.

## Passos

1. Enviar email: `gws gmail +send --to team@company.com --subject 'Important Update' --body 'Please review the attached policy changes.'`
2. Postar no Chat: `gws chat +send --space spaces/TEAM_SPACE --text '📢 Important Update: Please check your email for policy changes.'`

## Tarefa

Execute este workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-requisitos**
   - Verifique se a CLI `gws` está instalada: `gws --version`
   - Confirme a autenticação: `gws auth status`
   - Carregue as skills GWS necessárias (verifique a seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Analise os parâmetros da tarefa em $ARGUMENTS
   - Valide as entradas obrigatórias
   - Prepare payloads JSON e flags

3. **Execute as Etapas do Workflow**
   - Siga as etapas descritas acima
   - Substitua IDs de placeholder pelos valores reais
   - Trate erros e tente novamente
   - Registre o progresso e os resultados

4. **Verifique os Resultados**
   - Confirme que cada etapa foi concluída com sucesso
   - Verifique as mudanças no Google Workspace
   - Informe o status final e qualquer problema

## Dicas

- Use a flag `--dry-run` quando disponível para visualizar as mudanças
- Sempre inspecione os schemas da API antes de chamar: `gws schema <service>.<resource>.<method>`
- Verifique a ajuda de comando para todas as flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `recipe-send-team-announcement`