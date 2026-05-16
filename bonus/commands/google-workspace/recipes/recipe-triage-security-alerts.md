---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Listar e revisar alertas de segurança do Google Workspace do Alert Center.
---

# Triagem de Alertas de Segurança

Execute workflow do Google Workspace: $ARGUMENTS

# Triagem de Alertas de Segurança do Google Workspace

> **PRÉ-REQUISITO:** Carregue as seguintes skills para executar esta receita: `gws-alertcenter`

Listar e revisar alertas de segurança do Google Workspace do Alert Center.

## Etapas

1. Listar alertas ativos: `gws alertcenter alerts list --format table`
2. Obter detalhes do alerta: `gws alertcenter alerts get --params '{"alertId": "ALERT_ID"}'`
3. Reconhecer um alerta: `gws alertcenter alerts undelete --params '{"alertId": "ALERT_ID"}'`

## Tarefa

Execute este workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-requisitos**
   - Verifique se a CLI `gws` está instalada: `gws --version`
   - Confirme a autenticação: `gws auth status`
   - Carregue as skills de GWS necessárias (verifique a seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Analise os parâmetros da tarefa em $ARGUMENTS
   - Valide as entradas obrigatórias
   - Prepare payloads JSON e flags

3. **Execute as Etapas do Workflow**
   - Siga as etapas descritas acima
   - Substitua IDs de placeholder pelos valores reais
   - Trate erros e retentativas
   - Registre o progresso e os resultados

4. **Verifique os Resultados**
   - Confirme que cada etapa foi concluída com êxito
   - Verifique as mudanças no Google Workspace
   - Reporte o status final e qualquer problema

## Dicas

- Use a flag `--dry-run` quando disponível para visualizar alterações
- Sempre inspecione schemas de API antes de chamar: `gws schema <service>.<resource>.<method>`
- Verifique a ajuda de comando para todas as flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `recipe-triage-security-alerts`