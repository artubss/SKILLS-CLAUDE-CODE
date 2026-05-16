# E2B Claude Code Sandbox

Execute Claude Code em um ambiente sandbox isolado na nuvem E2B.

## Descrição

Este componente configura a integração E2B (E2B.dev) para executar Claude Code em um ambiente cloud seguro e isolado. Perfeito para executar código com segurança sem afetar seu sistema local.

## Funcionalidades

- **Execução Isolada**: Execute Claude Code em um sandbox cloud seguro
- **Ambiente Pré-configurado**: Já vem com Claude Code instalado
- **Integração de API**: Conexão contínua com a API Claude do Anthropic
- **Execução Segura de Código**: Execute prompts sem riscos para o sistema local
- **Instalação de Componentes**: Instala automaticamente qualquer componente especificado com flags CLI

## Requisitos

- Chave de API E2B (obtenha em https://e2b.dev/dashboard)
- Chave de API Anthropic
- Python 3.11+ (para SDK E2B)

## Uso

```bash
# Execute um prompt no sandbox E2B (requer chaves de API como variáveis de ambiente ou parâmetros CLI)
npx claude-code-templates@latest --sandbox e2b --prompt "Create a React todo app"

# Passe as chaves de API diretamente como parâmetros
npx claude-code-templates@latest --sandbox e2b \
  --e2b-api-key your_e2b_key \
  --anthropic-api-key your_anthropic_key \
  --prompt "Create a React todo app"

# Instale componentes e execute no sandbox
npx claude-code-templates@latest --sandbox e2b \
  --agent frontend-developer \
  --command setup-react \
  --e2b-api-key your_e2b_key \
  --anthropic-api-key your_anthropic_key \
  --prompt "Create a modern todo app with TypeScript"
```

## Configuração do Ambiente

O componente criará:
- `.claude/sandbox/e2b-launcher.py` - Script Python para lançar sandbox E2B
- `.claude/sandbox/requirements.txt` - Dependências Python  
- `.claude/sandbox/.env.example` - Template de variáveis de ambiente

## Configuração de Chave de API

Você pode fornecer chaves de API de duas formas:

### Opção 1: Parâmetros CLI (Recomendado)
```bash
# Passe as chaves diretamente como parâmetros de comando
npx claude-code-templates@latest --sandbox e2b \
  --e2b-api-key your_e2b_api_key \
  --anthropic-api-key your_anthropic_api_key \
  --prompt "Your prompt here"
```

### Opção 2: Variáveis de Ambiente
Defina essas variáveis de ambiente em seu shell ou arquivo `.env`:
```bash
export E2B_API_KEY=your_e2b_api_key_here
export ANTHROPIC_API_KEY=your_anthropic_api_key_here

# Ou crie arquivo .claude/sandbox/.env:
E2B_API_KEY=your_e2b_api_key_here
ANTHROPIC_API_KEY=your_anthropic_api_key_here
```

**Nota**: Parâmetros CLI têm precedência sobre variáveis de ambiente.

## Como Funciona

1. Cria sandbox E2B com template `anthropic-claude-code`
2. Instala qualquer componente especificado (agentes, comandos, etc.)
3. Executa seu prompt usando Claude Code dentro do sandbox
4. Retorna a saída completa e qualquer arquivo gerado
5. Limpa automaticamente o sandbox após a execução

## Benefícios de Segurança

- **Isolamento**: O código é executado em um ambiente cloud separado
- **Sem Impacto Local**: Sem risco para seu sistema local ou arquivos
- **Temporário**: O sandbox é destruído após a execução
- **Controlado**: Apenas componentes e prompts especificados são executados

## Exemplos

```bash
# Criação simples de aplicação web
npx claude-code-templates@latest --sandbox e2b --prompt "Create an HTML page with CSS animations"

# Desenvolvimento full stack
npx claude-code-templates@latest --sandbox e2b --agent fullstack-developer --prompt "Create a Node.js API with authentication"

# Análise de dados
npx claude-code-templates@latest --sandbox e2b --agent data-scientist --prompt "Analyze this CSV data and create visualizations"
```

## Informações do Template

- **Provedor**: E2B (https://e2b.dev)
- **Template Base**: anthropic-claude-code
- **Timeout**: 5 minutos (configurável)
- **Ambiente**: Ubuntu com Claude Code pré-instalado