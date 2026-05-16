---
allowed-tools: Read, Edit, Bash
argument-hint: [nome-do-workflow]
description: Executar GitHub Actions localmente usando act
---

# Act - Execução Local de GitHub Actions

Execute workflows de GitHub Actions localmente usando act: $ARGUMENTS

## Workflows Atuais

- Workflows disponíveis: !`find .github/workflows -name "*.yml" -o -name "*.yaml" | head -10`
- Configuração Act: @.actrc (se existir)
- Status Docker: !`docker --version`

## Tarefa

Execute workflow de GitHub Actions localmente:

1. **Verificação de Configuração**
   - Certifique-se de que act está instalado: `act --version`
   - Verifique se Docker está em execução
   - Verifique workflows disponíveis em `.github/workflows/`

2. **Seleção de Workflow**
   - Se workflow especificado: Execute workflow específico `$ARGUMENTS`
   - Se nenhum workflow: Liste todos os workflows disponíveis
   - Verifique triggers e eventos do workflow

3. **Execução Local**
   - Execute workflow com flags apropriadas
   - Use secrets de `.env` ou `.secrets`
   - Trate runners específicos da plataforma
   - Monitore execução e logs

4. **Suporte a Debugging**
   - Use `--verbose` para saída detalhada
   - Use `--dry-run` para testar
   - Use `--list` para mostrar actions disponíveis

## Exemplos de Comandos

```bash
# Listar todos os workflows
act --list

# Executar workflow específico
act workflow_dispatch -W .github/workflows/$ARGUMENTS.yml

# Executar com secrets
act --secret-file .env

# Modo debug
act --verbose --dry-run
```