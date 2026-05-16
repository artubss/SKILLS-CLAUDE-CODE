---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [tipo-perfil] | --developer | --designer | --devops | --comprehensive | --interactive
description: Cria guia abrangente de integração de desenvolvedores com setup de ambiente, workflows e tutoriais interativos
---

# Gerador de Guia de Integração de Desenvolvedores

Cria guia de integração de desenvolvedores: $ARGUMENTS

## Contexto Atual da Equipe

- Setup do projeto: @package.json ou @requirements.txt ou @Cargo.toml (detecta tech stack)
- Documentação existente: @docs/ ou @README.md (se existir)
- Ferramentas de desenvolvimento: !`find . -name ".env*" -o -name "docker-compose.yml" -o -name "Makefile" | head -3`
- Estrutura da equipe: @CODEOWNERS ou @.github/ (se existir)
- Setup de CI/CD: !`find .github/workflows -name "*.yml" 2>/dev/null | head -3`

## Tarefa

Cria experiência abrangente de integração personalizada para perfil e necessidades do projeto:

1. **Análise de Requisitos de Integração**
   - Analisa estrutura atual da equipe e requisitos de habilidades
   - Identifica áreas-chave de conhecimento e objetivos de aprendizado
   - Avalia desafios e dores atuais na integração
   - Define cronograma de integração e expectativas de marcos
   - Documenta requisitos e responsabilidades específicos do perfil

2. **Guia de Setup do Ambiente de Desenvolvimento**
   - Cria instruções abrangentes de setup do ambiente de desenvolvimento
   - Documenta ferramentas, software e requisitos de sistema necessários
   - Fornece guias passo a passo de instalação e configuração
   - Cria procedimentos de validação e resolução de problemas do ambiente
   - Configura scripts e ferramentas de setup automático do ambiente

3. **Visão Geral do Projeto e Base de Código**
   - Cria visão geral de alto nível do projeto e contexto de negócio
   - Documenta arquitetura de sistemas e stack de tecnologia
   - Fornece guia de estrutura e organização da base de código
   - Cria diretrizes de navegação e exploração do código
   - Documenta módulos, bibliotecas e frameworks principais

4. **Documentação de Workflow de Desenvolvimento**
   - Documenta workflows de controle de versão e estratégias de branching
   - Cria guia de processo de code review e padrões de qualidade
   - Documenta práticas e requisitos de testes
   - Fornece visão geral do processo de deployment e release
   - Cria guia de workflow de rastreamento de issues e gestão de projetos

5. **Comunicação e Colaboração em Equipe**
   - Documenta canais de comunicação e protocolos da equipe
   - Cria cronogramas de reunião e diretrizes de participação
   - Fornece informações de contato da equipe e organograma
   - Documenta ferramentas de colaboração e procedimentos de acesso
   - Cria procedimentos de escalação e contatos de suporte

6. **Recursos de Aprendizado e Materiais de Treinamento**
   - Curadoria de recursos de aprendizado para tecnologias específicas do projeto
   - Cria tutoriais práticos e exercícios de codificação
   - Fornece links para documentação, wikis e bases de conhecimento
   - Cria tutoriais em vídeo e gravações de tela
   - Configura procedimentos de mentoria e sistema buddy

7. **Primeiras Tarefas e Marcos**
   - Cria atribuições de tarefas com dificuldade progressiva
   - Define marcos de aprendizado e checkpoints
   - Fornece "boas primeiras issues" e projetos iniciantes
   - Cria desafios e exercícios de codificação práticos
   - Configura oportunidades de pair programming e shadowing

8. **Treinamento de Segurança e Conformidade**
   - Documenta políticas de segurança e controles de acesso
   - Cria diretrizes de manipulação de dados e privacidade
   - Fornece treinamento de conformidade e requisitos de certificação
   - Documenta procedimentos de resposta a incidentes e segurança
   - Cria diretrizes de melhores práticas de segurança

9. **Acesso a Ferramentas e Recursos**
   - Documenta contas necessárias e solicitações de acesso
   - Cria guias de setup e uso específicos de ferramentas
   - Fornece informações de licenças e subscrições
   - Documenta procedimentos de acesso VPN e rede
   - Cria guias de resolução de problemas para problemas de acesso comuns

10. **Feedback e Melhoria Contínua**
    - Cria processo de coleta de feedback de integração
    - Configura check-ins regulares e revisões de progresso
    - Documenta perguntas frequentes e seção FAQ
    - Cria métricas de integração e rastreamento de sucesso
    - Estabelece procedimentos de manutenção e atualização do guia
    - Configura monitoramento de sucesso e sistemas de suporte a novos colaboradores