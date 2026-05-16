---
allowed-tools: Ler, Escrever, Editar, Bash
argument-hint: [tipo-doc] | --implementação | --api | --arquitetura | --sincronizar | --validar
description: Atualizar sistematicamente a documentação do projeto com status de implementação, mudanças de API e conteúdo sincronizado
---

# Atualização e Sincronização de Documentação

Atualizar sistematicamente a documentação do projeto: $ARGUMENTS

## Estado Atual da Documentação

- Estrutura de documentação: !`find . -name "*.md" | head -10`
- Diretório specs: @specs/ (se existir)
- Status de implementação: !`grep -r "✅\|❌\|⚠️" docs/ specs/ 2>/dev/null | wc -l` indicadores de status
- Mudanças recentes: !`git log --oneline --since="1 week ago" -- "*.md" | head -5`
- Progresso do projeto: @CLAUDE.md ou @README.md (se existir)

## Tarefa

## Análise de Documentação

1. Revisar status atual da documentação:
   - Verificar `specs/implementation_status.md` para status geral do projeto
   - Revisar documento de fase implementada (`specs/phase{N}_implementation_plan.md`)
   - Revisar `specs/flutter_structurizr_implementation_spec.md` e `specs/flutter_structurizr_implementation_spec_updated.md`
   - Revisar `specs/testing_plan.md` para garantir que está atualizado conforme testes recentes aprovados, falhas e mudanças
   - Examinar `CLAUDE.md` e `README.md` para documentação em nível de projeto
   - Verificar e documentar quaisquer novas lições aprendidas ou melhores práticas em CLAUDE.md

2. Analisar resultados de implementação e testes:
   - Revisar o que foi implementado na última fase
   - Revisar resultados de testes e cobertura
   - Identificar novas melhores práticas descobertas durante a implementação
   - Anotar quaisquer desafios de implementação e soluções
   - Fazer referência cruzada da documentação atualizada com resultados recentes de implementação e testes para garantir precisão

## Atualizações de Documentação

1. Atualizar documento de implementação de fase:
   - Marcar tarefas concluídas com status ✅
   - Atualizar percentuais de implementação
   - Adicionar notas detalhadas sobre abordagem de implementação
   - Documentar quaisquer desvios do plano original com justificativa
   - Adicionar novas seções se necessário (lições aprendidas, melhores práticas)
   - Documentar detalhes de implementação específicos para componentes complexos
   - Incluir um resumo de quaisquer dicas de solução de problemas novas ou melhorias de fluxo de trabalho descobertas durante a fase

2. Atualizar documento de status de implementação:
   - Atualizar percentuais de conclusão de fase
   - Adicionar ou atualizar status de implementação para componentes
   - Adicionar notas sobre abordagem e decisões de implementação
   - Documentar melhores práticas descobertas durante a implementação
   - Anotar quaisquer desafios superados e soluções implementadas

3. Atualizar documentos de especificação de implementação:
   - Marcar itens concluídos com ✅ ou tachado, mas preservar requisitos originais
   - Adicionar notas sobre detalhes de implementação quando apropriado
   - Adicionar referências a arquivos implementados e classes
   - Atualizar qualquer orientação de implementação baseada em experiência

4. Atualizar CLAUDE.md e README.md se necessário:
   - Adicionar novas melhores práticas
   - Atualizar status do projeto
   - Adicionar nova orientação de implementação
   - Documentar problemas conhecidos ou limitações
   - Atualizar exemplos de uso para incluir nova funcionalidade

5. Documentar novos procedimentos de teste:
   - Adicionar detalhes sobre arquivos de teste criados
   - Incluir instruções de execução de testes
   - Documentar cobertura de testes
   - Explicar abordagem de testes para componentes complexos

## Formatação e Estrutura de Documentação

1. Manter estilo de documentação consistente:
   - Usar títulos e seções claros
   - Incluir exemplos de código quando útil
   - Usar indicadores de status (✅, ⚠️, ❌) consistentemente
   - Manter formatação apropriada de Markdown

2. Garantir completude de documentação:
   - Cobrir todas as funcionalidades implementadas
   - Incluir exemplos de uso
   - Documentar mudanças ou adições de API
   - Incluir orientação de solução de problemas para questões comuns

## Diretrizes

- NÃO CRIE novos arquivos de especificação
- ATUALIZE arquivos existentes no diretório `specs/`
- Mantenha estilo de documentação consistente
- Inclua exemplos práticos quando apropriado
- Faça referência cruzada de seções de documentação relacionadas
- Documente melhores práticas e lições aprendidas
- Forneça atualizações claras de status do projeto
- Atualize percentuais numéricos de conclusão
- Garanta que a documentação reflita a implementação real

Forneça um resumo das atualizações de documentação após conclusão, incluindo:
1. Arquivos atualizados
2. Principais mudanças na documentação
3. Percentuais de conclusão atualizados
4. Novas melhores práticas documentadas
5. Status do projeto geral após esta fase