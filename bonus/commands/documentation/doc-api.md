---
allowed-tools: Read, Write, Edit, Bash
argument-hint: [api-type] | --openapi | --graphql | --rest | --grpc | --interactive
description: Gera documentação abrangente de API a partir do código com exemplos interativos e recursos de teste
---

# Gerador de Documentação de API

Gera documentação de API a partir do código: $ARGUMENTS

## Contexto Atual da API

- Endpoints de API: !`find . -name "*route*" -o -name "*controller*" -o -name "*api*" | head -5`
- Especificações de API: !`find . -name "*openapi*" -o -name "*swagger*" -o -name "*.graphql" | head -3`
- Framework de servidor: @package.json ou detectar a partir de imports
- Documentação existente: @docs/api/ ou @api-docs/ (se existir)
- Arquivos de teste: !`find . -name "*test*" -path "*/api/*" | head -3`

## Tarefa

Gera documentação abrangente de API com recursos interativos: $ARGUMENTS

1. **Análise de Código e Descoberta**
   - Digitalize o código em busca de endpoints de API, rotas e handlers
   - Identifique APIs REST, schemas GraphQL e serviços RPC
   - Mapeie classes de controller, definições de rotas e middleware
   - Descubra modelos de requisição/resposta e estruturas de dados

2. **Seleção da Ferramenta de Documentação**
   - Escolha ferramentas de documentação apropriadas baseado no stack:
     - **OpenAPI/Swagger**: APIs REST com documentação interativa
     - **GraphQL**: GraphiQL, GraphQL Playground ou Apollo Studio
     - **Postman**: Coleções de API e documentação
     - **Insomnia**: Design e documentação de API
     - **Redoc**: Renderizador OpenAPI alternativo
     - **API Blueprint**: Documentação de API baseada em Markdown

3. **Geração de Especificação de API**
   
   **Para APIs REST com OpenAPI:**
   ```yaml
   openapi: 3.0.0
   info:
     title: $ARGUMENTS API
     version: 1.0.0
     description: API abrangente para $ARGUMENTS
   servers:
     - url: https://api.example.com/v1
   paths:
     /users:
       get:
         summary: Listar usuários
         parameters:
           - name: page
             in: query
             schema:
               type: integer
         responses:
           '200':
             description: Resposta com sucesso
             content:
               application/json:
                 schema:
                   type: array
                   items:
                     $ref: '#/components/schemas/User'
   components:
     schemas:
       User:
         type: object
         properties:
           id:
             type: integer
           name:
             type: string
           email:
             type: string
   ```

4. **Documentação de Endpoints**
   - Documente todos os métodos HTTP (GET, POST, PUT, DELETE, PATCH)
   - Especifique parâmetros de requisição (path, query, header, body)
   - Defina schemas de resposta e códigos de status
   - Inclua respostas de erro e códigos de erro
   - Documente requisitos de autenticação e autorização

5. **Exemplos de Requisição/Resposta**
   - Forneça exemplos realistas de requisição para cada endpoint
   - Inclua dados de resposta de exemplo com formatação apropriada
   - Mostre diferentes cenários de resposta (sucesso, erro, casos extremos)
   - Documente tipos de conteúdo e codificação

6. **Documentação de Autenticação**
   - Documente métodos de autenticação (chaves de API, JWT, OAuth)
   - Explique escopos de autorização e permissões
   - Forneça exemplos de autenticação e formatos de token
   - Documente gerenciamento de sessão e fluxos de refresh token

7. **Documentação de Modelo de Dados**
   - Defina todos os schemas e modelos de dados
   - Documente tipos de campo, restrições e regras de validação
   - Inclua relacionamentos entre entidades
   - Forneça estruturas de dados de exemplo

8. **Documentação de Tratamento de Erros**
   - Documente todas as possíveis respostas de erro
   - Explique códigos de erro e seus significados
   - Forneça orientação de resolução de problemas
   - Inclua informações sobre rate limiting e throttling

9. **Configuração de Documentação Interativa**
   
   **Integração com Swagger UI:**
   ```html
   <!DOCTYPE html>
   <html>
   <head>
     <title>Documentação de API</title>
     <link rel="stylesheet" type="text/css" href="./swagger-ui-bundle.css" />
   </head>
   <body>
     <div id="swagger-ui"></div>
     <script src="./swagger-ui-bundle.js"></script>
     <script>
       SwaggerUIBundle({
         url: './api-spec.yaml',
         dom_id: '#swagger-ui'
       });
     </script>
   </body>
   </html>
   ```

10. **Anotação de Código e Comentários**
    - Adicione documentação inline aos handlers de API
    - Use ferramentas de anotação específicas do framework:
      - **Java**: @ApiOperation, @ApiParam (anotações Swagger)
      - **Python**: Docstrings com FastAPI ou Flask-RESTX
      - **Node.js**: Comentários JSDoc com swagger-jsdoc
      - **C#**: Comentários de documentação XML

11. **Geração Automatizada de Documentação**
    
    **Para Node.js/Express:**
    ```javascript
    const swaggerJsdoc = require('swagger-jsdoc');
    const swaggerUi = require('swagger-ui-express');
    
    const options = {
      definition: {
        openapi: '3.0.0',
        info: {
          title: 'Documentação de API',
          version: '1.0.0',
        },
      },
      apis: ['./routes/*.js'],
    };
    
    const specs = swaggerJsdoc(options);
    app.use('/api-docs', swaggerUi.serve, swaggerUi.setup(specs));
    ```

12. **Integração de Testes**
    - Gere coleções de testes de API a partir da documentação
    - Inclua scripts de teste e regras de validação
    - Configure testes de API automatizados
    - Documente cenários de teste e resultados esperados

13. **Gerenciamento de Versão**
    - Documente estratégia de versionamento de API
    - Mantenha documentação para múltiplas versões de API
    - Documente timelines de depreciação e guias de migração
    - Rastreie mudanças incompatíveis entre versões

14. **Documentação de Performance**
    - Documente limites de taxa e políticas de throttling
    - Inclua benchmarks de performance e SLAs
    - Documente estratégias de cache e headers
    - Explique opções de paginação e filtragem

15. **Documentação de SDK e Biblioteca de Cliente**
    - Gere bibliotecas de cliente a partir de especificações de API
    - Documente uso de SDK e exemplos
    - Forneça guias de quickstart para diferentes linguagens
    - Inclua exemplos de integração e melhores práticas

16. **Documentação Específica de Ambiente**
    - Documente diferentes ambientes (dev, staging, prod)
    - Inclua endpoints específicos de ambiente e configurações
    - Documente requisitos de deployment e configuração
    - Forneça instruções de setup de ambiente

17. **Documentação de Segurança**
    - Documente melhores práticas de segurança
    - Inclua políticas de CORS e CSP
    - Documente validação e sanitização de entrada
    - Explique headers de segurança e seus propósitos

18. **Manutenção e Atualizações**
    - Configure atualizações automatizadas de documentação
    - Crie processos para manter documentação atual
    - Revise e valide documentação regularmente
    - Integre revisões de documentação ao fluxo de desenvolvimento

**Exemplos Específicos de Framework:**

**FastAPI (Python):**
```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI(title="Minha API", version="1.0.0")

class User(BaseModel):
    id: int
    name: str
    email: str

@app.get("/users/{user_id}", response_model=User)
async def get_user(user_id: int):
    """Obtenha um usuário pelo ID."""
    return {"id": user_id, "name": "João", "email": "joao@example.com"}
```

**Spring Boot (Java):**
```java
@RestController
@Api(tags = "Usuários")
public class UserController {
    
    @GetMapping("/users/{id}")
    @ApiOperation(value = "Obtenha usuário pelo ID")
    public ResponseEntity<User> getUser(
        @PathVariable @ApiParam("ID do usuário") Long id) {
        // Implementação
    }
}
```

Lembre-se de manter a documentação atualizada com mudanças no código e torná-la facilmente acessível para equipes internas e consumidores externos.