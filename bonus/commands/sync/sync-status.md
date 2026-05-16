---
allowed-tools: Read, Bash
argument-hint: [--detailed] | [--health-check] | [--diagnostics]
description: Monitorar status de integridade da sincronização GitHub-Linear com métricas de desempenho e diagnósticos
---

# Monitor de Status de Sincronização

Monitorar integridade da sincronização GitHub-Linear: $ARGUMENTS

## Estado de Sincronização Atual

- Configuração de sincronização: @.sync-config.json ou @sync/ (se existir)
- Logs de sincronização recentes: !`find . -name "*sync*.log" | head -3`
- Status do GitHub: !`gh api rate_limit` (se GitHub CLI disponível)
- Status do processo: !`ps aux | grep -i sync | head -3`

## Tarefa

Analisar status de sincronização entre GitHub e Linear. Ao verificar o status de sincronização:

1. **Visão Geral do Estado de Sincronização**
   ```javascript
   async function getSyncOverview() {
     const state = await loadSyncState();
     
     return {
       lastFullSync: state.lastFullSync,
       lastIncrementalSync: state.lastIncremental,
       totalSyncedItems: Object.keys(state.entities).length,
       pendingSync: state.queue.length,
       failedSync: state.failures.length,
       syncEnabled: state.config.enabled,
       syncDirection: state.config.direction,
       webhooksActive: await checkWebhooks()
     };
   }
   ```

2. **Métricas de Integridade**
   ```javascript
   const healthMetrics = {
     // Métricas de desempenho
     avgSyncTime: calculateAverage(syncTimes),
     maxSyncTime: Math.max(...syncTimes),
     syncSuccessRate: (successful / total) * 100,
     
     // Métricas de qualidade dos dados
     conflictRate: (conflicts / syncs) * 100,
     duplicateRate: (duplicates / total) * 100,
     orphanedItems: countOrphaned(),
     
     // Integridade da API
     githubRateLimit: await getGitHubRateLimit(),
     linearRateLimit: await getLinearRateLimit(),
     apiErrors: recentErrors.length,
     
     // Latência de sincronização
     avgSyncLag: calculateSyncLag(),
     maxSyncLag: findMaxLag(),
     itemsOutOfSync: findOutOfSync().length
   };
   ```

3. **Verificações de Consistência**
   ```javascript
   async function checkConsistency() {
     const issues = [];
     
     // Verificar GitHub → Linear
     const githubIssues = await fetchAllGitHubIssues();
     for (const issue of githubIssues) {
       const linearTask = await findLinearTask(issue);
       if (!linearTask) {
         issues.push({
           type: 'MISSING_IN_LINEAR',
           github: issue.number,
           severity: 'high'
         });
       } else {
         const diffs = compareFields(issue, linearTask);
         if (diffs.length > 0) {
           issues.push({
             type: 'FIELD_MISMATCH',
             github: issue.number,
             linear: linearTask.identifier,
             differences: diffs,
             severity: 'medium'
           });
         }
       }
     }
     
     return issues;
   }
   ```

4. **Análise do Histórico de Sincronização**
   ```javascript
   function analyzeSyncHistory(days = 7) {
     const history = loadSyncHistory(days);
     
     return {
       totalSyncs: history.length,
       byType: groupBy(history, 'type'),
       byDirection: groupBy(history, 'direction'),
       successRate: calculateRate(history, 'success'),
       
       patterns: {
         peakHours: findPeakSyncHours(history),
         commonErrors: findCommonErrors(history),
         slowestOperations: findSlowestOps(history)
       },
       
       trends: {
         syncVolume: calculateTrend(history, 'volume'),
         errorRate: calculateTrend(history, 'errors'),
         performance: calculateTrend(history, 'duration')
       }
     };
   }
   ```

5. **Monitoramento em Tempo Real**
   ```javascript
   class SyncMonitor {
     constructor() {
       this.metrics = new Map();
       this.alerts = [];
     }
     
     track(operation) {
       const start = Date.now();
       
       return {
         complete: (success, details) => {
           const duration = Date.now() - start;
           this.metrics.set(operation.id, {
             ...operation,
             duration,
             success,
             details,
             timestamp: new Date()
           });
           
           // Verificar alertas
           if (duration > SLOW_SYNC_THRESHOLD) {
             this.alert('SLOW_SYNC', operation);
           }
           if (!success) {
             this.alert('SYNC_FAILURE', operation);
           }
         }
       };
     }
   }
   ```

6. **Status de Webhook**
   ```bash
   # Verificar webhooks do GitHub
   gh api repos/:owner/:repo/hooks --jq '.[] | select(.config.url | contains("linear"))'
   
   # Validar integridade do webhook
   gh api repos/:owner/:repo/hooks/:id/deliveries --jq '.[0:10] | .[] | {id, status_code, delivered_at}'
   ```

7. **Gerenciamento de Fila**
   ```javascript
   async function getQueueStatus() {
     const queue = await loadSyncQueue();
     
     return {
       size: queue.length,
       oldest: queue[0]?.createdAt,
       byPriority: groupBy(queue, 'priority'),
       estimatedTime: estimateProcessingTime(queue),
       
       blocked: queue.filter(item => item.retries >= MAX_RETRIES),
       processing: queue.filter(item => item.status === 'processing'),
       pending: queue.filter(item => item.status === 'pending')
     };
   }
   ```

8. **Relatórios de Diagnóstico**
   ```javascript
   function generateDiagnostics() {
     return {
       systemInfo: {
         version: SYNC_VERSION,
         githubCLI: checkGitHubCLI(),
         linearMCP: checkLinearMCP(),
         config: loadSyncConfig()
       },
       
       connectivity: {
         github: testGitHubAPI(),
         linear: testLinearAPI(),
         webhooks: testWebhooks()
       },
       
       dataIntegrity: {
         orphanedGitHub: findOrphanedGitHubIssues(),
         orphanedLinear: findOrphanedLinearTasks(),
         duplicates: findDuplicates(),
         conflicts: findConflicts()
       },
       
       recommendations: generateRecommendations()
     };
   }
   ```

9. **Configuração de Alertas**
   ```yaml
   alerts:
     - name: high_conflict_rate
       condition: conflict_rate > 10%
       severity: warning
       action: notify
     
     - name: sync_failure
       condition: success_rate < 95%
       severity: critical
       action: pause_sync
     
     - name: api_rate_limit
       condition: rate_limit_remaining < 100
       severity: warning
       action: throttle
   ```

10. **Visualização de Desempenho**
    ```
    Desempenho de Sincronização (Últimas 24h)
    ━━━━━━━━━━━━━━━━━━━━━━━━━
    
    Volume de Sincronização:
    00:00 ▁▁▂▁▁▁▂▃▄▅▆▇█▇▆▅▄▃▂▁▁▁▂▁ 23:59
    
    Taxa de Sucesso: 98.5%
    ████████████████████░ 
    
    Duração Média: 2.3s
    ████████░░░░░░░░░░░░ (Meta: 5s)
    ```

## Exemplos

### Verificação de Status Básica
```bash
# Obter status de sincronização atual
claude sync-status

# Status detalhado com histórico
claude sync-status --detailed

# Verificar tipos específicos de sincronização
claude sync-status --type="issue-to-linear"
```

### Monitoramento de Integridade
```bash
# Executar verificação de integridade
claude sync-status --health-check

# Monitoramento contínuo
claude sync-status --monitor --interval=5m

# Gerar relatório de diagnóstico
claude sync-status --diagnostics
```

### Solução de Problemas
```bash
# Verificar problemas de sincronização
claude sync-status --check-issues

# Verificar itens específicos
claude sync-status --verify="gh-123,ABC-456"

# Gerenciamento de fila
claude sync-status --queue --clear-failed
```

## Formato de Saída

```
Status de Sincronização GitHub-Linear
======================================
Última atualização: 2025-01-16 10:45:00

Visão Geral:
✓ Sincronização Ativada: Bidirecional
✓ Webhooks: Ativos (GitHub: ✓, Linear: ✓)
✓ Última Sincronização Completa: há 2 horas
✓ Última Atividade: há 5 minutos

Estatísticas:
- Total de Itens Sincronizados: 1.234
- Itens na Fila: 3
- Itens Falhados: 1

Métricas de Integridade:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Taxa de Sucesso       █████████████████░░░ 96.5%
Taxa de Conflito      ███░░░░░░░░░░░░░░░░  8.2%
Latência de Sinc.     ████░░░░░░░░░░░░░░░ ~2min

Status da API:
- GitHub: 4.832/5.000 requisições restantes
- Linear: 1.245/1.500 requisições restantes

Atividades Recentes:
10:44 ✓ Issue #123 → ABC-789 (1.2s)
10:42 ✓ ABC-788 → Issue #122 (0.8s)
10:40 ⚠ Issue #121 → Conflito detectado
10:38 ✓ PR #456 → ABC-787 vinculado

Alertas:
⚠ Taxa alta de conflito na última hora (12%)
⚠ 1 item falhou após máximo de tentativas

Recomendações:
1. Revisar e resolver conflito para Issue #121
2. Repetir sincronização falhada para ABC-456
3. Considerar aumentar frequência de sincronização
```

## Funcionalidades Avançadas

### Dashboard de Análise de Sincronização
```
═══════════════════════════════════════════════════════
                 DASHBOARD DE ANÁLISE DE SINCRONIZAÇÃO
═══════════════════════════════════════════════════════

Volume de Sincronização Diária │ Tipos de Sincronização
─────────────────────────────┼─────────────────────────
     150 ┤               │ Issues → Linear  45%
     120 ┤    ╭─╮        │ Linear → Issues  30%
      90 ┤   ╱  ╲        │ PR → Tarefa      20%
      60 ┤  ╱    ╲       │ Comentários       5%
      30 ┤ ╱      ╲___   │
       0 └─────────────   │
         Seg  Qua  Sex    │

Distribuição de Erros    │ Tendências de Desempenho
─────────────────────────┼─────────────────────────
Rede         ████ 40%     │ Tempo Médio  ▂▄▆█▆▄▂ 2.3s
Limite Taxa  ███  30%     │ Tempo P95    ▃▅▇█▇▅▃ 5.1s
Conflitos    ██   20%     │ Tempo P99    ▄▆███▆▄ 8.2s
Outro        █    10%     │
```

### Análise Preditiva
```javascript
function predictSyncIssues() {
  const patterns = analyzeHistoricalData();
  
  return {
    likelyConflicts: predictConflicts(patterns),
    peakLoadTimes: predictPeakLoad(patterns),
    rateLimitRisk: calculateRateLimitRisk(),
    recommendations: {
      optimalSyncInterval: calculateOptimalInterval(),
      suggestedBatchSize: calculateOptimalBatch(),
      conflictPrevention: suggestConflictStrategies()
    }
  };
}
```

## Melhores Práticas

1. **Monitoramento Regular**
   - Configurar verificações de integridade automatizadas
   - Revisar métricas de sincronização diariamente
   - Agir rapidamente sobre alertas

2. **Manutenção Proativa**
   - Limpar itens falhados regularmente
   - Otimizar intervalos de sincronização
   - Atualizar estratégias de resolução de conflitos

3. **Documentação**
   - Registrar todos os problemas de sincronização
   - Documentar etapas de resolução
   - Acompanhar tendências de desempenho