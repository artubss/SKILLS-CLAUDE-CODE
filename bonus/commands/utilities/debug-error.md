# Depuração e Correção Sistemática de Erros

Depuração e correção sistemática de erros

## Instruções

Siga esta metodologia de depuração abrangente para resolver: **$ARGUMENTS**

1. **Coleta de Informações sobre o Erro**
   - Colete a mensagem de erro completa, stack trace e código de erro
   - Anote quando o erro ocorre (timing, condições, frequência)
   - Identifique o ambiente onde o erro acontece (dev, staging, prod)
   - Reúna logs relevantes de antes e depois do erro

2. **Reprodução do Erro**
   - Crie um caso de teste mínimo que reproduza o erro consistentemente
   - Documente os passos exatos necessários para disparar o erro
   - Teste em diferentes ambientes se possível
   - Anote padrões ou condições que afetam a ocorrência do erro

3. **Análise de Stack Trace**
   - Leia o stack trace de baixo para cima para entender a cadeia de chamadas
   - Identifique a linha exata onde o erro se origina
   - Rastreie o caminho de execução que leva ao erro
   - Procure por problemas óbvios no código que falha

4. **Investigação de Contexto de Código**
   - Examine o código em torno da localização do erro
   - Verifique mudanças recentes que possam ter introduzido o bug
   - Revise valores de variáveis e estado no momento do erro
   - Analise parâmetros de função e valores de retorno

5. **Formação de Hipóteses**
   - Com base em evidências, forme hipóteses sobre a causa raiz
   - Considere causas comuns:
     - Referência null/undefined
     - Incompatibilidade de tipos
     - Condições de corrida (race conditions)
     - Esgotamento de recursos
     - Erros de lógica
     - Falhas de dependências externas

6. **Configuração de Ferramentas de Depuração**
   - Configure ferramentas de depuração apropriadas para o stack de tecnologia
   - Use debugger, profiler ou logging conforme necessário
   - Configure breakpoints em locais estratégicos
   - Configure monitoramento e alertas se ainda não estiverem presentes

7. **Investigação Sistemática**
   - Teste cada hipótese metodicamente
   - Use abordagem de busca binária para isolar o problema
   - Adicione logs ou print statements estratégicos
   - Verifique fluxo de dados e transformações passo a passo

8. **Validação de Dados**
   - Verifique formato e validade dos dados de entrada
   - Procure por casos extremos e condições limites
   - Valide pressupostos sobre o estado dos dados
   - Teste com diferentes conjuntos de dados para isolar padrões

9. **Análise de Dependências**
   - Verifique dependências externas e suas versões
   - Valide conectividade de rede e disponibilidade de API
   - Revise arquivos de configuração e variáveis de ambiente
   - Teste conexões de banco de dados e execução de queries

10. **Análise de Memória e Recursos**
    - Verifique vazamentos de memória ou uso excessivo
    - Monitore consumo de CPU e I/O
    - Analise padrões de coleta de lixo (garbage collection) se aplicável
    - Procure por deadlocks ou contenção de recursos

11. **Investigação de Problemas de Concorrência**
    - Procure por condições de corrida em código multi-thread
    - Verifique mecanismos de sincronização e locks
    - Analise operações async e tratamento de promises
    - Teste sob diferentes condições de carga

12. **Identificação da Causa Raiz**
    - Uma vez identificada a causa, entenda por que aconteceu
    - Determine se é erro de lógica, falha de design ou problema externo
    - Avalie o escopo e impacto do problema
    - Considere se problemas similares existem em outro lugar

13. **Implementação da Solução**
    - Projete uma correção que aborde a causa raiz
    - Considere múltiplas abordagens de solução e trade-offs
    - Implemente a correção com tratamento apropriado de erros
    - Adicione validação e programação defensiva onde necessário

14. **Teste da Correção**
    - Teste a correção contra o caso de erro original
    - Teste casos extremos e cenários relacionados
    - Execute testes de regressão para garantir nenhum novo problema
    - Teste sob várias condições de carga e stress

15. **Medidas de Prevenção**
    - Adicione testes unitários e de integração apropriados
    - Melhore tratamento de erros e logging
    - Adicione validação de entrada e verificações defensivas
    - Atualize documentação e comentários de código

16. **Monitoramento e Alertas**
    - Configure monitoramento para problemas similares
    - Adicione métricas e verificações de integridade
    - Configure alertas para limites de erro
    - Implemente melhor observabilidade

17. **Documentação**
    - Documente o erro, processo de investigação e solução
    - Atualize guias de troubleshooting
    - Compartilhe aprendizados com o time
    - Atualize comentários de código com contexto

18. **Revisão Pós-Resolução**
    - Analise por que o erro não foi capturado antes
    - Revise processos de desenvolvimento e testes
    - Considere melhorias para prevenir problemas similares
    - Atualize padrões de código ou diretrizes se necessário

Lembre-se de manter notas detalhadas durante todo o processo de depuração e considere as implicações mais amplas tanto do erro quanto da correção.