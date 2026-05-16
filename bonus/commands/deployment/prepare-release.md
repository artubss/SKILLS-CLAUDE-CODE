---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [version-type] | patch | minor | major | --pre-release | --hotfix
description: Preparar e validar pacotes de release com testes abrangentes, documentação e automação
---

# Preparação de Release

Preparar e validar release: $ARGUMENTS

## Contexto Atual da Release

- Versão atual: !`git describe --tags --abbrev=0 2>/dev/null || echo "No previous releases"`
- Versão do pacote: @package.json ou @setup.py ou @pyproject.toml ou @go.mod (se existir)
- Mudanças não lançadas: !`git log $(git describe --tags --abbrev=0)..HEAD --oneline 2>/dev/null | wc -l || echo "All commits"`
- Status do branch: !`git status --porcelain | wc -l || echo "0"` alterações não commitadas
- Status do build: !`npm test 2>/dev/null || python -m pytest 2>/dev/null || go test ./... 2>/dev/null || echo "Test framework detection needed"`

## Tarefa

Preparação sistemática de release: $ARGUMENTS

1. **Planejamento e Validação da Release**
   - Determinar número da versão (versionamento semântico)
   - Revisar e validar todos os recursos inclusos na release
   - Verificar que todos os problemas e recursos planejados foram concluídos
   - Validar critérios de release e requisitos de aceitação

2. **Checklist Pré-Release**
   - Garantir que todos os testes estão passando (unit, integração, E2E)
   - Verificar que a cobertura de código atende aos padrões do projeto
   - Realizar verificação completa de vulnerabilidades de segurança
   - Executar testes de desempenho e validação
   - Revisar e aprovar todos os pull requests pendentes

3. **Gerenciamento de Versão**
   ```bash
   # Verificar versão atual
   git describe --tags --abbrev=0
   
   # Determinar próxima versão (versionamento semântico)
   # MAJOR.MINOR.PATCH
   # MAJOR: Mudanças incompatíveis
   # MINOR: Novos recursos (compatível com versões anteriores)
   # PATCH: Correções de bugs (compatível com versões anteriores)
   
   # Exemplos de atualização de versão
   # 1.2.3 -> 1.2.4 (patch)
   # 1.2.3 -> 1.3.0 (minor)
   # 1.2.3 -> 2.0.0 (major)
   ```

4. **Congelamento de Código e Gerenciamento de Branch**
   ```bash
   # Criar branch de release a partir da main
   git checkout main
   git pull origin main
   git checkout -b release/v1.2.3
   
   # Alternativa: Usar branch main diretamente para releases menores
   # Garantir que nenhum novo recurso seja mesclado durante a release
   ```

5. **Atualizações do Número de Versão**
   - Atualizar package.json, setup.py ou arquivos equivalentes de versão
   - Atualizar versão na configuração da aplicação
   - Atualizar versão em documentação e README
   - Atualizar versão da API se aplicável

   ```bash
   # Projetos Node.js
   npm version patch  # ou minor, major
   
   # Projetos Python
   # Atualizar versão em setup.py, __init__.py ou pyproject.toml
   
   # Atualização manual de versão
   sed -i 's/"version": "1.2.2"/"version": "1.2.3"/' package.json
   ```

6. **Geração de Changelog**
   ```markdown
   # CHANGELOG.md
   
   ## [1.2.3] - 2024-01-15
   
   ### Adicionado
   - Novo sistema de autenticação de usuário
   - Suporte para dark mode na UI
   - Funcionalidade de rate limiting na API
   
   ### Alterado
   - Desempenho aprimorado das queries do banco de dados
   - Design da interface do usuário atualizado
   - Tratamento de erros melhorado
   
   ### Corrigido
   - Corrigido vazamento de memória em tarefas em background
   - Resolvido problema com validação de upload de arquivo
   - Corrigido tratamento de timezone em cálculos de data
   
   ### Segurança
   - Dependências atualizadas com patches de segurança
   - Validação de entrada e sanitização melhoradas
   ```

7. **Atualizações de Documentação**
   - Atualizar documentação de API com novos endpoints
   - Revisar documentação de usuário e guias
   - Atualizar instruções de instalação e deployment
   - Revisar e atualizar README.md
   - Atualizar guias de migração se necessário

8. **Gerenciamento de Dependências**
   ```bash
   # Atualizar e auditar dependências
   npm audit fix
   npm update
   
   # Python
   pip-audit
   pip freeze > requirements.txt
   
   # Revisar vulnerabilidades de segurança
   npm audit
   snyk test
   ```

9. **Build e Geração de Artifacts**
   ```bash
   # Limpar ambiente de build
   npm run clean
   rm -rf dist/ build/
   
   # Build dos artifacts de produção
   npm run build
   
   # Verificar artifacts do build
   ls -la dist/
   
   # Testar artifacts compilados
   npm run test:build
   ```

10. **Testes e Garantia de Qualidade**
    - Executar suite completa de testes
    - Realizar testes manuais de recursos críticos
    - Executar testes de regressão
    - Conduzir testes de aceitação do usuário
    - Validar em ambiente de staging

    ```bash
    # Executar todos os testes
    npm test
    npm run test:integration
    npm run test:e2e
    
    # Verificar cobertura de código
    npm run test:coverage
    
    # Testes de desempenho
    npm run test:performance
    ```

11. **Verificação de Segurança e Conformidade**
    - Executar scans de segurança e testes de penetração
    - Validar conformidade com padrões de segurança
    - Verificar secrets ou credenciais expostas
    - Validar medidas de proteção de dados e privacidade

12. **Preparação de Release Notes**
    ```markdown
    # Release Notes v1.2.3
    
    ## 🎉 Novidades
    - **Dark Mode**: Usuários agora podem alternar para dark mode nas configurações
    - **Segurança Aprimorada**: Autenticação melhorada com suporte a 2FA
    - **Desempenho**: Tempo de carregamento de página 40% mais rápido
    
    ## 🔧 Melhorias
    - Mensagens de erro melhores para validação de formulário
    - Responsividade mobile aprimorada
    - Recursos de acessibilidade aprimorados
    
    ## 🐛 Correções de Bugs
    - Corrigido problema com downloads de arquivo no Safari
    - Resolvido vazamento de memória em tarefas em background
    - Corrigido problemas com exibição de timezone
    
    ## 📚 Documentação
    - Documentação de API atualizada
    - Novo guia de onboarding para usuários
    - Seção de troubleshooting aprimorada
    
    ## 🔄 Guia de Migração
    - Nenhuma mudança incompatível nesta release
    - Migrações automáticas de banco de dados incluídas
    - Consulte [Guia de Migração](link) para detalhes
    ```

13. **Tagging de Release e Versionamento**
    ```bash
    # Criar tag anotada
    git add .
    git commit -m "chore: prepare release v1.2.3"
    git tag -a v1.2.3 -m "Release version 1.2.3
    
    Features:
    - Dark mode support
    - Enhanced authentication
    
    Bug fixes:
    - Fixed file upload issues
    - Resolved memory leaks"
    
    # Fazer push da tag para remote
    git push origin v1.2.3
    git push origin release/v1.2.3
    ```

14. **Preparação de Deployment**
    - Preparar scripts de deployment e configurações
    - Atualizar variáveis de ambiente e secrets
    - Planejar estratégia de deployment (blue-green, rolling, canary)
    - Configurar monitoring e alertas para a release
    - Preparar procedimentos de rollback

15. **Validação do Ambiente de Staging**
    ```bash
    # Deploy para staging
    ./deploy-staging.sh v1.2.3
    
    # Executar smoke tests
    npm run test:smoke:staging
    
    # Checklist de validação manual
    # [ ] Login/logout de usuário
    # [ ] Funcionalidade principal
    # [ ] Novos recursos
    # [ ] Métricas de desempenho
    # [ ] Verificações de segurança
    ```

16. **Planejamento de Deployment em Produção**
    - Agendar janela de deployment
    - Notificar stakeholders e usuários
    - Preparar modo de manutenção se necessário
    - Configurar monitoring de deployment
    - Planejar estratégia de comunicação

17. **Configuração de Automação de Release**
    ```yaml
    # GitHub Actions Release Workflow
    name: Release
    
    on:
      push:
        tags:
          - 'v*'
    
    jobs:
      release:
        runs-on: ubuntu-latest
        steps:
          - uses: actions/checkout@v3
          - name: Setup Node.js
            uses: actions/setup-node@v3
            with:
              node-version: '18'
          
          - name: Install dependencies
            run: npm ci
          
          - name: Run tests
            run: npm test
          
          - name: Build
            run: npm run build
          
          - name: Create Release
            uses: actions/create-release@v1
            env:
              GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
            with:
              tag_name: ${{ github.ref }}
              release_name: Release ${{ github.ref }}
              draft: false
              prerelease: false
    ```

18. **Comunicação e Anúncios**
    - Preparar anúncio da release
    - Atualizar página de status e documentação
    - Notificar clientes e usuários
    - Compartilhar em canais de comunicação relevantes
    - Atualizar materiais de social media e marketing

19. **Monitoring Pós-Release**
    - Monitorar desempenho e erros da aplicação
    - Rastrear adoção de novos recursos pelos usuários
    - Monitorar métricas e alertas do sistema
    - Coletar feedback e problemas dos usuários
    - Preparar procedimentos de hotfix se necessário

20. **Retrospectiva da Release**
    - Documentar lições aprendidas
    - Revisar efetividade do processo de release
    - Identificar oportunidades de melhoria
    - Atualizar procedimentos de release
    - Planejar para próximo ciclo de release

**Tipos de Release e Considerações:**

**Release de Patch (1.2.3 → 1.2.4):**
- Apenas correções de bugs
- Nenhum novo recurso
- Testes mínimos necessários
- Deployment rápido

**Release Minor (1.2.3 → 1.3.0):**
- Novos recursos (compatível com versões anteriores)
- Funcionalidade aprimorada
- Testes abrangentes
- Comunicação com usuários necessária

**Release Major (1.2.3 → 2.0.0):**
- Mudanças incompatíveis
- Recursos novos significativos
- Guia de migração obrigatório
- Período estendido de testes
- Treinamento e suporte para usuários

**Release de Hotfix:**
```bash
# Processo de hotfix de emergência
git checkout main
git pull origin main
git checkout -b hotfix/critical-bug-fix

# Fazer fix mínimo
git add .
git commit -m "hotfix: fix critical security vulnerability"

# Testes acelerados e deployment
npm test
git tag -a v1.2.4-hotfix.1 -m "Hotfix for critical security issue"
git push origin hotfix/critical-bug-fix
git push origin v1.2.4-hotfix.1
```

Lembre-se de:
- Testar tudo minuciosamente antes da release
- Comunicar claramente com todos os stakeholders
- Ter procedimentos de rollback prontos
- Monitorar a release de perto após deployment
- Documentar tudo para próximas releases