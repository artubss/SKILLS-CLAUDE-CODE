---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [output-format] | --swagger-ui | --redoc | --postman | --insomnia | --multi-format
description: Gera automaticamente documentação de referência de API em múltiplos formatos e deploy automatizado
---

# Gerador Automatizado de Documentação de API

Gera automaticamente documentação de referência de API: $ARGUMENTS

## Infraestrutura de API Atual

- Anotações de código: !`grep -r "@api\|@swagger\|@doc" src/ 2>/dev/null | wc -l` anotações encontradas
- Framework de API: @package.json ou detectar a partir de imports
- Especificações existentes: !`find . -name "*spec*.yaml" -o -name "*spec*.json" | head -3`
- Ferramentas de documentação: !`grep -E "swagger|redoc|postman" package.json 2>/dev/null || echo "Nenhuma detectada"`
- Pipeline CI/CD: @.github/workflows/ (se existir)

## Tarefa

Configure geração automatizada de documentação de API com ferramentas modernas:

1. **Análise da Estratégia de Documentação de API**
   - Analise a estrutura atual de API e endpoints
   - Identifique requisitos de documentação (REST, GraphQL, gRPC, etc.)
   - Avalie anotações de código e documentação existentes
   - Determine formatos de saída de documentação e requisitos de hospedagem
   - Planeje automação de documentação e estratégia de manutenção

2. **Seleção de Ferramentas de Documentação**
   - Escolha ferramentas apropriadas de documentação de API:
     - **OpenAPI/Swagger**: Documentação de API REST com Swagger UI
     - **Redoc**: Renderizador moderno de documentação OpenAPI
     - **GraphQL**: GraphiQL, Apollo Studio, GraphQL Playground
     - **Postman**: Documentação de API com collections
     - **Insomnia**: Documentação e testes de API
     - **API Blueprint**: Documentação de API baseada em Markdown
     - **JSDoc/TSDoc**: Geração de documentação primeiro o código
   - Considere fatores: tipo de API, fluxo de trabalho do time, hospedagem, interatividade

3. **Anotação de Código e Definição de Schema**
   - Adicione anotações abrangentes para endpoints de API
   - Defina schemas de requisição/resposta e modelos de dados
   - Adicione descrições de parâmetros e regras de validação
   - Documente requisitos de autenticação e autorização
   - Adicione exemplos de requisições e respostas

4. **Geração de Especificação de API**
   - Configure geração automatizada de especificação de API a partir de código
   - Configure geração de especificação OpenAPI/Swagger
   - Configure validação de schema e verificação de consistência
   - Configure geração de versionamento de API e changelog
   - Configure gerenciamento de arquivo de especificação e controle de versão

5. **Configuração de Documentação Interativa**
   - Configure documentação de API interativa com funcionalidade try-it-out
   - Configure testes de API e execução de exemplos
   - Configure manipulação de autenticação na documentação
   - Configure validação de requisição/resposta e exemplos
   - Configure categorização e organização de endpoint de API

6. **Aprimoramento de Conteúdo de Documentação**
   - Adicione guias e tutoriais abrangentes de API
   - Crie documentação de autenticação e autorização
   - Adicione documentação de tratamento de erro e códigos de status
   - Crie documentação de SDK e bibliotecas cliente
   - Adicione diretrizes de limite de taxa e uso

7. **Hospedagem e Deploy de Documentação**
   - Configure hospedagem e deploy de documentação
   - Configure geração e estilo de website de documentação
   - Configure domínio personalizado e configuração SSL
   - Configure busca e navegação de documentação
   - Configure analytics e rastreamento de uso de documentação

8. **Automação e Integração CI/CD**
   - Configure geração automatizada de documentação em pipeline CI/CD
   - Configure automação de deploy de documentação
   - Configure validação e verificações de qualidade de documentação
   - Configure detecção de mudanças de documentação e notificações
   - Configure testes e validação de links de documentação

9. **Geração de Documentação em Múltiplos Formatos**
   - Gere documentação em múltiplos formatos (HTML, PDF, Markdown)
   - Configure pacotes de documentação para download
   - Configure acesso a documentação offline
   - Configure API de documentação para acesso programático
   - Configure sindicação e distribuição de documentação

10. **Manutenção e Garantia de Qualidade**
    - Configure monitoramento e validação de qualidade de documentação
    - Configure fluxos de trabalho de feedback e melhoria de documentação
    - Configure analytics e métricas de uso de documentação
    - Crie procedimentos de manutenção de documentação e diretrizes
    - Treine o time em melhores práticas e ferramentas de documentação
    - Configure processos de revisão e aprovação de documentação