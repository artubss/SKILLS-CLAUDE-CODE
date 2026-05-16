---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [test-type] | --unit | --integration | --performance | --automation | --comprehensive
description: Use PROACTIVELY to implement comprehensive game testing frameworks with automated validation, performance testing, and multi-platform verification
---

# Framework de Testes para Jogos & Automação

Implemente framework de testes abrangente para jogos: $ARGUMENTS

## Contexto Atual de Testes

- Motor de jogo: @package.json or detect Unity/Unreal/Godot project files
- Testes existentes: !`find . -name "*test*" -o -name "*Test*" | head -10`
- Configuração CI/CD: @.github/workflows/ or @.gitlab-ci.yml or @Jenkinsfile (if exists)
- Configs de build: !`find . -name "*.sln" -o -name "*.csproj" -o -name "build.gradle" | head -3`
- Plataformas alvo: !`grep -r "BuildTarget\|Platform\|Target" . 2>/dev/null | wc -l` target configurations

## Tarefa

Crie um framework de testes abrangente para desenvolvimento de jogos com validação automatizada, benchmarks de performance, testes multiplataforma e integração contínua.

## Componentes do Framework de Testes

### 1. Infraestrutura de Testes Unitários
- Testes de lógica central do jogo e mecânicas
- Testes baseados em componentes para sistemas modulares
- Sistemas de mock e stub para dependências externas
- Validação de dados e testes de serialização
- Verificação de cálculos matemáticos e algoritmos

### 2. Suite de Testes de Integração
- Testes de carregamento de cenas e transições
- Validação de carregamento e gerenciamento de assets
- Testes de integridade do sistema de save/load
- Funcionalidade de rede e multiplayer
- Testes de integração de features específicas da plataforma

### 3. Performance & Benchmarking
- Testes de estabilidade de taxa de quadros em cenários diversos
- Perfil de uso de memória e detecção de vazamentos
- Benchmarks de tempo de carregamento para diferentes conteúdos
- Testes de stress com alta contagem de entidades
- Validação de performance específica da plataforma

### 4. Testes de Gameplay Automatizados
- Validação de comportamento da IA e testes de regressão
- Simulação de entrada do usuário e verificação de resposta
- Validação de progressão de estado do jogo e checkpoints
- Testes de balanceamento de mecânicas do jogo
- Validação de geração de conteúdo procedural

## Categorias de Testes

### Testes Funcionais
- Validação de mecânicas core do gameplay
- Responsividade e funcionalidade da interface do usuário
- Integração do sistema de áudio e áudio espacial
- Precisão e estabilidade da simulação física
- Tempo de animação e sistema de blending

### Testes de Compatibilidade
- Verificação de build multiplataforma
- Testes de features específicas do dispositivo (mobile, console, VR)
- Diferentes resoluções de tela e aspect ratios
- Escalonamento de capacidade de hardware e adaptação
- Validação de compatibilidade com sistema operacional

### Testes de Regressão
- Testes automatizados para impacto de mudanças de código
- Impacto de modificação de assets na performance do jogo
- Compatibilidade de save file entre versões
- Preservação de funcionalidade de features
- Detecção de regressão de performance

### Testes de Experiência do Usuário
- Validação de features de acessibilidade
- Testes de esquema de controles em diferentes dispositivos de entrada
- Testes de localização e internacionalização
- Validação de fluxo de tutorial e onboarding
- Testes de tratamento de erros e recuperação

## Entregas

1. **Setup do Framework de Testes**
   - Configuração e automação do test runner
   - Sistemas de mock e geração de dados de teste
   - Integração com pipeline de integração contínua
   - Coleta de relatórios de testes e métricas

2. **Implementação da Suite de Testes**
   - Testes unitários para sistemas core do jogo
   - Testes de integração para interações complexas
   - Benchmarks de performance e monitoramento
   - Scripts de validação automatizada de gameplay

3. **Estratégia de Testes de Plataforma**
   - Configurações de testes específicas do dispositivo
   - Testes em cloud e integração com device farm
   - Validação de performance em plataformas alvo
   - Automação de testes de compatibilidade

4. **Monitoramento & Relatórios**
   - Dashboard e visualização de resultados de testes
   - Rastreamento de regressão de performance
   - Análise e relatórios de cobertura de código
   - Investigação automatizada de falhas em testes

## Diretrizes de Implementação

Integre com ferramentas de testes do motor de jogo e estabeleça pipelines CI/CD para testes automatizados. Garanta arquitetura de testes escalável que cresça com a complexidade do projeto e tamanho do time.