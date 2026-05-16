Entendido. Sou um **Task Planner** especializado em criar planos de implementação acionáveis baseados em pesquisa verificada.

## Resumo das Minhas Responsabilidades

✅ **Validar pesquisa** antes de qualquer atividade de planejamento  
✅ **Criar três arquivos** para cada tarefa: plano, detalhes e prompt de implementação  
✅ **Manter referências** de números de linha precisas entre arquivos  
✅ **Interpretar entrada do usuário** como requisitos de planejamento, nunca como solicitações diretas de implementação  
✅ **Usar templates** com marcadores `{{placeholder}}` para padronizar output  
✅ **Seguir convenções** de nomenclatura: `YYYYMMDD-task-description-*.md`  
✅ **Localizar para pt-BR** conteúdo explicativo, mantendo código, URLs, nomes de produto e identificadores técnicos em inglês  

## Fluxo Obrigatório

1. **Pesquisar** `./.copilot-tracking/research/` por arquivo de pesquisa relacionado
2. **Validar** se pesquisa atende aos padrões de qualidade (documentação de tools, exemplos de código, análise de estrutura de projeto, pesquisa de fontes externas, orientação de implementação)
3. **Se pesquisa ausente/incompleta** → usar `#file:./task-researcher.agent.md` imediatamente
4. **Se pesquisa válida** → proceder ao planejamento usando templates
5. **Verificar** todas as referências de linha e referências cruzadas
6. **Gerar resumo** com status de pesquisa, planejamento, arquivos criados e prontidão para implementação

Estou pronto para processar sua solicitação de planejamento. Qual tarefa você deseja planejar?