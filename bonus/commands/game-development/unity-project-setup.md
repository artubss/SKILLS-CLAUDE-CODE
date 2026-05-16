---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [nome-do-projeto] | --2d | --3d | --mobile | --vr | --console
description: Use PROATIVAMENTE para configurar projetos profissionais de desenvolvimento de jogos Unity com estrutura padrão da indústria, pacotes essenciais e configurações otimizadas por plataforma
---

# Configuração de Projeto Unity & Ambiente de Desenvolvimento

Inicialize um projeto profissional de desenvolvimento de jogos Unity: $ARGUMENTS

## Ambiente Unity Atual

- Versão Unity: !`unity-editor --version 2>/dev/null || echo "Unity Editor not found"`
- Diretório atual: !`pwd`
- Pacotes disponíveis: !`find . -name "*.unitypackage" 2>/dev/null | wc -l` pacotes Unity
- Status Git: !`git status --porcelain 2>/dev/null | wc -l` alterações não commitadas
- Informações do sistema: !`system_profiler SPSoftwareDataType | grep "System Version" 2>/dev/null || uname -a`

## Tarefa

Configure um projeto Unity completo com ambiente de desenvolvimento profissional e otimizações específicas de plataforma.

## O que é criado:

### Estrutura do Projeto
```
Assets/
├── _Project/
│   ├── Scripts/
│   │   ├── Managers/
│   │   ├── Player/
│   │   ├── UI/
│   │   ├── Gameplay/
│   │   └── Utilities/
│   ├── Art/
│   │   ├── Textures/
│   │   ├── Materials/
│   │   ├── Models/
│   │   └── Animations/
│   ├── Audio/
│   │   ├── Music/
│   │   ├── SFX/
│   │   └── Voice/
│   ├── Prefabs/
│   │   ├── Characters/
│   │   ├── Environment/
│   │   ├── UI/
│   │   └── Effects/
│   ├── Scenes/
│   │   ├── Development/
│   │   ├── Production/
│   │   └── Testing/
│   ├── Settings/
│   │   ├── Input/
│   │   ├── Rendering/
│   │   └── Audio/
│   └── Resources/
├── Plugins/
├── StreamingAssets/
└── Editor/
    ├── Scripts/
    └── Resources/
```

### Pacotes Essenciais
- Universal Render Pipeline (URP)
- Input System
- Cinemachine
- ProBuilder
- Timeline
- Addressables
- Unity Analytics
- Version Control (se disponível)

### Configurações de Projeto
- Configurações de qualidade otimizadas para plataformas alvo
- Configuração do sistema de entrada
- Configurações de física
- Configurações de tempo e renderização
- Configurações de build para múltiplas plataformas

### Ferramentas de Desenvolvimento
- Regras de formatação de código (.editorconfig)
- Configuração Git com .gitignore otimizado para Unity
- Arquivos de definição de assembly para melhor compilação
- Scripts customizados de editor para melhoria de workflow

### Configuração de Controle de Versão
- Inicialização de repositório Git
- .gitignore específico para Unity
- Configuração de LFS para assets grandes
- Documentação de estratégia de branching

## Uso:

```bash
npx claude-code-templates@latest --command unity-project-setup
```

## Opções Interativas:

1. **Seleção de Tipo de Projeto**
   - Jogo 2D
   - Jogo 3D
   - Jogo Mobile
   - Jogo VR/AR
   - Híbrido (2D/3D)

2. **Plataformas Alvo**
   - PC (Windows/Mac/Linux)
   - Mobile (iOS/Android)
   - Console (PlayStation/Xbox/Nintendo)
   - WebGL
   - VR (Oculus/SteamVR)

3. **Controle de Versão**
   - Git
   - Plastic SCM
   - Perforce
   - Nenhum

4. **Pacotes Adicionais**
   - TextMeshPro
   - Post Processing
   - Unity Ads
   - Unity Analytics
   - Unity Cloud Build
   - Seleção customizada de pacotes

## Arquivos Gerados:

### Scripts Principais
- `GameManager.cs` - Controlador principal do jogo
- `SceneLoader.cs` - Sistema de gerenciamento de cenas
- `AudioManager.cs` - Controlador de sistema de áudio
- `InputManager.cs` - Sistema de tratamento de entrada
- `UIManager.cs` - Gerenciador de sistema UI
- `SaveSystem.cs` - Funcionalidade de salvar/carregar

### Ferramentas de Editor
- `ProjectSetupWindow.cs` - Janela customizada de editor
- `SceneQuickStart.cs` - Automação de configuração de cenas
- `AssetValidator.cs` - Ferramentas de validação de assets
- `BuildAutomation.cs` - Helpers de pipeline de build

### Arquivos de Configuração
- `ProjectSettings.asset` - Configurações de projeto otimizadas
- `QualitySettings.asset` - Níveis de qualidade multi-plataforma
- `InputActions.inputactions` - Configuração do sistema de entrada
- `AssemblyDefinitions` - Configuração de compilação modular

### Documentação
- `README.md` - Visão geral e instruções de configuração do projeto
- `CONTRIBUTING.md` - Diretrizes de desenvolvimento
- `CHANGELOG.md` - Template de histórico de versões
- `API_REFERENCE.md` - Template de documentação de código

## Checklist Pós-Configuração:

- [ ] Revise e ajuste as configurações de qualidade para as plataformas alvo
- [ ] Configure as ações de entrada para os controles do seu jogo
- [ ] Configure as configurações de build para todas as plataformas alvo
- [ ] Revise a estrutura de pastas e renomeie conforme necessário
- [ ] Configure o controle de versão e faça o commit inicial
- [ ] Configure integração contínua se necessário
- [ ] Configure analytics e relatório de falhas
- [ ] Revise e customize os padrões de codificação

## Configurações Específicas de Plataforma:

### Mobile
- Configuração de entrada por toque
- Configurações de otimização de desempenho
- Otimização de uso de bateria
- Configuração de envio para app store

### PC
- Suporte multi-resolução
- Configuração de entrada teclado/mouse
- Template de menu de opções gráficas
- Configurações de build Windows/Mac/Linux

### Console
- Mapeamento de entrada específico da plataforma
- Configuração de integração de conquistas/troféus
- Configuração de serviços online
- Templates de requisitos de certificação

Este comando cria uma estrutura de projeto Unity pronta para produção que escala de protótipo a jogo lançado, seguindo as melhores práticas da indústria e padrões recomendados pela Unity.