---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Ativar resposta automática de ausência do Gmail com mensagem personalizada e intervalo de datas.
---

# Criar Resposta de Férias

Executar workflow do Google Workspace: $ARGUMENTS

# Configurar Resposta de Férias do Gmail

> **PRÉ-REQUISITO:** Carregue as seguintes competências para executar esta receita: `gws-gmail`

Ativar uma resposta automática de ausência do Gmail com mensagem personalizada e intervalo de datas.

## Etapas

1. Ativar resposta de férias: `gws gmail users settings updateVacation --params '{"userId": "me"}' --json '{"enableAutoReply": true, "responseSubject": "Out of Office", "responseBodyPlainText": "I am out of the office until Jan 20. For urgent matters, contact backup@company.com.", "restrictToContacts": false, "restrictToDomain": false}'`
2. Verificar configurações: `gws gmail users settings getVacation --params '{"userId": "me"}'`
3. Desativar ao retornar: `gws gmail users settings updateVacation --params '{"userId": "me"}' --json '{"enableAutoReply": false}'`

## Tarefa

Execute este workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-requisitos**
   - Verificar se a CLI `gws` está instalada: `gws --version`
   - Confirmar autenticação: `gws auth status`
   - Carregar as competências GWS necessárias (verifique a seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Analisar parâmetros da tarefa a partir de $ARGUMENTS
   - Validar entradas obrigatórias
   - Preparar payloads JSON e flags

3. **Executar Etapas do Workflow**
   - Seguir as etapas descritas acima
   - Substituir IDs de placeholder pelos valores reais
   - Lidar com erros e tentativas
   - Registrar progresso e resultados

4. **Verificar Resultados**
   - Confirmar que cada etapa foi concluída com sucesso
   - Verificar alterações no Google Workspace
   - Relatar status final e qualquer problema

## Dicas

- Use a flag `--dry-run` quando disponível para visualizar alterações
- Sempre inspecione schemas da API antes de chamar: `gws schema <service>.<resource>.<method>`
- Verifique a ajuda do comando para todas as flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Competência Original**: `recipe-create-vacation-responder`