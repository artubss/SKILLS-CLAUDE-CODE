---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [--pull] [--push] [--validate] [--backup]
description: Sincronizar variáveis de ambiente entre desenvolvimento local e deployments Vercel
---

## Sincronização de Ambiente Vercel

**Operação de Sincronização**: $ARGUMENTS

## Análise de Ambiente Atual

### Ambiente Local
- Arquivos de ambiente: 
  - @.env.local (se existir)
  - @.env.development (se existir)
  - @.env.production (se existir)
  - @.env (se existir)
- Exemplo de ambiente: @.env.example (se existir)
- Config Vercel: @vercel.json (se existir)

### Status do Projeto
- Status Vercel CLI: !`vercel --version 2>/dev/null || echo "Vercel CLI não instalado"`
- Projeto atual: !`vercel project ls 2>/dev/null | head -5 || echo "Não vinculado a projeto Vercel"`
- Status Git: !`git status --porcelain | head -5`

## Estratégia de Sincronização de Ambiente

### 1. Análise de Arquivo de Ambiente
```typescript
// Análise de estrutura de arquivo de ambiente
interface EnvironmentConfig {
  development: Record<string, string>;
  preview: Record<string, string>;
  production: Record<string, string>;
}

const environmentFiles = {
  '.env.local': 'Sobreposições de desenvolvimento local',
  '.env.development': 'Ambiente de desenvolvimento',
  '.env.staging': 'Ambiente de staging/preview', 
  '.env.production': 'Ambiente de produção',
  '.env': 'Ambiente padrão (commitado no git)',
  '.env.example': 'Template de ambiente (seguro para commitar)',
};
```

### 2. Gerenciamento de Ambiente Vercel
```bash
# Listar todas as variáveis de ambiente para todos os ambientes
vercel env ls

# Listar variáveis de ambiente para ambiente específico
vercel env ls --environment=production
vercel env ls --environment=preview
vercel env ls --environment=development

# Puxar variáveis de ambiente da Vercel
vercel env pull .env.vercel

# Adicionar nova variável de ambiente
vercel env add [name] [environment]

# Remover variável de ambiente
vercel env rm [name] [environment]
```

## Operações de Sincronização

### 1. Puxar Variáveis de Ambiente da Vercel
```bash
#!/bin/bash
# Puxar ambientes da Vercel

echo "🔄 Puxando variáveis de ambiente da Vercel..."

# Criar backup de arquivos existentes
if [ -f .env.local ]; then
  cp .env.local .env.local.backup.$(date +%Y%m%d_%H%M%S)
  echo "📦 Backup criado para .env.local"
fi

# Puxar da Vercel (cria .env.local por padrão)
vercel env pull .env.local

if [ $? -eq 0 ]; then
  echo "✅ Variáveis de ambiente puxadas com sucesso"
  echo "📁 Variáveis salvas em .env.local"
  
  # Mostrar resumo
  echo ""
  echo "📊 Resumo de Variáveis de Ambiente:"
  echo "================================"
  grep -c "=" .env.local 2>/dev/null && echo "Total de variáveis: $(grep -c "=" .env.local)"
  
  # Listar nomes de variáveis (esconder valores por segurança)
  echo ""
  echo "🔑 Nomes de Variáveis:"
  grep "^[A-Z]" .env.local | cut -d'=' -f1 | sort
else
  echo "❌ Falha ao puxar variáveis de ambiente"
  exit 1
fi
```

### 2. Enviar Variáveis de Ambiente para Vercel
```bash
#!/bin/bash
# Enviar variáveis de ambiente para Vercel

echo "🚀 Enviando variáveis de ambiente para Vercel..."

# Verificar se arquivos de ambiente existem
ENV_FILES=(".env.production" ".env.staging" ".env.development")
FOUND_FILES=()

for file in "${ENV_FILES[@]}"; do
  if [ -f "$file" ]; then
    FOUND_FILES+=("$file")
  fi
done

if [ ${#FOUND_FILES[@]} -eq 0 ]; then
  echo "❌ Nenhum arquivo de ambiente encontrado para enviar"
  echo "💡 Arquivos esperados: ${ENV_FILES[*]}"
  exit 1
fi

# Enviar cada arquivo de ambiente
for file in "${FOUND_FILES[@]}"; do
  echo "📤 Processando $file..."
  
  # Determinar ambiente alvo
  if [[ "$file" == *"production"* ]]; then
    ENV="production"
  elif [[ "$file" == *"staging"* ]]; then
    ENV="preview"  # Vercel usa 'preview' para staging
  elif [[ "$file" == *"development"* ]]; then
    ENV="development"
  else
    ENV="development"  # Padrão
  fi
  
  echo "🎯 Enviando para ambiente $ENV..."
  
  # Ler variáveis do arquivo e enviar para Vercel
  while IFS='=' read -r key value; do
    # Pular linhas vazias e comentários
    if [[ -z "$key" || "$key" =~ ^#.* ]]; then
      continue
    fi
    
    # Remover aspas do valor se presentes
    value=$(echo "$value" | sed 's/^"\(.*\)"$/\1/' | sed "s/^'\(.*\)'$/\1/")
    
    echo "  🔑 Definindo $key..."
    echo "$value" | vercel env add "$key" "$ENV" --force
    
  done < "$file"
  
  echo "✅ Completado $file -> $ENV"
  echo ""
done

echo "🎉 Todas as variáveis de ambiente foram enviadas com sucesso!"
```

### 3. Validação de Ambiente
```typescript
// Script de validação de ambiente
interface ValidationRule {
  name: string;
  required: boolean;
  pattern?: RegExp;
  description: string;
}

const validationRules: ValidationRule[] = [
  {
    name: 'DATABASE_URL',
    required: true,
    pattern: /^(postgresql|mysql|sqlite):\/\/.+/,
    description: 'String de conexão do banco de dados',
  },
  {
    name: 'NEXTAUTH_SECRET',
    required: true,
    pattern: /.{32,}/,
    description: 'Chave secreta NextAuth.js (mínimo 32 caracteres)',
  },
  {
    name: 'NEXTAUTH_URL',
    required: true,
    pattern: /^https?:\/\/.+/,
    description: 'URL canônica NextAuth.js',
  },
  {
    name: 'API_KEY',
    required: false,
    pattern: /^[A-Za-z0-9_-]+$/,
    description: 'Chave de API para serviços externos',
  },
];

function validateEnvironment(envFile: string): ValidationResult {
  const errors: string[] = [];
  const warnings: string[] = [];
  const env = readEnvironmentFile(envFile);
  
  // Verificar variáveis obrigatórias
  validationRules.forEach(rule => {
    const value = env[rule.name];
    
    if (rule.required && !value) {
      errors.push(`Variável obrigatória ausente: ${rule.name}`);
      return;
    }
    
    if (value && rule.pattern && !rule.pattern.test(value)) {
      errors.push(`Formato inválido para ${rule.name}: ${rule.description}`);
    }
  });
  
  // Verificar problemas comuns
  Object.entries(env).forEach(([key, value]) => {
    // Verificar valores de placeholder
    if (value === 'your-secret-here' || value === 'change-me') {
      warnings.push(`Valor de placeholder detectado para ${key}`);
    }
    
    // Verificar segredos potencialmente commitados
    if (key.includes('SECRET') || key.includes('PRIVATE')) {
      if (value.length < 16) {
        warnings.push(`${key} parece ser muito curto para um segredo`);
      }
    }
  });
  
  return {
    valid: errors.length === 0,
    errors,
    warnings,
  };
}

function readEnvironmentFile(filePath: string): Record<string, string> {
  // Implementação para ler e fazer parse do arquivo de ambiente
  return {};
}

interface ValidationResult {
  valid: boolean;
  errors: string[];
  warnings: string[];
}
```

### 4. Backup e Restauração de Ambiente
```bash
#!/bin/bash
# Fazer backup e restaurar variáveis de ambiente

BACKUP_DIR=".env-backups"
TIMESTAMP=$(date +%Y%m%d_%H%M%S)

backup_environment() {
  echo "📦 Criando backup de ambiente..."
  
  mkdir -p "$BACKUP_DIR"
  
  # Fazer backup de arquivos locais
  for file in .env.local .env.development .env.staging .env.production; do
    if [ -f "$file" ]; then
      cp "$file" "$BACKUP_DIR/${file}.${TIMESTAMP}"
      echo "✅ Backup feito para $file"
    fi
  done
  
  # Fazer backup de variáveis de ambiente Vercel
  echo "📤 Fazendo backup de variáveis de ambiente Vercel..."
  
  for env in production preview development; do
    vercel env ls --environment="$env" > "$BACKUP_DIR/vercel-${env}.${TIMESTAMP}.txt"
    echo "✅ Backup feito para ambiente Vercel $env"
  done
  
  echo "🎉 Backup completado em $BACKUP_DIR/"
  ls -la "$BACKUP_DIR/" | grep "$TIMESTAMP"
}

restore_environment() {
  local backup_timestamp="$1"
  
  if [ -z "$backup_timestamp" ]; then
    echo "❌ Por favor, especifique o timestamp do backup"
    echo "💡 Backups disponíveis:"
    ls -1 "$BACKUP_DIR/" | grep -E "\.env" | cut -d'.' -f3 | sort -u
    exit 1
  fi
  
  echo "🔄 Restaurando ambiente a partir do backup $backup_timestamp..."
  
  # Restaurar arquivos locais
  for file in .env.local .env.development .env.staging .env.production; do
    backup_file="$BACKUP_DIR/${file}.${backup_timestamp}"
    if [ -f "$backup_file" ]; then
      cp "$backup_file" "$file"
      echo "✅ $file restaurado"
    fi
  done
  
  echo "🎉 Ambiente restaurado a partir do backup"
}

# Funções de uso
case "$1" in
  backup)
    backup_environment
    ;;
  restore)
    restore_environment "$2"
    ;;
  *)
    echo "Uso: $0 {backup|restore} [timestamp]"
    exit 1
    ;;
esac
```

## Recursos Avançados de Sincronização

### 1. Comparação e Diff de Ambiente
```typescript
// Ferramenta de comparação de ambiente
interface EnvironmentDiff {
  added: string[];
  removed: string[];
  modified: Array<{
    key: string;
    local: string;
    remote: string;
  }>;
  unchanged: string[];
}

function compareEnvironments(
  local: Record<string, string>,
  remote: Record<string, string>
): EnvironmentDiff {
  const diff: EnvironmentDiff = {
    added: [],
    removed: [],
    modified: [],
    unchanged: [],
  };
  
  const allKeys = new Set([...Object.keys(local), ...Object.keys(remote)]);
  
  allKeys.forEach(key => {
    if (!(key in local)) {
      diff.added.push(key);
    } else if (!(key in remote)) {
      diff.removed.push(key);
    } else if (local[key] !== remote[key]) {
      diff.modified.push({
        key,
        local: local[key],
        remote: remote[key],
      });
    } else {
      diff.unchanged.push(key);
    }
  });
  
  return diff;
}

// Gerar relatório de diff
function generateDiffReport(diff: EnvironmentDiff): string {
  let report = '# Comparação de Variáveis de Ambiente\n\n';
  
  if (diff.added.length > 0) {
    report += '## ➕ Variáveis no Remote (não no Local)\n';
    diff.added.forEach(key => {
      report += `- \`${key}\`\n`;
    });
    report += '\n';
  }
  
  if (diff.removed.length > 0) {
    report += '## ➖ Variáveis no Local (não no Remote)\n';
    diff.removed.forEach(key => {
      report += `- \`${key}\`\n`;
    });
    report += '\n';
  }
  
  if (diff.modified.length > 0) {
    report += '## 🔄 Variáveis Modificadas\n';
    diff.modified.forEach(({ key, local, remote }) => {
      report += `### \`${key}\`\n`;
      report += `- **Local**: \`${maskSensitive(local)}\`\n`;
      report += `- **Remote**: \`${maskSensitive(remote)}\`\n\n`;
    });
  }
  
  if (diff.unchanged.length > 0) {
    report += `## ✅ Variáveis Inalteradas (${diff.unchanged.length})\n`;
    report += `${diff.unchanged.map(key => `- \`${key}\``).join('\n')}\n\n`;
  }
  
  return report;
}

function maskSensitive(value: string): string {
  // Mascarar valores sensíveis para segurança
  if (value.length <= 8) {
    return '*'.repeat(value.length);
  }
  return `${value.substring(0, 4)}${'*'.repeat(value.length - 8)}${value.substring(value.length - 4)}`;
}
```

### 2. Geração de Template de Ambiente
```typescript
// Gerar .env.example a partir do ambiente existente
function generateEnvExample(envFile: string): string {
  const env = readEnvironmentFile(envFile);
  let template = '# Template de Variáveis de Ambiente\n';
  template += '# Copie este arquivo para .env.local e preencha os valores\n\n';
  
  const categories = categorizeVariables(env);
  
  Object.entries(categories).forEach(([category, variables]) => {
    template += `# ${category.toUpperCase()}\n`;
    variables.forEach(({ key, description, example }) => {
      if (description) {
        template += `# ${description}\n`;
      }
      template += `${key}=${example || 'seu-valor-aqui'}\n\n`;
    });
  });
  
  return template;
}

function categorizeVariables(env: Record<string, string>) {
  const categories: Record<string, Array<{
    key: string;
    description?: string;
    example?: string;
  }>> = {
    database: [],
    authentication: [],
    external_apis: [],
    configuration: [],
  };
  
  Object.keys(env).forEach(key => {
    if (key.includes('DATABASE') || key.includes('DB_')) {
      categories.database.push({ key, description: getDatabaseDescription(key) });
    } else if (key.includes('AUTH') || key.includes('SECRET')) {
      categories.authentication.push({ key, description: getAuthDescription(key) });
    } else if (key.includes('API_KEY') || key.includes('_TOKEN')) {
      categories.external_apis.push({ key, description: getApiDescription(key) });
    } else {
      categories.configuration.push({ key, description: getConfigDescription(key) });
    }
  });
  
  return categories;
}

function getDatabaseDescription(key: string): string {
  if (key === 'DATABASE_URL') return 'String de conexão do banco de dados';
  if (key === 'DB_HOST') return 'Host do banco de dados';
  if (key === 'DB_PORT') return 'Porta do banco de dados';
  if (key === 'DB_NAME') return 'Nome do banco de dados';
  return 'Configuração de banco de dados';
}

function getAuthDescription(key: string): string {
  if (key === 'NEXTAUTH_SECRET') return 'Chave secreta NextAuth.js';
  if (key === 'NEXTAUTH_URL') return 'URL canônica NextAuth.js';
  if (key === 'JWT_SECRET') return 'Chave secreta JWT';
  return 'Configuração de autenticação';
}

function getApiDescription(key: string): string {
  return `Chave de API para ${key.toLowerCase().replace(/_/g, ' ')}`;
}

function getConfigDescription(key: string): string {
  return `Configuração para ${key.toLowerCase().replace(/_/g, ' ')}`;
}
```

### 3. Segurança e Validação
```bash
#!/bin/bash
# Verificações de segurança para variáveis de ambiente

security_check() {
  echo "🔐 Executando verificações de segurança em variáveis de ambiente..."
  
  local issues=0
  
  # Verificar problemas comuns de segurança
  for file in .env.local .env.development .env.staging .env.production; do
    if [ ! -f "$file" ]; then
      continue
    fi
    
    echo "🔍 Verificando $file..."
    
    # Verificar segredos fracos
    while IFS='=' read -r key value; do
      if [[ -z "$key" || "$key" =~ ^#.* ]]; then
        continue
      fi
      
      # Remover aspas
      value=$(echo "$value" | sed 's/^"\(.*\)"$/\1/' | sed "s/^'\(.*\)'$/\1/")
      
      # Verificar valores de placeholder
      if [[ "$value" == *"your-"* || "$value" == *"change-me"* || "$value" == *"replace-me"* ]]; then
        echo "⚠️  Valor de placeholder em $key"
        ((issues++))
      fi
      
      # Verificar segredos curtos
      if [[ "$key" =~ (SECRET|PRIVATE|KEY|TOKEN) ]]; then
        if [ ${#value} -lt 16 ]; then
          echo "⚠️  $key parece ser muito curto para um segredo (${#value} caracteres)"
          ((issues++))
        fi
      fi
      
      # Verificar URLs hardcoded em produção
      if [[ "$file" == *"production"* && "$value" =~ localhost ]]; then
        echo "⚠️  $key contém localhost em ambiente de produção"
        ((issues++))
      fi
      
    done < "$file"
  done
  
  # Verificar se arquivos .env estão em .gitignore
  if [ -f .gitignore ]; then
    if ! grep -q ".env.local" .gitignore; then
      echo "⚠️  .env.local não está em .gitignore"
      ((issues++))
    fi
    if ! grep -q ".env.production" .gitignore; then
      echo "⚠️  .env.production não está em .gitignore"
      ((issues++))
    fi
  else
    echo "⚠️  Nenhum arquivo .gitignore encontrado"
    ((issues++))
  fi
  
  echo ""
  if [ $issues -eq 0 ]; then
    echo "✅ Nenhum problema de segurança encontrado"
  else
    echo "❌ Encontrados $issues problemas de segurança"
    exit 1
  fi
}

security_check
```

## Automação e Integração

### 1. Integração GitHub Actions
```yaml
# .github/workflows/env-sync.yml
name: Environment Sync

on:
  push:
    branches: [main, develop]
    paths: ['.env.example', '.env.*']
  
  workflow_dispatch:
    inputs:
      action:
        description: 'Ação de sincronização'
        required: true
        default: 'validate'
        type: choice
        options:
        - validate
        - pull
        - push

jobs:
  env-sync:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Instalar Vercel CLI
        run: npm i -g vercel@latest
      
      - name: Vincular a Vercel
        run: vercel link --token=${{ secrets.VERCEL_TOKEN }} --yes
      
      - name: Validar Ambiente
        if: github.event.inputs.action == 'validate' || github.event.inputs.action == ''
        run: |
          # Executar validação de ambiente
          node scripts/validate-env.js
      
      - name: Puxar Ambiente
        if: github.event.inputs.action == 'pull'
        run: |
          vercel env pull .env.ci --token=${{ secrets.VERCEL_TOKEN }}
          # Validar ambiente puxado
          node scripts/validate-env.js .env.ci
      
      - name: Enviar Ambiente
        if: github.event.inputs.action == 'push'
        run: |
          # Enviar variáveis de ambiente para Vercel
          node scripts/push-env.js --token=${{ secrets.VERCEL_TOKEN }}
```

### 2. Integração de Fluxo de Desenvolvimento
```bash
#!/bin/bash
# Hook pre-commit para validação de ambiente

echo "🔍 Validando variáveis de ambiente..."

# Verificar se arquivos de ambiente estão propriamente configurados
if [ -f .env.local ]; then
  echo "✅ .env.local existe"
else
  echo "⚠️  .env.local não encontrado"
  if [ -f .env.example ]; then
    echo "💡 Copie .env.example para .env.local e configure suas variáveis"
  fi
fi

# Validar estrutura de ambiente
node scripts/validate-env.js

if [ $? -ne 0 ]; then
  echo "❌ Validação de ambiente falhou"
  exit 1
fi

echo "✅ Validação de ambiente passou"
```

## Operações Completas de Sincronização

Implementar workflow completo de sincronização de ambiente com validação, backup, verificações de segurança e integração de automação para processos perfeitos de desenvolvimento e deployment.

<function_calls>
<invoke name="TodoWrite">
<parameter name="todos">[{"content": "Analyze hook system and context monitoring capabilities", "status": "completed"}, {"content": "Design context threshold detection mechanism", "status": "completed"}, {"content": "Create hook script for automatic compact + clear sequence", "status": "pending"}, {"content": "Configure hook in settings.json with proper event triggers", "status": "pending"}, {"content": "Test hook execution and threshold detection", "status": "pending"}, {"content": "Add markdown copy button to Supabase blog post", "status": "completed"}, {"content": "Write Claude Code + Next.js and Vercel Integration article", "status": "completed"}, {"content": "Create Next.js Architecture Expert agent", "status": "completed"}, {"content": "Create Vercel Deployment Specialist agent", "status": "completed"}, {"content": "Create React Performance Optimizer agent", "status": "completed"}, {"content": "Create Next.js app scaffolding command", "status": "completed"}, {"content": "Create Vercel deployment optimization command", "status": "completed"}, {"content": "Create component generator command", "status": "completed"}, {"content": "Create API route tester command", "status": "completed"}, {"content": "Create bundle analyzer command", "status": "completed"}, {"content": "Create middleware creator command", "status": "completed"}, {"content": "Create edge function generator command", "status": "completed"}, {"content": "Create performance audit command", "status": "completed"}, {"content": "Create environment sync command", "status": "completed"}, {"content": "Create migration helper command", "status": "in_progress"}]
</parameter>
</invoke>
</function_calls>