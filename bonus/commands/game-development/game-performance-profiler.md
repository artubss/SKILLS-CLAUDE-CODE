---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [tipo-perfil] | --fps | --memoria | --renderizacao | --abrangente
description: Use PROATIVAMENTE para analisar gargalos de performance de jogos e gerar recomendações de otimização em múltiplas plataformas
---

# Análise de Performance & Otimização de Jogos

Analise a performance de jogos e gere recomendações de otimização: $ARGUMENTS

## Contexto de Performance Atual

- Game engine: @package.json ou detectar arquivos de projeto Unity/Unreal/Godot
- Plataformas alvo: !`find . -name "*.pbxproj" -o -name "*.gradle" -o -name "*.vcxproj" | head -3`
- Pipeline de assets: !`find . -name "*.meta" -o -name "*.asset" | wc -l` assets de jogo
- Configurações de build: !`grep -r "BuildTarget\|Platform" . 2>/dev/null | wc -l` configurações de plataforma
- Logs de performance: !`find . -name "*profile*" -o -name "*perf*" | head -5`

## Tarefa

Crie análise de performance abrangente com detecção automatizada de gargalos, sugestões de otimização e recomendações específicas por plataforma para projetos de desenvolvimento de jogos.

## Áreas de Análise de Performance

### 1. Performance de Taxa de Quadros & Renderização
- Analisar draw calls e eficiência de batching
- Identificar gargalos de overdraw e fillrate
- Revisar complexidade de shaders e oportunidades de otimização
- Avaliar potencial de otimização de meshes e texturas
- Verificar performance de renderização de iluminação e sombras

### 2. Análise de Uso de Memória
- Padrões de alocação de memória e vazamentos potenciais
- Uso de memória de texturas e oportunidades de compressão
- Sugestões de otimização de áudio
- Análise de object pooling e garbage collection
- Avaliação de restrições de memória específicas por plataforma

### 3. Profiling de Performance de CPU
- Identificação de gargalos de execução de scripts
- Oportunidades de otimização de simulação de física
- Análise de performance de IA e pathfinding
- Revisão de eficiência do sistema de animação
- Recomendações de threading e paralelização

### 4. Otimização Específica por Plataforma
- Considerações de performance mobile (bateria, thermal throttling)
- Diretrizes de otimização específicas de console
- Recomendações de scaling de hardware para PC
- Requisitos de performance de VR e otimizações
- Considerações de performance específicas de Web/WebGL

## Entregas

1. **Relatório de Auditoria de Performance**
   - Métricas de performance atuais e benchmarks
   - Gargalos identificados com classificação de severidade
   - Análise de performance específica por plataforma

2. **Recomendações de Otimização**
   - Sugestões de otimização priorizadas
   - Avaliação de dificuldade de implementação e impacto
   - Diretrizes de otimização de código e assets

3. **Configuração de Monitoramento**
   - Implementação de monitoramento de performance
   - Configuração de rastreamento de métricas-chave
   - Detecção automatizada de regressão de performance

4. **Estratégia de Teste**
   - Procedimentos de teste de performance
   - Recomendações de teste em dispositivos alvo
   - Configuração de monitoramento contínuo de performance

## Diretrizes de Implementação

Siga as melhores práticas de game engines e requisitos de plataformas alvo. Gere recomendações acionáveis com passos de implementação claros e melhorias de performance esperadas.