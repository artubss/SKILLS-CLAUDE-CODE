---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [resource] [method] [flags]
description: Google Workspace Reseller: Gerenciar assinaturas do Workspace.
---

# Google Workspace Reseller

Execute operações do Google Workspace Reseller: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalado
- Autenticação configurada: Execute `gws auth status` para verificar
- Revise `gws reseller --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# reseller (v1)

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para autenticação, flags globais e regras de segurança. Se não existir, execute `gws generate-skills` para criá-lo.

```bash
gws reseller <resource> <method> [flags]
```

## Recursos da API

### customers

  - `get` — Obtém uma conta de cliente. Use esta operação para ver uma conta de cliente já em seu gerenciamento de revenda, ou para ver as informações mínimas da conta de um cliente existente que você não gerencia. Para mais informações sobre a resposta da API para clientes existentes, consulte [recuperar uma conta de cliente](https://developers.google.com/workspace/admin/reseller/v1/how-tos/manage_customers#get_customer).
  - `insert` — Solicita uma nova conta de cliente.
  - `patch` — Atualiza as configurações da conta de um cliente. Este método oferece suporte a semântica de patch. Você não pode atualizar `customerType` pela API Reseller, mas um cliente `"team"` pode verificar seu domínio e se tornar `customerType = "domain"`. Para mais informações, consulte [Verificar seu domínio para desbloquear recursos Essentials](https://support.google.com/a/answer/9122284).
  - `update` — Atualiza as configurações da conta de um cliente. Você não pode atualizar `customerType` pela API Reseller, mas um cliente `"team"` pode verificar seu domínio e se tornar `customerType = "domain"`. Para mais informações, consulte [atualizar as configurações de um cliente](https://developers.google.com/workspace/admin/reseller/v1/how-tos/manage_customers#update_customer).

### resellernotify

  - `getwatchdetails` — Retorna todos os detalhes do watch correspondente ao reseller.
  - `register` — Registra um Reseller para receber notificações.
  - `unregister` — Cancela o registro de um Reseller para receber notificações.

### subscriptions

  - `activate` — Ativa uma assinatura previamente suspensa pelo reseller. Se você não suspendeu a assinatura do cliente e ela está suspensa por outro motivo, como abuso ou aceitação pendente de ToS, esta chamada não reativará a assinatura do cliente.
  - `changePlan` — Atualiza um plano de assinatura. Use este método para atualizar um plano para uma assinatura de período de avaliação de 30 dias ou flexível para um plano de compromisso anual com pagamentos mensais ou anuais. A forma como um plano é atualizado difere dependendo do plano e dos produtos. Para mais informações, consulte a descrição em [gerenciar assinaturas](https://developers.google.com/workspace/admin/reseller/v1/how-tos/manage_subscriptions#update_subscription_plan).
  - `changeRenewalSettings` — Atualiza as configurações de renovação de uma licença de usuário. Isto é aplicável apenas para contas com planos de compromisso anual. Para mais informações, consulte a descrição em [gerenciar assinaturas](https://developers.google.com/workspace/admin/reseller/v1/how-tos/manage_subscriptions#update_renewal).
  - `changeSeats` — Atualiza as configurações de licença de usuário de uma assinatura. Para mais informações sobre como atualizar uma assinatura com plano de compromisso anual ou flexível, consulte [Gerenciar Assinaturas](https://developers.google.com/workspace/admin/reseller/v1/how-tos/manage_subscriptions#update_subscription_seat).
  - `delete` — Cancela, suspende ou transfere uma assinatura para direto.
  - `get` — Obtém uma assinatura específica. O `subscriptionId` pode ser encontrado usando o método [Recuperar todas as assinaturas de reseller](https://developers.google.com/workspace/admin/reseller/v1/how-tos/manage_subscriptions#get_all_subscriptions). Para mais informações sobre como recuperar uma assinatura específica, consulte as informações descritas em [gerenciar assinaturas](https://developers.google.com/workspace/admin/reseller/v1/how-tos/manage_subscriptions#get_subscription).
  - `insert` — Cria ou transfere uma assinatura. Crie uma assinatura para a conta de um cliente que você solicitou usando o método [Solicitar uma nova conta de cliente](https://developers.google.com/workspace/admin/reseller/v1/reference/customers/insert.html).
  - `list` — Lista as assinaturas gerenciadas pelo reseller. A lista pode ser todas as assinaturas, todas as assinaturas de um cliente, ou todas as assinaturas transferíveis de um cliente. Opcionalmente, este método pode filtrar a resposta por um `customerNamePrefix`. Para mais informações, consulte [gerenciar assinaturas](https://developers.google.com/workspace/admin/reseller/v1/how-tos/manage_subscriptions).
  - `startPaidService` — Move imediatamente uma assinatura de período de avaliação gratuito de 30 dias para uma assinatura de serviço pago. Este método é aplicável apenas se um plano de pagamento já tiver sido configurado para a assinatura do período de avaliação de 30 dias. Para mais informações, consulte [gerenciar assinaturas](https://developers.google.com/workspace/admin/reseller/v1/how-tos/manage_subscriptions#paid_service).
  - `suspend` — Suspende uma assinatura ativa. Você pode usar este método para suspender uma assinatura paga que está atualmente no estado `ACTIVE`. * Para assinaturas `FLEXIBLE`, a cobrança é pausada. * Para assinaturas `ANNUAL_MONTHLY_PAY` ou `ANNUAL_YEARLY_PAY`: * Suspender a assinatura não altera a data de renovação originalmente comprometida. * Uma assinatura suspensa não é renovada.

## Descobrindo Comandos

Antes de chamar qualquer método da API, inspecione-o:

```bash
# Navegue pelos recursos e métodos
gws reseller --help

# Inspecione os parâmetros, tipos e padrões necessários de um método
gws schema reseller.<resource>.<method>
```

Use a saída de `gws schema` para construir seus flags `--params` e `--json`.

## Uso

```bash
# Listar recursos e métodos disponíveis
gws reseller --help

# Inspecionar o schema do método antes de chamar
gws schema reseller.<resource>.<method>

# Executar comando com argumentos
gws reseller $ARGUMENTS
```

## Tarefa

Execute a operação Reseller solicitada: $ARGUMENTS

1. **Verificar Pré-requisitos**
   - Verificar se `gws` está instalado: `gws --version`
   - Verificar autenticação: `gws auth status`
   - Revisar comandos disponíveis: `gws reseller --help`

2. **Inspecionar o Schema do Método**
   - Antes de chamar qualquer método, inspecione seus parâmetros
   - Use `gws schema` para entender os campos obrigatórios
   - Revise os tipos de parâmetro e restrições

3. **Executar Operação**
   - Construir comando com flags apropriados
   - Use `--params` para parâmetros de query/path
   - Use `--json` para o corpo da requisição
   - Lidar com paginação com `--max-results` ou `--page-token`

4. **Tratamento de Erros**
   - Verificar a saída do comando em busca de erros
   - Revisar quotas e limites de taxa de API
   - Lidar com problemas de autenticação
   - Tentar novamente falhas transitórias

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-reseller`