---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [api-version] | --v1 | --v2 | --graphql-hybrid | --openapi
description: Projetar arquitetura de API RESTful com endpoints abrangentes, autenticação e documentação
---

# Projetar API REST

Projetar arquitetura RESTful abrangente: **$ARGUMENTS**

## Estado Atual da Aplicação

- Detecção de framework: @package.json ou @requirements.txt (Express, FastAPI, Spring Boot, etc.)
- API existente: !`grep -r "route\|endpoint\|@app\\.route" src/ 2>/dev/null | wc -l` rotas encontradas
- Autenticação: !`grep -r "auth\|jwt\|session" src/ 2>/dev/null | wc -l` componentes de autenticação
- Documentação: @swagger.yaml ou @openapi.json (se existir)

## Tarefa

Projetar API RESTful completa com melhores práticas da indústria e funcionalidade abrangente:

**Versão da API**: Use $ARGUMENTS para especificar versão da API, abordagem híbrida GraphQL ou especificação OpenAPI

**Arquitetura da API**:
1. **Design de Recursos** - Endpoints RESTful, métodos HTTP, estrutura de URL, relacionamentos entre recursos
2. **Modelos de Requisição/Resposta** - Validação de dados, serialização, tratamento de erros, códigos de status
3. **Autenticação & Autorização** - JWT, OAuth, RBAC, API keys, rate limiting
4. **Documentação da API** - Especificações OpenAPI/Swagger, documentação interativa, exemplos de código
5. **Estratégia de Versionamento** - Versionamento por URL, header ou content-type
6. **Performance & Segurança** - Cache, paginação, CORS, validação de entrada, prevenção de SQL injection

**Recursos Avançados**: Capacidades em tempo real, upload de arquivos, operações em lote, webhooks e integração de monitoramento.

**Conformidade com Padrões**: Seguir princípios REST, especificações HTTP e melhores práticas de design de API.

**Saída**: Especificação completa de API com endpoints, autenticação, validação, documentação e SDKs cliente.