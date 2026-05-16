---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Criar uma análise pós-incidente no Google Docs, agendar análise no Google Calendar e notificar via Chat.
---

# Configuração de Análise Pós-Incidente

Execute workflow do Google Workspace: $ARGUMENTS

# Configurar Análise Pós-Incidente

> **PRÉ-REQUISITO:** Carregue as seguintes skills para executar esta receita: `gws-docs`, `gws-calendar`, `gws-chat`

Crie uma análise pós-incidente no Google Docs, agende uma análise no Google Calendar e notifique via Chat.

## Etapas

1. Criar documento de análise: `gws docs +write --title 'Análise Pós-Incidente: [Incident]' --body '## Resumo\n\n## Cronograma\n\n## Causa Raiz\n\n## Itens de Ação'`
2. Agendar reunião de análise: `gws calendar +insert --summary 'Análise Pós-Incidente: [Incident]' --attendees team@company.com --start 'next monday 14:00' --duration 60`
3. Notificar no Chat: `gws chat +send --space spaces/ENG_SPACE --text '🔍 Análise pós-incidente agendada para [Incident].'`

## Tarefa

Execute este workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-Requisitos**
   - Verifique se a CLI `gws` está instalada: `gws --version`
   - Confirme autenticação: `gws auth status`
   - Carregue as skills obrigatórias do GWS (confira a seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Analise parâmetros de tarefa a partir de $ARGUMENTS
   - Valide entradas obrigatórias
   - Prepare payloads JSON e flags

3. **Executar Etapas do Workflow**
   - Siga as etapas descritas acima
   - Substitua IDs de placeholder por valores reais
   - Trate erros e tentativas novamente
   - Registre progresso e resultados

4. **Verificar Resultados**
   - Confirme que cada etapa foi concluída com sucesso
   - Verifique alterações no Google Workspace
   - Relate status final e qualquer problema

## Dicas

- Use a flag `--dry-run` quando disponível para visualizar alterações
- Sempre inspecione schemas de API antes de chamar: `gws schema <service>.<resource>.<method>`
- Verifique a ajuda do comando para todas as flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `recipe-post-mortem-setup`