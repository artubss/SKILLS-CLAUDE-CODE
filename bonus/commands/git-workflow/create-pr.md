# Comando Criar Pull Request

Cria uma nova branch, faz commit das alterações e submete uma pull request.

## Comportamento
- Cria uma nova branch com base nas alterações atuais
- Formata arquivos modificados usando Biome
- Analisa alterações e divide automaticamente em commits lógicos quando apropriado
- Cada commit se concentra em uma única alteração lógica ou feature
- Cria mensagens de commit descritivas para cada unidade lógica
- Envia a branch para o repositório remoto
- Cria pull request com resumo apropriado e plano de testes

## Diretrizes para Divisão Automática de Commits
- Divida commits por feature, componente ou responsabilidade
- Mantenha alterações de arquivos relacionados no mesmo commit
- Separe refatoração de adições de features
- Garanta que cada commit possa ser entendido independentemente
- Múltiplas alterações não relacionadas devem ser divididas em commits separados