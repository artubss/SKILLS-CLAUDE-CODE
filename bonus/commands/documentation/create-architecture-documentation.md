---
allowed-tools: Read, Write, Edit, Bash
argument-hint: "[framework] | --c4-model | --arc42 | --adr | --plantuml | --full-suite"
description: Gere documentação de arquitetura abrangente com diagramas, ADRs e visualização interativa
---

# Gerador de Documentação de Arquitetura

Gere documentação de arquitetura abrangente: $ARGUMENTS

## Contexto de Arquitetura Atual

- Estrutura do projeto: !`find . -type f -name "*.json" -o -name "*.yaml" -o -name "*.toml" | head -5`
- Documentação existente: @docs/ ou @README.md (se existir)
- Arquivos de arquitetura: !`find . -name "*architecture*" -o -name "*design*" -o -name "*.puml" | head -3`
- Serviços/containers: @docker-compose.yml ou @k8s/ (se existir)
- Definições de API: !`find . -name "*api*" -o -name "*openapi*" -o -name "*swagger*" | head -3`

## Tarefa

Gere documentação de arquitetura abrangente com ferramentas modernas e melhores práticas:

1. **Análise e Descoberta da Arquitetura**
   - Analise a arquitetura atual do sistema e relacionamentos entre componentes
   - Identifique padrões arquiteturais principais e decisões de design
   - Documente limites do sistema, interfaces e dependências
   - Avalie fluxos de dados e padrões de comunicação
   - Identifique débito arquitetural e oportunidades de melhoria

2. **Framework de Documentação de Arquitetura**
   - Escolha framework e ferramentas de documentação apropriadas:
     - **C4 Model**: Diagramas de Contexto, Containers, Componentes, Código
     - **Arc42**: Template abrangente de documentação de arquitetura
     - **Architecture Decision Records (ADRs)**: Documentação de decisões
     - **PlantUML/Mermaid**: Documentação diagrama-como-código
     - **Structurizr**: Ferramentas e visualização do modelo C4
     - **Draw.io/Lucidchart**: Ferramentas de diagramação visual

3. **Documentação de Contexto do Sistema**
   - Crie diagramas de contexto de alto nível do sistema
   - Documente sistemas externos e integrações
   - Defina limites e responsabilidades do sistema
   - Documente personas de usuários e stakeholders
   - Crie visão geral do cenário e ecossistema do sistema

4. **Arquitetura de Containers e Serviços**
   - Documente arquitetura de containers/serviços e visão de deployment
   - Crie mapas de dependência de serviços e padrões de comunicação
   - Documente arquitetura de deployment e infraestrutura
   - Defina limites de serviço e contratos de API
   - Documente persistência de dados e arquitetura de armazenamento

5. **Documentação de Componentes e Módulos**
   - Crie diagramas de arquitetura de componentes detalhados
   - Documente estrutura interna de módulos e relacionamentos
   - Defina responsabilidades e interfaces de componentes
   - Documente padrões de design e estilos arquiteturais
   - Crie documentação de organização de código e estrutura de pacotes

6. **Documentação de Arquitetura de Dados**
   - Documente modelos de dados e esquemas de banco de dados
   - Crie diagramas de fluxo de dados e pipelines de processamento
   - Documente estratégias e tecnologias de armazenamento de dados
   - Defina governança de dados e gerenciamento de ciclo de vida
   - Crie documentação de integração e sincronização de dados

7. **Arquitetura de Segurança e Conformidade**
   - Documente arquitetura de segurança e modelo de ameaças
   - Crie diagramas de fluxo de autenticação e autorização
   - Documente requisitos e controles de conformidade
   - Defina limites de segurança e zonas de confiança
   - Crie documentação de resposta a incidentes e monitoramento de segurança

8. **Atributos de Qualidade e Preocupações Transversais**
   - Documente características de desempenho e padrões de escalabilidade
   - Crie documentação de arquitetura de confiabilidade e disponibilidade
   - Documente arquitetura de monitoramento e observabilidade
   - Defina estratégias de manutenibilidade e evolução
   - Crie documentação de recuperação de desastres e continuidade de negócios

9. **Architecture Decision Records (ADRs)**
   - Crie template abrangente de ADR e processo
   - Documente decisões arquiteturais históricas e fundamentação
   - Crie processo de rastreamento e revisão de decisões
   - Documente trade-offs e alternativas consideradas
   - Configure procedimentos de manutenção e evolução de ADRs

10. **Automação e Manutenção de Documentação**
    - Configure geração automática de diagramas a partir de anotações de código
    - Configure pipeline de documentação e automação de publicação
    - Configure validação de documentação e verificação de consistência
    - Crie processo de revisão e aprovação de documentação
    - Treine equipe em práticas e ferramentas de documentação de arquitetura
    - Configure versionamento de documentação e gerenciamento de mudanças