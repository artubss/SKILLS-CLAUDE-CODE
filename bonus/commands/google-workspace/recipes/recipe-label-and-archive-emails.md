---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Aplique rótulos do Gmail a mensagens correspondentes e arquive-as para manter sua caixa de entrada limpa.
---

# Rotular e Arquivar E-mails

Execute workflow do Google Workspace: $ARGUMENTS

# Rotular e Arquivar Conversas do Gmail

> **PRÉ-REQUISITO:** Carregue as seguintes habilidades para executar esta receita: `gws-gmail`

Aplique rótulos do Gmail a mensagens correspondentes e arquive-as para manter sua caixa de entrada limpa.

## Etapas

1. Pesquise e-mails correspondentes: `gws gmail users messages list --params '{"userId": "me", "q": "from:notifications@service.com"}' --format table`
2. Aplique um rótulo: `gws gmail users messages modify --params '{"userId": "me", "id": "MESSAGE_ID"}' --json '{"addLabelIds": ["LABEL_ID"]}'`
3. Arquive (remova da caixa de entrada): `gws gmail users messages modify --params '{"userId": "me", "id": "MESSAGE_ID"}' --json '{"removeLabelIds": ["INBOX"]}'`

## Tarefa

Execute este workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-requisitos**
   - Verifique se a CLI `gws` está instalada: `gws --version`
   - Confirme a autenticação: `gws auth status`
   - Carregue as habilidades GWS necessárias (verifique a seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Analise os parâmetros da tarefa de $ARGUMENTS
   - Valide as entradas obrigatórias
   - Prepare os payloads JSON e flags

3. **Execute as Etapas do Workflow**
   - Siga as etapas descritas acima
   - Substitua os IDs de placeholder pelos valores reais
   - Trate erros e repetições
   - Registre progresso e resultados

4. **Verifique os Resultados**
   - Confirme que cada etapa foi concluída com sucesso
   - Verifique as alterações no Google Workspace
   - Informe o status final e qualquer problema

## Dicas

- Use a flag `--dry-run` quando disponível para visualizar as alterações
- Sempre inspecione os schemas da API antes de chamar: `gws schema <service>.<resource>.<method>`
- Verifique a ajuda de comandos para todos os flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Habilidade Original**: `recipe-label-and-archive-emails`