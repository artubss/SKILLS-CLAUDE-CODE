---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [nome-epic] | --name | --description | --owner
description: Criar novo epic PAC seguindo a especificação Product as Code
---

# Criar Epic PAC

Criar um novo epic seguindo a especificação Product as Code com workflow guiado: **$ARGUMENTS**

## Verificação de Configuração PAC

- Diretório PAC: !`ls -la .pac/ 2>/dev/null || echo "Nenhum diretório .pac encontrado"`
- Config PAC: @.pac/pac.config.yaml (se existir)
- Epics existentes: !`ls -la .pac/epics/ 2>/dev/null | head -10`

## Tarefa

Criar um novo epic Product as Code:

**Argumentos**: 
- Nome do epic (obrigatório se não usar a flag --name)
- --name <name>: Nome do epic
- --description <desc>: Descrição do epic  
- --owner <owner>: Proprietário do epic
- --scope <scope>: Definição de escopo

**Processo de Criação do Epic**:
1. Validar se a configuração PAC existe (sugerir `/project:pac-configure` se ausente)
2. Gerar ID do epic a partir do nome (formato: epic-[nome-em-kebab-case])
3. Criar arquivo YAML do epic seguindo a especificação PAC v0.1.0 em `.pac/epics/[epic-id].yaml`
4. Incluir metadados obrigatórios: id, name, timestamp de criação, owner
5. Adicionar spec com descrição, escopo, critérios de sucesso, restrições, dependências
6. Criar estrutura de diretório do epic: `.pac/epics/[epic-id]/`
7. Atualizar índice PAC se `.pac/index.yaml` existir
8. Criar branch git `pac/[epic-id]` se em repositório git

Se informações estiverem faltando, solicitar detalhes do epic interativamente ao usuário.

**Próximos Passos**: Use `/project:pac-create-ticket --epic [epic-id]` para adicionar tickets a este epic.