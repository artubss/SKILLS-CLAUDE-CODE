---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [pipeline-type] | --art | --audio | --models | --textures | --comprehensive
description: Use PROACTIVELY para construir pipelines automatizados de processamento de ativos de jogos com otimização, validação e sistemas de entrega multiplataforma
---

# Sistema de Pipeline e Processamento de Ativos de Jogos

Construa pipeline abrangente de processamento de ativos: $ARGUMENTS

## Ambiente Atual de Ativos

- Ativos do projeto: !`find . -name "*.png" -o -name "*.fbx" -o -name "*.wav" -o -name "*.mp3" | wc -l` ativos totais
- Tamanhos de ativos: !`du -sh Assets/ 2>/dev/null || du -sh assets/ 2>/dev/null || echo "No assets folder found"`
- Ferramentas de build: !`which blender`; !`which ffmpeg`; !`which imagemagick`
- Plataformas alvo: @ProjectSettings/ProjectSettings.asset ou detectar de configs de build
- Controle de versão: !`git lfs ls-files | wc -l` arquivos rastreados em LFS

## Tarefa

Crie um pipeline automatizado de processamento de ativos com otimização, validação, entrega específica por plataforma e monitoramento em tempo real para fluxos de trabalho de desenvolvimento de jogos.

## Componentes do Pipeline de Ativos

### 1. Importação e Validação de Ativos
- Validação automatizada de formato de ativos e padronização
- Verificações de garantia de qualidade para resolução de texturas e complexidade do modelo
- Aplicação de convenções de nomenclatura de ativos
- Sistema de extração de metadados e marcação
- Backup de ativos de origem e integração com controle de versão

### 2. Otimização Multiplataforma
- Compressão de texturas específica da plataforma (ASTC, DXT, etc.)
- Geração e otimização de LOD de modelo
- Conversão e compressão de formato de áudio
- Compilação de variantes de shader para plataformas alvo
- Validação de orçamento de memória por plataforma

### 3. Integração de Build
- Processamento automatizado de ativos durante pipeline de build
- Processamento incremental apenas para ativos modificados
- Geração e empacotamento de asset bundles
- Rastreamento e resolução de dependências
- Validação de ativos em tempo de build e relatório de erros

### 4. Garantia de Qualidade
- Comparação visual de diferenças em texturas
- Validação e otimização de geometria de modelo
- Análise de qualidade de áudio e taxa de compressão
- Avaliação de impacto de desempenho para novos ativos
- Testes de regressão automatizados para mudanças de ativos

## Fluxos de Trabalho de Processamento

### Pipeline de Processamento de Texturas
- Validação de importação e padronização de formato
- Geração e otimização automática de mipmaps
- Compressão específica da plataforma com configurações de qualidade
- Estimativa e otimização de uso de memória
- Integração com sprite atlasing e texture streaming

### Pipeline de Processamento de Modelos 3D
- Validação de importação e otimização de malha
- Geração automática de LOD com proporções de redução configuráveis
- Otimização de osso e animação
- Validação e otimização de coordenadas de textura
- Geração e validação de malha de colisão

### Pipeline de Processamento de Áudio
- Padronização de formato e validação de qualidade
- Compressão específica da plataforma com otimização de bitrate
- Marcação e categorização de ativos de áudio
- Recomendações de streaming versus carregamento em memória
- Preparação de oclusão de áudio e espacialização

### Pipeline de Processamento de Animação
- Otimização e compressão de clipe de animação
- Redução e suavização de keyframe
- Validação e otimização de hierarquia de osso
- Validação de evento de animação e documentação
- Análise de impacto de desempenho em tempo de execução

## Entregáveis

1. **Configuração de Processamento de Ativos**
   - Regras e configurações de processamento específicas da plataforma
   - Limites de qualidade e critérios de validação
   - Acionadores e condições de fluxo de trabalho automatizado

2. **Implementação do Pipeline**
   - Scripts de processamento de ativos e ferramentas de automação
   - Integração de sistema de build e deployment
   - Hooks de controle de versão e rastreamento de ativos

3. **Monitoramento e Relatórios**
   - Métricas de desempenho de processamento de ativos
   - Relatórios de garantia de qualidade e resultados de validação
   - Relatórios de compatibilidade de plataforma e otimização

4. **Documentação e Diretrizes**
   - Diretrizes de criação de ativos para artistas e designers
   - Documentação de uso do pipeline e solução de problemas
   - Diretrizes de impacto de desempenho e boas práticas

## Diretrizes de Integração

Implemente o pipeline com otimizações específicas do engine de jogos e ferramentas padrão da indústria. Garanta escalabilidade para colaboração em equipe e fluxos de trabalho de deployment automatizado.