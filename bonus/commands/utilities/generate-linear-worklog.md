# Gerar Log de Trabalho Linear

Você está encarregado de gerar um comentário técnico de log de trabalho para um issue Linear baseado em commits git recentes.

## Instruções

1. **Verificar Disponibilidade do Linear MCP**
   - Verificar se as ferramentas Linear MCP estão disponíveis (funções mcp__linear__*)
   - Se Linear MCP não estiver instalado, informar o usuário para instalá-lo e fornecer instruções de instalação
   - Não prosseguir com a geração do log de trabalho se Linear MCP não estiver disponível

2. **Verificar Log de Trabalho Existente**
   - Usar Linear MCP para obter comentários existentes no issue
   - Procurar por comentários com a data de hoje no formato "## Trabalho Concluído [DATA_DE_HOJE]"
   - Se encontrado, anotar o conteúdo existente para adicionar/atualizar em vez de duplicar

3. **Extrair Informações do Git**
   - Obter o nome do branch atual
   - Obter commits recentes no branch atual (últimos 10 commits)
   - Obter commits que estão no branch atual mas não no branch main
   - Para cada commit relevante, obter informações detalhadas incluindo mudanças de arquivos e contagens de linhas
   - Focar em commits desde a última atualização do log de trabalho (se houver)

4. **Gerar Conteúdo do Log de Trabalho**
   - Usar linguagem técnica seca sem adjetivos ou emojis
   - Focar em detalhes de implementação factuais
   - Estruturar o log com data, branch e informações de commit
   - Incluir métricas quantitativas (contagem de arquivos, contagem de linhas) quando relevante
   - Evitar comentários subjetivos ou linguagem promocional

5. **Lidar com Log de Trabalho Existente**
   - Se nenhum log de trabalho existir para hoje: Criar novo comentário
   - Se log de trabalho existir para hoje: Substituir o comentário existente com conteúdo atualizado incluindo todo o trabalho de hoje
   - Garantir ordem cronológica dos commits
   - Incluir trabalho anterior e novo concluído hoje

6. **Estrutura de Formatação**
   ```
   ## Trabalho Concluído [DATA_DE_HOJE]

   ### Branch: [nome-do-branch-atual]

   **Commit [hash-curto]: [Título do Commit]**
   - [Detalhe técnico 1]
   - [Detalhe técnico 2]
   - [Contagem de linhas] linhas de código em [contagem de arquivos] arquivos

   [Commits adicionais em ordem cronológica]

   ### [Seção de Status]
   - [Status atual da infraestrutura/testes]
   - [O que está agora disponível/pronto]
   ```

7. **Postar no Linear**
   - Usar a integração Linear MCP para criar ou atualizar o comentário
   - Postar o log de trabalho formatado no issue Linear especificado
   - Se atualizando, substituir todo o comentário de log de trabalho existente
   - Confirmar postagem bem-sucedida

## Comandos Git para Usar
- `git branch --show-current` - Obter branch atual
- `git log --oneline -10` - Obter commits recentes
- `git log main..HEAD --oneline` - Obter commits específicos do branch
- `git show --stat [commit-hash]` - Obter informações detalhadas do commit
- `git log --since="[data-de-hoje]" --pretty=format:"%h %ad %s" --date=short` - Obter commits de hoje

## Diretrizes de Conteúdo
- Incluir hashes de commit e títulos descritivos
- Fornecer implementações técnicas específicas
- Incluir contagens de arquivos e linhas para mudanças significativas
- Manter formatação consistente
- Focar em realizações técnicas
- Incluir resumo de status atual
- Sem emojis ou caracteres especiais

## Tratamento de Erros
- Verificar se o cliente Linear MCP está disponível antes de prosseguir
- Se Linear MCP não estiver disponível, exibir instruções de instalação:
  ```
  Cliente Linear MCP não está instalado. Para instalá-lo:
  
  1. Instalar o servidor Linear MCP:
     npm install -g @modelcontextprotocol/server-linear
  
  2. Adicionar Linear MCP à sua configuração Claude:
     Adicione o seguinte às suas configurações MCP do Claude:
     {
       "mcpServers": {
         "linear": {
           "command": "npx",
           "args": ["@modelcontextprotocol/server-linear"],
           "env": {
             "LINEAR_API_KEY": "sua_chave_api_linear_aqui"
           }
         }
       }
     }
  
  3. Reiniciar Claude Code
  4. Obter sua chave API Linear em: https://linear.app/settings/api
  ```
- Validar que o ID do ticket Linear existe
- Lidar com casos onde nenhum commit recente é encontrado
- Fornecer mensagens de erro claras para falhas de operação git
- Confirmar postagem bem-sucedida do comentário

## Exemplo de Uso
Quando invocado com `/generate-linear-worklog BLA2-2`, o comando deve:
1. Analisar commits git no branch atual
2. Gerar um log de trabalho estruturado
3. Postar o comentário no issue Linear BLA2-2
4. Confirmar postagem bem-sucedida