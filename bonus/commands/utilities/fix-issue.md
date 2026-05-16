# Comando Corrigir Issue

Identifique e resolva problemas de código

## Instruções

Siga esta abordagem estruturada para analisar e corrigir issues: **$ARGUMENTS**

1. **Análise da Issue**
   - Use `gh issue view $ARGUMENTS` para obter os detalhes completos da issue
   - Leia a descrição da issue, comentários e qualquer log/screenshot anexado
   - Identifique o tipo de issue (bug, requisição de feature, melhoria, etc.)
   - Compreenda o comportamento esperado vs comportamento atual

2. **Configuração do Ambiente**
   - Garanta que você está na branch correta (geralmente main/master)
   - Puxe as últimas mudanças: `git pull origin main`
   - Crie uma nova feature branch: `git checkout -b fix/issue-$ARGUMENTS`

3. **Reproduzir a Issue**
   - Siga as etapas de reprodução descritas na issue
   - Configure o ambiente de desenvolvimento se necessário
   - Execute a aplicação/testes para confirmar que a issue existe
   - Documente o comportamento atual

4. **Análise de Causa Raiz**
   - Procure no codebase pelos arquivos e funções relevantes
   - Use ferramentas grep/search para localizar o código problemático
   - Analise a lógica do código e identifique a causa raiz
   - Verifique se há issues relacionadas ou padrões similares

5. **Design da Solução**
   - Projete uma solução que aborde a causa raiz, não apenas os sintomas
   - Considere casos extremos e possíveis efeitos colaterais
   - Garanta que a solução segue as convenções e padrões do projeto
   - Planeje compatibilidade retroativa se necessário

6. **Implementação**
   - Implemente a correção com código limpo e legível
   - Siga os padrões de codificação e estilo do projeto
   - Adicione tratamento de erros apropriado e logging
   - Mantenha as mudanças mínimas e focadas

7. **Estratégia de Testes**
   - Escreva ou atualize testes para cobrir a correção
   - Garanta que os testes existentes ainda passam
   - Teste casos extremos e condições de erro
   - Execute a suite de testes completa para verificar regressões

8. **Verificações de Qualidade de Código**
   - Execute ferramentas de linting e formatação
   - Realize análise estática se disponível
   - Verifique implicações de segurança
   - Garanta que o desempenho não foi impactado negativamente

9. **Atualizações de Documentação**
   - Atualize a documentação relevante se necessário
   - Adicione ou atualize comentários de código para clareza
   - Atualize o changelog se o projeto mantém um
   - Documente quaisquer mudanças quebradas

10. **Commit e Push**
    - Prepare as mudanças: `git add .`
    - Crie uma mensagem de commit descritiva seguindo as convenções do projeto
    - Exemplo: `fix: resolve issue com timeout de autenticação de usuário (#$ARGUMENTS)`
    - Faça push da branch: `git push origin fix/issue-$ARGUMENTS`

11. **Criar Pull Request**
    - Use `gh pr create` para criar um pull request
    - Referencie a issue na descrição do PR: "Fixes #$ARGUMENTS"
    - Forneça uma descrição clara das mudanças e testes realizados
    - Adicione labels e revisores apropriados

12. **Acompanhamento**
    - Monitore o PR para feedback e mudanças solicitadas
    - Aborde os comentários de revisão prontamente
    - Atualize a issue com progresso e resolução
    - Garanta que as verificações de CI/CD passam

13. **Verificação**
    - Após o merge, verifique a correção na branch main
    - Feche a issue se não for fechada automaticamente
    - Monitore se há issues relacionadas ou regressões

Lembre-se de comunicar-se claramente tanto em código quanto em comentários, e sempre priorize soluções mantíveis em vez de correções rápidas.