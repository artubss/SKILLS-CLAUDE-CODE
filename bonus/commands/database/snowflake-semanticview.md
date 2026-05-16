---
allowed-tools: Bash, Read
description: Criar, alterar e validar visualizações semânticas do Snowflake usando Snowflake CLI (snow). Use quando solicitado criar ou resolver problemas de visualizações semânticas/definições de camada semântica com CREATE/ALTER SEMANTIC VIEW, validar DDL de visualização semântica contra Snowflake via CLI, ou guiar instalação e configuração de conexão do Snowflake CLI.
---

# Visualizações Semânticas do Snowflake

## Configuração Única

- Verifique a instalação do Snowflake CLI abrindo um novo terminal e executando `snow --help`.
- Se o Snowflake CLI estiver faltando ou o usuário não conseguir instalá-lo, dirija-o para https://docs.snowflake.com/en/developer-guide/snowflake-cli/installation/installation.
- Configure uma conexão Snowflake com `snow connection add` conforme https://docs.snowflake.com/en/developer-guide/snowflake-cli/connecting/configure-connections#add-a-connection.
- Use a conexão configurada para todas as etapas de validação e execução.

## Fluxo de Trabalho Para Cada Solicitação de Visualização Semântica

1. Confirme o banco de dados de destino, schema, função, warehouse e nome final da visualização semântica.
2. Confirme que o modelo segue um esquema em estrela (fatos com dimensões conformadas).
3. Elabore o DDL da visualização semântica usando a sintaxe oficial:
   - https://docs.snowflake.com/en/sql-reference/sql/create-semantic-view
4. Preencha sinônimos e comentários para cada dimensão, fato e métrica:
   - Leia primeiro os comentários de tabela/visualização/coluna do Snowflake (fonte preferida):
     - https://docs.snowflake.com/en/sql-reference/sql/comment
   - Se os comentários ou sinônimos estiverem faltando, pergunte se você pode criá-los, se o usuário deseja fornecer texto ou se você deve elaborar sugestões para aprovação.
5. Crie um nome de validação temporário (por exemplo, acrescente `__tmp_validate`) mantendo o mesmo banco de dados e schema.
6. Sempre valide enviando o DDL para Snowflake via Snowflake CLI antes de finalizar:
   - Use `snow sql` para executar a instrução com a conexão configurada.
   - Se as flags diferirem por versão, verifique `snow sql --help` e use a opção de conexão mostrada lá.
7. Se a validação falhar, itere no DDL e re-execute a etapa de validação até que tenha sucesso.
8. Aplique o DDL final (criar ou alterar) usando o nome da visualização semântica real.
9. Limpe qualquer visualização semântica temporária criada durante a validação.

## Sinônimos e Comentários (Obrigatórios)

- Use a sintaxe da visualização semântica para sinônimos e comentários:

```
WITH SYNONYMS [ = ] ( 'synonym' [ , ... ] )
COMMENT = 'comment_about_dim_fact_or_metric'
```

- Trate sinônimos como apenas informativos; não os use para referenciar dimensões, fatos ou métricas em outro lugar.
- Use comentários Snowflake como fonte preferida e primeira para sinônimos e comentários:
  - https://docs.snowflake.com/en/sql-reference/sql/comment
- Se os comentários Snowflake estiverem faltando, pergunte se você pode criá-los, se o usuário deseja fornecer texto ou se você deve elaborar sugestões para aprovação.
- Não invente sinônimos ou comentários sem aprovação do usuário.

## Padrão de Validação (Obrigatório)

- Nunca pule a validação. Sempre execute o DDL contra Snowflake com Snowflake CLI antes de apresentá-lo como final.
- Prefira um nome temporário para validação a fim de evitar sobrescrever a visualização real.

## Exemplo de Validação CLI (Template)

```bash
# Substitua os placeholders por valores reais.
snow sql -q "<CREATE OR ALTER SEMANTIC VIEW ...>" --connection <connection_name>
```

Se a CLI usar uma flag de conexão diferente em sua versão, execute:

```bash
snow sql --help
```

## Observações

- Trate instalação e configuração de conexão como etapas únicas, mas confirme que estão feitas antes da primeira validação.
- Mantenha a definição final da visualização semântica idêntica à definição temporária validada, exceto pelo nome.
- Não omita sinônimos ou comentários; considere-os obrigatórios para completude, mesmo que opcionais na sintaxe.