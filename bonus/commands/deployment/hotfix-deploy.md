---
allowed-tools: Read, Edit, Bash
argument-hint: [tipo-hotfix] | --security | --critical | --rollback-ready | --emergency
description: Faça deploy de hotfixes críticos com procedimentos de emergência, validação e capacidades de rollback
---

# Deploy de Hotfix de Emergência

Faça deploy de hotfix crítico: $ARGUMENTS

## Estado Atual da Produção

- Versão atual: !`git describe --tags --abbrev=0 2>/dev/null || echo "No tags found"`
- Branch de produção: !`git branch --show-current`
- Commits recentes: !`git log --oneline -5`
- Status do deployment: !`curl -s https://api.example.com/health 2>/dev/null | jq -r '.version // "Unknown"' || echo "Health check failed"`
- Ambiente de staging: Verifique capacidades de deployment em staging

## Protocolo de Resposta de Emergência

Execute deploy de hotfix de emergência: $ARGUMENTS

1. **Avaliação e Triagem de Emergência**
   - Avalie a severidade e impacto do problema
   - Determine se um hotfix é necessário ou se pode esperar
   - Identifique sistemas afetados e impacto para usuários
   - Estime sensibilidade ao tempo e impacto comercial
   - Documente o incidente e a lógica da decisão

2. **Preparação da Resposta a Incidentes**
   - Crie rastreamento de incidentes no seu sistema de gerenciamento
   - Configure sala de crise ou canal de comunicação
   - Notifique stakeholders e membros da equipe on-call
   - Estabeleça protocolos claros de comunicação
   - Documente detalhes iniciais do incidente e cronograma

3. **Preparação de Branch e Ambiente**
   ```bash
   # Create hotfix branch from production tag
   git fetch --tags
   git checkout tags/v1.2.3  # Latest production version
   git checkout -b hotfix/critical-auth-fix
   
   # Alternative: Branch from main if using trunk-based development
   git checkout main
   git pull origin main
   git checkout -b hotfix/critical-auth-fix
   ```

4. **Processo de Desenvolvimento Rápido**
   - Mantenha mudanças mínimas e focadas apenas no problema crítico
   - Evite refatoração, otimização ou melhorias não relacionadas
   - Use padrões bem testados e abordagens estabelecidas
   - Adicione apenas logging mínimo para fins de troubleshooting
   - Siga convenções e padrões de código existentes

5. **Testes Acelerados**
   ```bash
   # Run focused tests related to the fix
   npm test -- --testPathPattern=auth
   npm run test:security
   
   # Manual testing checklist
   # [ ] Core functionality works correctly
   # [ ] Hotfix resolves the critical issue
   # [ ] No new issues introduced
   # [ ] Critical user flows remain functional
   ```

6. **Code Review Acelerado**
   - Obtenha revisão expedita de membro sênior da equipe
   - Foque a revisão em segurança e correção
   - Use pair programming se disponível e o tempo permitir
   - Documente decisões de revisão e justificativa rapidamente
   - Garanta processo de aprovação apropriado mesmo sob pressão de tempo

7. **Versionamento e Tagging**
   ```bash
   # Update version for hotfix
   # 1.2.3 -> 1.2.4 (patch version)
   # or 1.2.3 -> 1.2.3-hotfix.1 (hotfix identifier)
   
   # Commit with detailed message
   git add .
   git commit -m "hotfix: fix critical authentication vulnerability
   
   - Fix password validation logic
   - Resolve security issue allowing bypass
   - Minimal change to reduce deployment risk
   
   Fixes: #1234"
   
   # Tag the hotfix version
   git tag -a v1.2.4 -m "Hotfix v1.2.4: Critical auth security fix"
   git push origin hotfix/critical-auth-fix
   git push origin v1.2.4
   ```

8. **Deployment em Staging e Validação**
   ```bash
   # Deploy to staging environment for final validation
   ./deploy-staging.sh v1.2.4
   
   # Critical path testing
   curl -X POST staging.example.com/api/auth/login \
        -H "Content-Type: application/json" \
        -d '{"email":"test@example.com","password":"testpass"}'
   
   # Run smoke tests
   npm run test:smoke:staging
   ```

9. **Estratégia de Deployment em Produção**
   
   **Blue-Green Deployment:**
   ```bash
   # Deploy to blue environment
   ./deploy-blue.sh v1.2.4
   
   # Validate blue environment health
   ./health-check-blue.sh
   
   # Switch traffic to blue environment
   ./switch-to-blue.sh
   
   # Monitor deployment metrics
   ./monitor-deployment.sh
   ```
   
   **Rolling Deployment:**
   ```bash
   # Deploy to subset of servers first
   ./deploy-rolling.sh v1.2.4 --batch-size 1
   
   # Monitor each batch deployment
   ./monitor-batch.sh
   
   # Continue with next batch if healthy
   ./deploy-next-batch.sh
   ```

10. **Checklist Pré-Deployment**
    ```bash
    # Verify all prerequisites are met
    # [ ] Database backup completed successfully
    # [ ] Rollback plan documented and ready
    # [ ] Monitoring alerts configured and active
    # [ ] Team members standing by for support
    # [ ] Communication channels established
    
    # Execute production deployment
    ./deploy-production.sh v1.2.4
    
    # Run immediate post-deployment validation
    ./validate-hotfix.sh
    ```

11. **Monitoramento em Tempo Real**
    ```bash
    # Monitor key application metrics
    watch -n 10 'curl -s https://api.example.com/health | jq .'
    
    # Monitor error rates and logs
    tail -f /var/log/app/error.log | grep -i "auth"
    
    # Track critical metrics:
    # - Response times and latency
    # - Error rates and exception counts
    # - User authentication success rates
    # - System resource usage (CPU, memory)
    ```

12. **Validação Pós-Deployment**
    ```bash
    # Run comprehensive validation tests
    ./test-critical-paths.sh
    
    # Test user authentication functionality
    curl -X POST https://api.example.com/auth/login \
         -H "Content-Type: application/json" \
         -d '{"email":"test@example.com","password":"testpass"}'
    
    # Validate security fix effectiveness
    ./security-validation.sh
    
    # Check overall system performance
    ./performance-check.sh
    ```

13. **Comunicação e Atualizações de Status**
    - Forneça atualizações de status regulares para stakeholders
    - Use canais de comunicação consistentes
    - Documente progresso e resultados do deployment
    - Atualize sistemas de rastreamento de incidentes
    - Notifique equipes relevantes sobre conclusão do deployment

14. **Procedimentos de Rollback**
    ```bash
    # Automated rollback script
    #!/bin/bash
    PREVIOUS_VERSION="v1.2.3"
    
    if [ "$1" = "rollback" ]; then
        echo "Rolling back to $PREVIOUS_VERSION"
        ./deploy-production.sh $PREVIOUS_VERSION
        ./validate-rollback.sh
        echo "Rollback completed successfully"
    fi
    
    # Manual rollback steps if automation fails:
    # 1. Switch load balancer back to previous version
    # 2. Validate previous version health and functionality
    # 3. Monitor system stability after rollback
    # 4. Communicate rollback status to team
    ```

15. **Período de Monitoramento Pós-Deployment**
    - Monitore o sistema por 2-4 horas após o deployment
    - Observe taxa de erros e métricas de performance de perto
    - Verifique feedback de usuários e volume de tickets de suporte
    - Valide que o hotfix resolve o problema original
    - Documente quaisquer problemas ou comportamentos inesperados

16. **Documentação e Relatório de Incidente**
    - Documente o processo e cronograma completo do hotfix
    - Registre lições aprendidas e melhorias de processo
    - Atualize sistemas de gerenciamento de incidentes com resolução
    - Crie materiais de revisão pós-incidente
    - Compartilhe conhecimento com a equipe para referência futura

17. **Merge de Volta para Branch Main**
    ```bash
    # After successful hotfix deployment and validation
    git checkout main
    git pull origin main
    git merge hotfix/critical-auth-fix
    git push origin main
    
    # Clean up hotfix branch
    git branch -d hotfix/critical-auth-fix
    git push origin --delete hotfix/critical-auth-fix
    ```

18. **Atividades Pós-Incidente**
    - Agende e conduza reunião de revisão pós-incidente
    - Atualize runbooks e procedimentos de emergência
    - Identifique e implemente melhorias de processo
    - Atualize configurações de monitoramento e alertas
    - Planeje medidas preventivas para evitar problemas similares

**Boas Práticas de Hotfix:**

- **Mantenha a Simplicidade:** Faça apenas mudanças mínimas focadas no problema crítico
- **Teste Minuciosamente:** Mantenha padrões de testes mesmo sob pressão de tempo
- **Comunique Claramente:** Mantenha todos os stakeholders informados ao longo do processo
- **Monitore de Perto:** Acompanhe o hotfix cuidadosamente no ambiente de produção
- **Documente Tudo:** Registre todas as decisões e ações para revisão pós-incidente
- **Planeje Rollback:** Sempre tenha uma forma testada de reverter mudanças rapidamente
- **Aprenda e Melhore:** Use cada incidente para fortalecer processos e procedimentos

**Diretrizes de Escalação de Emergência:**

```bash
# Emergency contact information
ON_CALL_ENGINEER="+1-555-0123"
SENIOR_ENGINEER="+1-555-0124"
ENGINEERING_MANAGER="+1-555-0125"
INCIDENT_COMMANDER="+1-555-0126"

# Escalation timeline thresholds:
# 15 minutes: Escalate to senior engineer
# 30 minutes: Escalate to engineering manager
# 60 minutes: Escalate to incident commander
```

**Lembretes Importantes:**

- Hotfixes devem ser usados apenas para emergências genuínas de produção
- Quando houver dúvida sobre severidade, siga o processo normal de release
- Sempre priorize estabilidade do sistema sobre velocidade de deployment
- Mantenha trilhas de auditoria claras para todas as mudanças de emergência
- Simulados regulares ajudam a garantir prontidão da equipe para emergências reais