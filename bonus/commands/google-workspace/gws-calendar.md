---
allowed-tools: Bash, Read, Write, Edit
argument-hint: [recurso] [método] [sinalizadores]
description: Google Calendar: Gerenciar calendários e eventos.
---

# Google Workspace Calendar

Execute operações do Google Workspace Calendar: $ARGUMENTS

## Pré-requisitos

- Google Workspace CLI (`gws`) deve estar instalado
- Autenticação configurada: Execute `gws auth status` para verificar
- Consulte `gws calendar --help` para todos os comandos disponíveis

## Recursos e Métodos Disponíveis

# calendar (v3)

> **PRÉ-REQUISITO:** Leia `../gws-shared/SKILL.md` para autenticação, sinalizadores globais e regras de segurança. Se não encontrado, execute `gws generate-skills` para criá-lo.

```bash
gws calendar <recurso> <método> [sinalizadores]
```

## Comandos Auxiliares

| Comando | Descrição |
|---------|------------|
| [`+insert`](../gws-calendar-insert/SKILL.md) | criar um novo evento |
| [`+agenda`](../gws-calendar-agenda/SKILL.md) | Mostrar eventos próximos em todos os calendários |

## Recursos da API

### acl

  - `delete` — Deleta uma regra de controle de acesso.
  - `get` — Retorna uma regra de controle de acesso.
  - `insert` — Cria uma regra de controle de acesso.
  - `list` — Retorna as regras na lista de controle de acesso do calendário.
  - `patch` — Atualiza uma regra de controle de acesso. Este método oferece suporte a semântica de patch.
  - `update` — Atualiza uma regra de controle de acesso.
  - `watch` — Observar mudanças nos recursos de ACL.

### calendarList

  - `delete` — Remove um calendário da lista de calendários do usuário.
  - `get` — Retorna um calendário da lista de calendários do usuário.
  - `insert` — Insere um calendário existente na lista de calendários do usuário.
  - `list` — Retorna os calendários na lista de calendários do usuário.
  - `patch` — Atualiza um calendário existente na lista de calendários do usuário. Este método oferece suporte a semântica de patch.
  - `update` — Atualiza um calendário existente na lista de calendários do usuário.
  - `watch` — Observar mudanças nos recursos de CalendarList.

### calendars

  - `clear` — Limpa um calendário primário. Esta operação deleta todos os eventos associados ao calendário primário de uma conta.
  - `delete` — Deleta um calendário secundário. Use calendars.clear para limpar todos os eventos em calendários primários.
  - `get` — Retorna metadados para um calendário.
  - `insert` — Cria um calendário secundário.
O usuário autenticado da solicitação se torna o proprietário dos dados do novo calendário.

Observação: Recomendamos autenticar como o proprietário de dados pretendido do calendário. Você pode usar delegação de autoridade em toda a empresa para permitir que aplicativos ajam em nome de um usuário específico. Não use uma conta de serviço para autenticação. Se usar uma conta de serviço para autenticação, a conta de serviço se torna o proprietário dos dados, o que pode levar a comportamento inesperado.
  - `patch` — Atualiza metadados para um calendário. Este método oferece suporte a semântica de patch.
  - `update` — Atualiza metadados para um calendário.

### channels

  - `stop` — Para de observar recursos através deste canal

### colors

  - `get` — Retorna as definições de cor para calendários e eventos.

### events

  - `delete` — Deleta um evento.
  - `get` — Retorna um evento com base em sua ID do Google Calendar. Para recuperar um evento usando sua ID iCalendar, chame o método events.list usando o parâmetro iCalUID.
  - `import` — Importa um evento. Esta operação é usada para adicionar uma cópia privada de um evento existente a um calendário. Apenas eventos com um eventType padrão podem ser importados.
Comportamento deprecado: Se um evento não-padrão for importado, seu tipo será alterado para padrão e quaisquer propriedades específicas de tipo de evento que ele possa ter serão removidas.
  - `insert` — Cria um evento.
  - `instances` — Retorna instâncias do evento recorrente especificado.
  - `list` — Retorna eventos no calendário especificado.
  - `move` — Move um evento para outro calendário, ou seja, altera o organizador de um evento. Observe que apenas eventos padrão podem ser movidos; eventos de aniversário, tempo focado, do Gmail, fora do escritório e local de trabalho não podem ser movidos.
  - `patch` — Atualiza um evento. Este método oferece suporte a semântica de patch.
  - `quickAdd` — Cria um evento com base em uma string de texto simples.
  - `update` — Atualiza um evento.
  - `watch` — Observar mudanças nos recursos de Events.

### freebusy

  - `query` — Retorna informações de disponibilidade para um conjunto de calendários.

### settings

  - `get` — Retorna uma única configuração de usuário.
  - `list` — Retorna todas as configurações do usuário para o usuário autenticado.
  - `watch` — Observar mudanças nos recursos de Settings.

## Descobrindo Comandos

Antes de chamar qualquer método da API, inspecione-o:

```bash
# Navegar por recursos e métodos
gws calendar --help

# Inspecionar parâmetros necessários, tipos e padrões de um método
gws schema calendar.<recurso>.<método>
```

Use a saída de `gws schema` para construir seus sinalizadores `--params` e `--json`.

## Uso

```bash
# Listar recursos e métodos disponíveis
gws calendar --help

# Inspecionar schema do método antes de chamar
gws schema calendar.<recurso>.<método>

# Executar comando com argumentos
gws calendar $ARGUMENTS
```

## Tarefa

Execute a operação de Calendar solicitada: $ARGUMENTS

1. **Verificar Pré-requisitos**
   - Verificar se `gws` está instalado: `gws --version`
   - Verificar autenticação: `gws auth status`
   - Revisar comandos disponíveis: `gws calendar --help`

2. **Inspecionar Schema do Método**
   - Antes de chamar qualquer método, inspecione seus parâmetros
   - Use `gws schema` para compreender os campos necessários
   - Revise tipos de parâmetro e restrições

3. **Executar Operação**
   - Construir comando com sinalizadores apropriados
   - Usar `--params` para parâmetros de query/path
   - Usar `--json` para corpo da solicitação
   - Lidar com paginação com `--max-results` ou `--page-token`

4. **Tratamento de Erros**
   - Verificar saída do comando para erros
   - Revisar quotas de API e limites de taxa
   - Lidar com problemas de autenticação
   - Repetir falhas transitórias

---

**Licença**: Apache License 2.0
**Fonte**: [Google Workspace CLI](https://github.com/googleworkspace/cli)
**Skill Original**: `gws-calendar`