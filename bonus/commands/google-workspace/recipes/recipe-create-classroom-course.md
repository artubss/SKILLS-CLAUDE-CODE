---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [task-parameters]
description: Criar um curso no Google Classroom e convidar alunos.
---

# Criar Curso no Classroom

Execute o workflow do Google Workspace: $ARGUMENTS

# Criar um Curso no Google Classroom

> **PRÉ-REQUISITO:** Carregue as seguintes skills para executar esta receita: `gws-classroom`

Crie um curso no Google Classroom e convide alunos.

## Etapas

1. Criar o curso: `gws classroom courses create --json '{"name": "Introduction to CS", "section": "Period 1", "room": "Room 101", "ownerId": "me"}'`
2. Convidar um aluno: `gws classroom invitations create --json '{"courseId": "COURSE_ID", "userId": "student@school.edu", "role": "STUDENT"}'`
3. Listar alunos inscritos: `gws classroom courses students list --params '{"courseId": "COURSE_ID"}' --format table`

## Tarefa

Execute este workflow com os seguintes parâmetros: $ARGUMENTS

1. **Verificação de Pré-requisitos**
   - Verifique se o CLI `gws` está instalado: `gws --version`
   - Confirme a autenticação: `gws auth status`
   - Carregue as skills do GWS necessárias (verifique a seção PRÉ-REQUISITO acima)

2. **Preparação de Parâmetros**
   - Analise os parâmetros da tarefa de $ARGUMENTS
   - Valide as entradas obrigatórias
   - Prepare os payloads JSON e flags

3. **Executar Etapas do Workflow**
   - Siga as etapas descritas acima
   - Substitua os IDs placeholder pelos valores reais
   - Trate erros e reexecuções
   - Registre progresso e resultados

4. **Verificar Resultados**
   - Confirme que cada etapa foi concluída com sucesso
   - Verifique as mudanças no Google Workspace
   - Relate o status final e quaisquer problemas

## Dicas

- Use a flag `--dry-run` quando disponível para visualizar mudanças
- Sempre inspecione schemas da API antes de chamar: `gws schema <service>.<resource>.<method>`
- Verifique a ajuda de comando para todas as flags: `gws <service> <resource> <method> --help`

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `recipe-create-classroom-course`