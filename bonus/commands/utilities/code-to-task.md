# Converter Análise de Código em Tarefas Linear

Converter análise de código em tarefas Linear

## Propósito
Este comando varre sua base de código em busca de comentários TODO/FIXME, marcadores de débito técnico, código deprecated e outros indicadores que devem ser rastreados como tarefas. Ele cria automaticamente tarefas Linear organizadas e priorizadas para garantir que melhorias importantes no código não sejam esquecidas.

## Uso
```bash
# Varrer toda a base de código em busca de TODOs e criar tarefas
claude "Create tasks from all TODO comments in the codebase"

# Varrer diretório ou módulo específico
claude "Find TODOs in src/api and create Linear tasks"

# Criar tarefas a partir de padrões específicos
claude "Create tasks for all deprecated functions"

# Gerar relatório de débito técnico
claude "Analyze technical debt in the project and create improvement tasks"
```

## Instruções

### 1. Varrer Marcadores de Tarefas
Procure por padrões comuns indicando trabalho necessário:

```bash
# Encontrar comentários TODO
rg "TODO|FIXME|HACK|XXX|OPTIMIZE|REFACTOR" --type-add 'code:*.{js,ts,py,java,go,rb,php}' -t code

# Encontrar marcadores deprecated
rg "@deprecated|DEPRECATED|@obsolete" -t code

# Encontrar código temporário
rg "TEMPORARY|TEMP|REMOVE BEFORE|DELETE ME" -t code -i

# Encontrar marcadores de débito técnico
rg "TECHNICAL DEBT|TECH DEBT|REFACTOR|NEEDS REFACTORING" -t code -i

# Encontrar problemas de segurança
rg "SECURITY|INSECURE|VULNERABILITY|CVE-" -t code -i

# Encontrar problemas de desempenho
rg "SLOW|PERFORMANCE|OPTIMIZE|BOTTLENECK" -t code -i
```

### 2. Analisar Contexto do Comentário
Extraia informações significativas dos comentários:

```javascript
class CommentParser {
  parseComment(file, lineNumber, comment) {
    const parsed = {
      type: 'todo',
      priority: 'medium',
      title: '',
      description: '',
      author: null,
      date: null,
      tags: [],
      code_context: '',
      file_path: file,
      line_number: lineNumber
    };
    
    // Detectar tipo de comentário
    if (comment.match(/FIXME/i)) {
      parsed.type = 'fixme';
      parsed.priority = 'high';
    } else if (comment.match(/HACK|XXX/i)) {
      parsed.type = 'hack';
      parsed.priority = 'high';
    } else if (comment.match(/OPTIMIZE|PERFORMANCE/i)) {
      parsed.type = 'optimization';
    } else if (comment.match(/DEPRECATED/i)) {
      parsed.type = 'deprecation';
      parsed.priority = 'high';
    } else if (comment.match(/SECURITY/i)) {
      parsed.type = 'security';
      parsed.priority = 'urgent';
    }
    
    // Extrair autor e data
    const authorMatch = comment.match(/@(\w+)|by (\w+)/i);
    if (authorMatch) {
      parsed.author = authorMatch[1] || authorMatch[2];
    }
    
    const dateMatch = comment.match(/(\d{4}-\d{2}-\d{2})|(\d{1,2}\/\d{1,2}\/\d{2,4})/);
    if (dateMatch) {
      parsed.date = dateMatch[0];
    }
    
    // Extrair título e descrição
    const cleanComment = comment
      .replace(/^\/\/\s*|^\/\*\s*|\*\/\s*$|^#\s*/g, '')
      .replace(/TODO|FIXME|HACK|XXX/i, '')
      .trim();
    
    const parts = cleanComment.split(/[:\-–—]/);
    if (parts.length > 1) {
      parsed.title = parts[0].trim();
      parsed.description = parts.slice(1).join(':').trim();
    } else {
      parsed.title = cleanComment;
    }
    
    // Extrair tags
    const tagMatch = comment.match(/#(\w+)/g);
    if (tagMatch) {
      parsed.tags = tagMatch.map(tag => tag.substring(1));
    }
    
    return parsed;
  }
  
  getCodeContext(file, lineNumber, contextLines = 5) {
    const lines = readFileLines(file);
    const start = Math.max(0, lineNumber - contextLines);
    const end = Math.min(lines.length, lineNumber + contextLines);
    
    return lines.slice(start, end).map((line, i) => ({
      number: start + i + 1,
      content: line,
      isTarget: start + i + 1 === lineNumber
    }));
  }
}
```

### 3. Agrupar e Desduplicar
Organize problemas encontrados de forma inteligente:

```javascript
class TaskGrouper {
  groupTasks(parsedComments) {
    const groups = {
      byFile: new Map(),
      byType: new Map(),
      byAuthor: new Map(),
      byModule: new Map()
    };
    
    for (const comment of parsedComments) {
      // Agrupar por arquivo
      if (!groups.byFile.has(comment.file_path)) {
        groups.byFile.set(comment.file_path, []);
      }
      groups.byFile.get(comment.file_path).push(comment);
      
      // Agrupar por tipo
      if (!groups.byType.has(comment.type)) {
        groups.byType.set(comment.type, []);
      }
      groups.byType.get(comment.type).push(comment);
      
      // Agrupar por módulo
      const module = this.extractModule(comment.file_path);
      if (!groups.byModule.has(module)) {
        groups.byModule.set(module, []);
      }
      groups.byModule.get(module).push(comment);
    }
    
    return groups;
  }
  
  mergeSimilarTasks(tasks) {
    const merged = [];
    const seen = new Set();
    
    for (const task of tasks) {
      if (seen.has(task)) continue;
      
      // Encontrar tarefas similares
      const similar = tasks.filter(t => 
        t !== task &&
        !seen.has(t) &&
        this.areSimilar(task, t)
      );
      
      if (similar.length > 0) {
        // Mesclar em uma única tarefa
        const mergedTask = {
          ...task,
          title: this.generateMergedTitle(task, similar),
          description: this.generateMergedDescription(task, similar),
          locations: [task, ...similar].map(t => ({
            file: t.file_path,
            line: t.line_number
          }))
        };
        merged.push(mergedTask);
        seen.add(task);
        similar.forEach(t => seen.add(t));
      } else {
        merged.push(task);
        seen.add(task);
      }
    }
    
    return merged;
  }
}
```

### 4. Analisar Débito Técnico
Identifique problemas de qualidade do código:

```javascript
class TechnicalDebtAnalyzer {
  async analyzeFile(filePath) {
    const issues = [];
    const content = await readFile(filePath);
    const lines = content.split('\n');
    
    // Verificar funções longas
    const functionMatches = content.matchAll(/function\s+(\w+)|(\w+)\s*=\s*\(.*?\)\s*=>/g);
    for (const match of functionMatches) {
      const functionName = match[1] || match[2];
      const startLine = getLineNumber(content, match.index);
      const functionLength = this.getFunctionLength(lines, startLine);
      
      if (functionLength > 50) {
        issues.push({
          type: 'long_function',
          severity: functionLength > 100 ? 'high' : 'medium',
          title: `Refatorar função longa: ${functionName}`,
          description: `Função ${functionName} tem ${functionLength} linhas. Considere dividir em funções menores.`,
          file_path: filePath,
          line_number: startLine
        });
      }
    }
    
    // Verificar código duplicado
    const duplicates = await this.findDuplicateCode(filePath);
    for (const dup of duplicates) {
      issues.push({
        type: 'duplicate_code',
        severity: 'medium',
        title: 'Remover código duplicado',
        description: `Código similar encontrado em ${dup.otherFile}:${dup.otherLine}`,
        file_path: filePath,
        line_number: dup.line
      });
    }
    
    // Verificar condicionais complexas
    const complexConditions = content.matchAll(/if\s*\([^)]{50,}\)/g);
    for (const match of complexConditions) {
      issues.push({
        type: 'complex_condition',
        severity: 'low',
        title: 'Simplificar condicional complexa',
        description: 'Considere extrair lógica condicional em variáveis ou funções nomeadas',
        file_path: filePath,
        line_number: getLineNumber(content, match.index)
      });
    }
    
    // Verificar dependências desatualizadas
    if (filePath.endsWith('package.json')) {
      const outdated = await this.checkOutdatedDependencies(filePath);
      for (const dep of outdated) {
        issues.push({
          type: 'outdated_dependency',
          severity: dep.major ? 'high' : 'low',
          title: `Atualizar ${dep.name} de ${dep.current} para ${dep.latest}`,
          description: dep.major ? 'Atualização de versão major disponível' : 'Atualização minor disponível',
          file_path: filePath
        });
      }
    }
    
    return issues;
  }
}
```

### 5. Criar Tarefas Linear
Converta descobertas em tarefas acionáveis:

```javascript
async function createLinearTasks(groupedTasks, options = {}) {
  const created = [];
  const skipped = [];
  
  // Verificar tarefas existentes para evitar duplicatas
  const existingTasks = await linear.searchTasks('TODO OR FIXME');
  const existingTitles = new Set(existingTasks.map(t => t.title));
  
  // Criar tarefa pai para grupos grandes
  if (options.createEpic && groupedTasks.length > 10) {
    const epic = await linear.createTask({
      title: `Débito Técnico: Limpeza de ${options.module || 'Base de Código'}`,
      description: `Tarefa pai para ${groupedTasks.length} melhorias de código`,
      priority: 2,
      labels: ['technical-debt', 'code-quality']
    });
    options.parentId = epic.id;
  }
  
  for (const task of groupedTasks) {
    // Pular se tarefa similar existe
    if (existingTitles.has(task.title)) {
      skipped.push({ task, reason: 'duplicate' });
      continue;
    }
    
    // Construir descrição da tarefa
    const description = buildTaskDescription(task);
    
    // Mapear prioridade
    const priorityMap = {
      urgent: 1,
      high: 2,
      medium: 3,
      low: 4
    };
    
    try {
      const linearTask = await linear.createTask({
        title: task.title,
        description,
        priority: priorityMap[task.priority] || 3,
        labels: getLabelsForTask(task),
        parentId: options.parentId,
        estimate: estimateTaskSize(task)
      });
      
      created.push({
        linear: linearTask,
        source: task
      });
      
      // Adicionar link de código como comentário
      await linear.createComment({
        issueId: linearTask.id,
        body: `📍 Localização do código: \`${task.file_path}:${task.line_number}\``
      });
      
    } catch (error) {
      skipped.push({ task, reason: error.message });
    }
  }
  
  return { created, skipped };
}

function buildTaskDescription(task) {
  let description = task.description || '';
  
  // Adicionar contexto de código
  if (task.code_context) {
    description += '\n\n### Contexto do Código\n```\n';
    task.code_context.forEach(line => {
      const prefix = line.isTarget ? '>>> ' : '    ';
      description += `${prefix}${line.number}: ${line.content}\n`;
    });
    description += '```\n';
  }
  
  // Adicionar metadados
  description += '\n\n### Detalhes\n';
  description += `- **Tipo**: ${task.type}\n`;
  description += `- **Arquivo**: \`${task.file_path}\`\n`;
  description += `- **Linha**: ${task.line_number}\n`;
  
  if (task.author) {
    description += `- **Autor**: @${task.author}\n`;
  }
  if (task.date) {
    description += `- **Data**: ${task.date}\n`;
  }
  if (task.tags.length > 0) {
    description += `- **Tags**: ${task.tags.join(', ')}\n`;
  }
  
  // Adicionar sugestões
  if (task.type === 'deprecated') {
    description += '\n### Ações Sugeridas\n';
    description += '1. Identificar todos os usos deste código deprecated\n';
    description += '2. Atualizar para usar a alternativa recomendada\n';
    description += '3. Adicionar avisos de deprecação se não estiverem presentes\n';
    description += '4. Agendar para remoção na próxima versão major\n';
  }
  
  return description;
}
```

### 6. Gerar Relatório de Resumo
Crie uma visão geral das descobertas:

```javascript
function generateReport(scanResults, createdTasks) {
  const report = {
    summary: {
      totalFound: scanResults.length,
      tasksCreated: createdTasks.created.length,
      tasksSkipped: createdTasks.skipped.length,
      byType: {},
      byPriority: {},
      byFile: {}
    },
    details: [],
    recommendations: []
  };
  
  // Analisar distribuição
  for (const result of scanResults) {
    report.summary.byType[result.type] = (report.summary.byType[result.type] || 0) + 1;
    report.summary.byPriority[result.priority] = (report.summary.byPriority[result.priority] || 0) + 1;
  }
  
  // Gerar recomendações
  if (report.summary.byType.security > 0) {
    report.recommendations.push({
      priority: 'urgent',
      action: 'Resolver TODOs relacionados a segurança imediatamente',
      tasks: scanResults.filter(r => r.type === 'security').length
    });
  }
  
  if (report.summary.byType.deprecated > 5) {
    report.recommendations.push({
      priority: 'high',
      action: 'Criar sprint de remoção de deprecação',
      tasks: report.summary.byType.deprecated
    });
  }
  
  return report;
}
```

### 7. Tratamento de Erros
```javascript
// Tratar erros de acesso
try {
  await scanDirectory(path);
} catch (error) {
  if (error.code === 'EACCES') {
    console.warn(`Ignorando ${path} - permissão negada`);
  }
}

// Tratar limites da API Linear
const rateLimiter = {
  tasksCreated: 0,
  resetTime: Date.now() + 3600000,
  
  async createTask(taskData) {
    if (this.tasksCreated >= 50) {
      console.log('Limite de requisições se aproximando, agrupando tarefas restantes...');
      // Criar tarefa única com lista de TODOs
      return this.createBatchTask(remainingTasks);
    }
    this.tasksCreated++;
    return linear.createTask(taskData);
  }
};

// Tratar comentários malformados
const safeParser = {
  parse(comment) {
    try {
      return this.parseComment(comment);
    } catch (error) {
      return {
        type: 'todo',
        title: comment.substring(0, 50) + '...',
        priority: 'low',
        parseError: true
      };
    }
  }
};
```

## Exemplo de Saída

```
Varrendo base de código em busca de TODOs e débito técnico...

📊 Resultados da Varredura:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Encontrados 47 itens em 23 arquivos:
  • 24 TODOs
  • 8 FIXMEs 
  • 5 funções deprecated
  • 3 problemas de segurança
  • 7 otimizações de desempenho

🔍 Detalhamento por Prioridade:
  🔴 Urgente: 3 (relacionado a segurança)
  🟠 Alta: 13 (FIXMEs + deprecações)
  🟡 Média: 24 (TODOs padrão)
  🟢 Baixa: 7 (otimizações)

📁 Arquivos com Maior Volume:
  1. src/api/auth.js - 8 itens
  2. src/utils/validation.js - 6 itens
  3. src/models/User.js - 5 itens

🚨 Descobertas Críticas:

1. SEGURANÇA: Chave de API hardcoded
   Arquivo: src/config/api.js:45
   TODO: Remover chave hardcoded e usar variável env
   → Criando tarefa com prioridade URGENTE

2. DEPRECATED: Método de autenticação legado
   Arquivo: src/api/auth.js:120
   Múltiplos usos encontrados em 4 arquivos
   → Criando tarefa de migração

3. FIXME: Condição de corrida em atualizações concorrentes
   Arquivo: src/services/sync.js:78
   Autor: @alice (2024-01-03)
   → Criando tarefa de bug com alta prioridade

📝 Resumo de Criação de Tarefas:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

✅ Criadas 32 tarefas Linear:
   - Epic: "Limpeza de Débito Técnico Q1" (LIN-456)
   - 3 tarefas de segurança urgentes
   - 10 correções de alta prioridade
   - 19 melhorias de prioridade média

⏭️ 15 itens ignorados:
   - 8 duplicatas (tarefas já existem)
   - 4 comentários de baixo valor (ex: "TODO: pensar sobre isso")
   - 3 dependências externas (aguardando upstream)

📊 Estimativas:
   - Total de pontos de história: 89
   - Esforço estimado: 2-3 sprints
   - Tamanho de time recomendado: 2-3 desenvolvedores

🎯 Ações Recomendadas:
1. Agendar sprint de segurança imediatamente (3 itens urgentes)
2. Atribuir remoção de deprecação ao próximo sprint (5 itens)
3. Criar padrões de código para reduzir futuros TODOs
4. Configurar pre-commit hook para limitar novos TODOs

Visualize todas as tarefas criadas:
https://linear.app/yourteam/project/q1-technical-debt-cleanup
```

## Funcionalidades Avançadas

### Padrões Personalizados
Defina padrões específicos do projeto:
```bash
# Adicionar marcadores personalizados para varredura
claude "Scan for REVIEW, QUESTION, and ASSUMPTION comments"
```

### Integração com CI/CD
```bash
# Falhar build se TODOs críticos forem encontrados
claude "Check for SECURITY or FIXME comments and exit with error if found"
```

### Varreduras Agendadas
```bash
# Relatório de débito técnico semanal
claude "Generate weekly technical debt report and create tasks for new items"
```

## Dicas
- Execute regularmente para evitar acúmulo de TODOs
- Use formatos consistentes de comentários em toda a equipe
- Inclua autor e data nos TODOs
- Vincule TODOs a issues Linear existentes quando possível
- Configure snippets no IDE para TODOs formatados corretamente
- Revise e feche tarefas TODO concluídas
- Use comentários TODO como quality gate em revisões de pull request