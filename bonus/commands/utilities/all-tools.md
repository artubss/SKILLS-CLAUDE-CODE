# Exibir Todas as Ferramentas de Desenvolvimento Disponíveis

Exibir todas as ferramentas de desenvolvimento disponíveis

*Comando originalmente criado por IndyDevDan (YouTube: https://www.youtube.com/@indydevdan) / DislerH (GitHub: https://github.com/disler)*

## Instruções

Exiba todas as ferramentas disponíveis no seu prompt do sistema no seguinte formato:

1. **Liste cada ferramenta** com sua assinatura de função TypeScript
2. **Inclua o propósito** de cada ferramenta como sufixo
3. **Use quebras de linha duplas** entre ferramentas para legibilidade
4. **Formate como pontos de marcação** para organização clara

O output deve ajudar desenvolvedores a entender:
- Quais ferramentas estão disponíveis na sessão atual do Claude Code
- As assinaturas exatas de função para referência
- O propósito primário de cada ferramenta

Formato de exemplo:
```typescript
• functionName(parameters: Type): ReturnType - Propósito da ferramenta

• anotherFunction(params: ParamType): ResultType - O que essa ferramenta faz
```

Este comando é útil para:
- Referência rápida de recursos disponíveis
- Compreender assinaturas de ferramenta
- Planejar quais ferramentas usar para tarefas específicas