# StayDesk — Estudo de Cobertura dos Requisitos

> Status: **Em andamento** · Baseline v0.1 · 2026-09-03
> Fonte de requisitos: `Requisitos - Sistema de Atendimento (externo) (1).md`, v1.7, 05/08/2026, fornecido pelo Matheus
> Produto avaliado: Chatwoot/StayDesk 4.17.1 · evidência de código em `707120f46b`
> Regra de evidência: nenhum requisito é considerado atendido sem referência reproduzível em código, API, UI ou prova de conceito.

## Objetivo

Determinar, requisito por requisito, o que o Chatwoot já entrega, o que pode ser configurado ou integrado e o que exigirá licença Enterprise, customização do fork ou desenvolvimento novo. Este documento substitui a leitura agregada do gap analysis preliminar como baseline de decisão; a matriz atômica ainda será produzida na próxima iteração.

O documento externo é tratado somente como fonte de requisitos. Ele não é versionado neste repositório nesta fase; antes de publicá-lo, é necessário confirmar a classificação e a política de acesso aplicável aos dados operacionais nele contidos.

## Baseline recebida

| Conjunto | Quantidade | Observação |
|---|---:|---|
| Requisitos funcionais | 115 | 78 obrigatórios, 30 importantes e 7 desejáveis |
| Requisitos não funcionais | 10 | Desempenho, disponibilidade, escala, retenção, recuperação, observabilidade, portabilidade, residência e custo |
| Processos críticos | 12 | Precisam continuar demonstráveis de ponta a ponta |
| Critérios de aceite | 9 | Incluem SLA no BI, app próprio em produção e zero perda na migração |

## Método e legenda

Cada requisito atômico será classificado por edição, cobertura e ação:

- **Nativo:** atende no produto sem desenvolvimento.
- **Configuração:** atende por configuração operacional suportada.
- **Parcial:** existe uma fundação, mas parte do contrato não é atendida.
- **Ausente:** não foi encontrada capacidade equivalente.
- **Requer PoC:** análise estática não é suficiente para aprovar.
- **Enterprise:** depende do overlay proprietário e de licença válida em produção.
- **Externo:** permanece no bot, sistema interno, WHMCS ou BI e precisa de contrato de integração.

A ação resultante será uma de: `configurar`, `licenciar`, `integrar`, `estender`, `construir`, `migrar` ou `validar por PoC`.

## Síntese executiva inicial

O Chatwoot é uma base forte para conversas, inboxes, times, mensagens, canais, custom attributes, prioridades, macros, automações, portal, CSAT, APIs e webhooks. Ele reduz significativamente o trabalho de construir o núcleo de atendimento.

Ainda assim, o fork atual **não atende integralmente os requisitos eliminatórios**. Os principais gaps não estão no rebrand: estão no framework de aplicativos, na governança de automações, no modelo de SLA/calendário, na natureza do ticket, nos indicadores e na migração Zendesk. Além disso, requisitos obrigatórios relevantes dependem hoje do Enterprise.

### Achados críticos

1. **RF-8 é o gate eliminatório principal.** Dashboard Apps cadastra uma URL e renderiza um iframe na conversa, mas não oferece o SDK/plataforma exigido para múltiplos pontos de extensão, escrita segura, sessão assinada, rollback, sandbox e autorização por papel.
2. **SLA tem boa fundação no Enterprise, mas segue parcial.** A API/eventos expõem política, thresholds e vencimentos; faltam histórico de pausas, calendário excepcional unificado, estado `em risco`, percentis e snapshot imutável da meta.
3. **Automações executam, mas não são governáveis no patamar pedido.** Faltam dry-run e ledger operacional por execução com regra, ticket, condições, ações e before/after.
4. **O bot existente deve ser integrado, não presumidamente substituído pelo Captain.** Agent Bots fornece webhook/handoff e uma base de fail-open, mas não editor determinístico, simulador, versionamento de fluxo ou calendário compartilhado.
5. **Relatórios nativos são insuficientes para o contrato analítico.** O produto trabalha principalmente com médias e contagens; mediana, p90/p95, FCR, reaberturas e natureza como dimensão exigem nova camada analítica.
6. **Não existe importador Zendesk nativo.** O framework de importação pode ser estendido, mas a migração completa, o mapa de IDs e a reconciliação precisam ser construídos.
7. **A decisão MIT versus Enterprise precisa ocorrer antes do delivery dependente dessas features.** SLA, audit logs, custom roles, atributos obrigatórios, advanced assignment, Captain e busca semântica estão marcados como premium.

## Escopo funcional — criar, ajustar e configurar

### Capacidades a criar praticamente do zero

| Bloco | O que precisa ser construído | Prioridade |
|---|---|---|
| Framework de aplicativos | Evoluir o Dashboard App, hoje centrado em iframe e contexto limitado, para oferecer escrita segura no ticket, autenticação assinada, permissões, múltiplos pontos de extensão, versionamento, rollback, sandbox e logs. A `DEC-002` deve decidir entre um framework genérico e um módulo first-party do StayDesk. | P0 · eliminatório |
| Natureza do ticket | Criar `humano`, `automático`, `interno` e `resolvido pelo bot` como atributo estrutural, com efeito consistente sobre fila, SLA, indicadores e integrações. | P0 · eliminatório |
| Governança de automações | Implementar dry-run, histórico por execução, before/after, ordem explícita, identificação de conflitos/loops e uma interface compreensível pela operação. | P0 · eliminatório |
| Calendário operacional único | Criar grade semanal, feriados e exceções compartilhadas entre SLA, bot e plantão, com histórico/versionamento. | P0 |
| Camada analítica | Calcular mediana, p90/p95, FCR, reaberturas e segmentações por marca, natureza, produto, servidor e classificação, expondo resultados ao BI. | P0 |
| Migração Zendesk | Construir importador de tickets, mensagens, notas, anexos, campos, avaliações e timestamps, com mapa de IDs, sincronização incremental, reconciliação e rollback. | P0 · eliminatório |

### Capacidades existentes que precisam de ajuste ou ampliação

| Capacidade | Base reutilizável | Ajuste necessário |
|---|---|---|
| SLA | Políticas, applied SLA, thresholds, vencimentos e eventos no Enterprise | Histórico de pausas, estado `em risco`, calendário excepcional, exclusão por natureza e snapshot imutável da meta aplicada |
| Multimarca | Inboxes, portais e identidades configuráveis | Marca como dimensão consistente de filas, permissões, SLA, relatórios e integrações |
| Identidade do cliente | Contatos, identificadores e merge | Resolver Chatwoot, WHMCS e sistema interno para uma identidade única, incluindo confirmação e aprendizado de telefone novo |
| Tickets e campos | Conversas, custom attributes, notas, anexos e menções | Equivalência para estados `novo`/`fechado`, validação condicional dos 27 campos e contagem confiável de reaberturas |
| Tags | Labels normalizadas e únicas | Sinônimos, governança administrativa e migração de tags que representam servidor/departamento para campos estruturados |
| Macros | Texto, notas, status, prioridade, tags, anexos e webhook | Escopo por grupo, alteração de custom fields, dados dinâmicos do WHMCS, versionamento e métrica de uso |
| Roteamento | Times, inboxes, automações, prioridade e autoatribuição | Condições por marca, natureza, horário, classificação do cliente e competência técnica |
| Bot | Agent Bot, webhook, eventos e handoff | Integrar o bot do Matheus, preservar contexto/transcrição, validar fail-open, versionar sessões e medir deflexão por caminho |
| API e webhooks | REST, OpenAPI, rate limit, HMAC e delivery ID | Cobertura equivalente à operação, tokens com escopos, allowlist de IP, retry, log de entrega e eventos de SLA/CSAT |
| Administração e segurança | MFA individual, exportação/exclusão de contato, custom roles e audit logs premium | 2FA obrigatório, reset administrativo, retenção LGPD, mascaramento de logs e trilha ampliada |
| Conhecimento, CSAT e QA | Portal, artigos, busca, CSAT e transcrições | Versionamento editorial, base interna com permissões, CSAT no canal, alerta negativo e integração com o QA existente |

### Capacidades predominantemente configuráveis

- WhatsApp, chat do site, e-mail e API como canais de entrada.
- Inboxes, times e permissões básicas de acesso.
- Atribuição manual e automática.
- Prioridade e disponibilidade dos agentes.
- Notas internas, anexos e menções.
- Respostas prontas e macros básicas.
- Automações básicas no modelo evento → condição → ação.
- Portal público, categorias e artigos.
- CSAT básico.
- REST API, OpenAPI e webhooks básicos.
- Agent Bots e transferência para humanos.
- Relatórios básicos e exportações CSV/JSON.

### Impacto da decisão Enterprise

Com licença Enterprise, o StayDesk pode reutilizar a base existente de SLA, custom roles, audit logs, atributos obrigatórios, advanced assignment, Captain e busca semântica. Isso reduz o volume de código próprio, mas **não elimina** os seis blocos classificados acima como construção substancial.

Na opção MIT-only, além desses seis blocos, será necessário implementar alternativas independentes para SLA, papéis, auditoria e demais recursos premium, sem copiar código do overlay proprietário. Portanto, a opção MIT-only aumenta significativamente o esforço de construção, testes e manutenção contra o upstream.

### Ordem funcional recomendada

1. Executar o PoC do aplicativo real do Matheus dentro do Chatwoot.
2. Modelar e validar a natureza do ticket de ponta a ponta.
3. Executar o PoC do SLA completo na API e nos webhooks.
4. Integrar o bot em web e WhatsApp, com handoff e fail-open.
5. Provar dry-run e log auditável de automações.
6. Executar o spike do importador Zendesk.
7. Decidir Enterprise versus MIT próprio com base nos PoCs.
8. Decompor os gaps aprovados em stories de implementação.

## Matriz preliminar por domínio

> Esta visão é deliberadamente conservadora. “Parcial” não significa pronto para produção; significa apenas que há fundação reutilizável.

| Domínio | Veredito inicial | O que já existe | Gap dominante | Direção recomendada |
|---|---|---|---|---|
| RF-1 Omnicanal e multimarca | Parcial | WhatsApp, widget, e-mail, API, inboxes e portais | Marca não é entidade/dimensão transversal; identidade multicanal e aprendizado de telefone não são nativos | Configurar canais e modelar marca/identidade com integração |
| RF-2 Tickets e conversas | Parcial | Conversas, custom attributes tipados, labels, notas privadas, anexos e menções | Estados não equivalem ao contrato completo; validação condicional e governança de tags são limitadas | Estender modelo e validações |
| RF-3 Filas e atribuição | Parcial forte | Times, permissões por inbox, autoatribuição, disponibilidade e prioridade | Natureza do ticket e exclusão transversal de fila/SLA/indicadores não existem | Construir natureza como conceito de primeira classe |
| RF-4 SLA e calendário | Parcial · Enterprise | Políticas, applied SLA, eventos, thresholds e payloads na API | Pausas, feriados/exceções, calendário único, `at_risk`, percentis e versionamento histórico | PoC + decisão licenciar versus implementar |
| RF-5 Macros | Parcial | Texto, nota, status, prioridade, tags, anexos e webhook | Escopo por grupo, custom fields, dados dinâmicos WHMCS, métricas de uso e versionamento | Estender macros e comprovar o mecanismo de variáveis por canal |
| RF-6 Automações | Parcial | Eventos, condições, ações e execuções atrasadas | Dry-run, ledger auditável, ordenação e detecção de conflito/loop | Construir camada de governança |
| RF-7 Bot de triagem | Parcial como plataforma | Agent Bot, webhook assinado, eventos, token e handoff | Editor/simulador/calendário/versionamento não existem no núcleo | Integrar o bot do Matheus e validar dois canais reais |
| RF-8 Framework de apps | Não atende · eliminatório | Dashboard App privado por URL, iframe e contexto básico | SDK de escrita, assinatura, extensões, rollback, sandbox, permissões e observabilidade | PoC do app real antes de avançar |
| RF-9 IA | Parcial · Enterprise | Captain cobre sugestão e resumo em parte | Classificação/fail-open e políticas específicas não estão comprovadas | Tratar como trilha posterior, sem substituir bot determinístico |
| RF-10 Conhecimento, CSAT e QA | Parcial | Portais, artigos, busca, CSAT e transcrição | Base interna, versionamento editorial, QA e alerta negativo completos | Manter QA externo no início e estender portal/CSAT |
| RF-11 Indicadores | Parcial | Relatórios por conta/agente/inbox/time, CSAT, heatmap, API/CSV | Mediana/p90/p95, FCR, reaberturas e segmentações exigidas | BI próprio como fonte canônica com eventos enriquecidos |
| RF-12 API e webhooks | Parcial | REST, OpenAPI, rate limit, HMAC e delivery ID | Cobertura não é equivalente à UI; scopes/IP allowlist/retry/log/eventos SLA-CSAT incompletos | Definir contrato de API e delivery ledger |
| RF-13 Administração e segurança | Parcial · Enterprise | MFA individual, exportação/exclusão de contato, custom roles e audit logs premium | 2FA obrigatório/reset admin, retenção e auditoria completa não comprovados | Threat model, política LGPD e decisão de licença |
| RF-14 Migração | Ausente para Zendesk | Framework reutilizável para Freshdesk/Intercom e IDs externos | Fonte Zendesk, sincronização paralela, anexos e reconciliação | Construir importador e migração de ensaio completa |
| RNFs | Não verificadas | Self-hosting dá controle de infraestrutura | Sem benchmark, SLO, backup testado, RPO/RTO ou prova de exportação integral | Criar plano de testes não funcionais |

## Evidências de código prioritárias

| Tema | Evidência inicial |
|---|---|
| Versão avaliada | `VERSION_CW`, `config/app.yml`, `package.json` |
| Licenciamento/capacidades premium | `config/features.yml`, `enterprise/LICENSE` |
| Estados da conversa | `app/models/conversation.rb` |
| SLA | `enterprise/app/models/applied_sla.rb`, `enterprise/app/models/sla_event.rb`, `enterprise/app/views/enterprise/api/v1/conversations/partials/_conversation.json.jbuilder` |
| Horário de atendimento | `app/models/concerns/out_of_offisable.rb`, `app/models/working_hour.rb` |
| Macros | `app/models/macro.rb`, `app/services/macros/execution_service.rb` |
| Automações | `app/models/automation_rule.rb`, `app/listeners/automation_rule_listener.rb`, `app/services/automation_rules/action_service.rb` |
| Agent Bot | `app/models/agent_bot.rb`, `app/models/agent_bot_inbox.rb`, `app/listeners/agent_bot_listener.rb` |
| Dashboard Apps | `app/models/dashboard_app.rb`, `app/javascript/dashboard/components/widgets/DashboardApp/Frame.vue` |
| Métricas | `app/services/reports/report_metric_registry.rb`, `app/builders/v2/reports/first_response_time_distribution_builder.rb` |
| Webhooks/tokens | `app/models/webhook.rb`, `lib/webhooks/trigger.rb`, `app/models/access_token.rb` |
| Importação | `app/models/data_import.rb`, `app/services/data_imports/source.rb` |

## Gates de decisão

### Gate 0 — Ativos existentes do Matheus

Receber e versionar, em repositório ou ambiente apropriado, arquitetura e contratos do bot, app, API própria, webhooks, modelos de dados e exemplos de tickets. Validar o que está realmente em produção e o que é apenas requisito futuro. Em paralelo, executar o spike 2.6 para comprovar limites, anexos, timestamps, IDs e carga incremental da migração Zendesk.

### Gate 1 — PoCs eliminatórios

- App real na interface: leitura, escrita, autenticação assinada, permissões e escalonamento ponta a ponta.
- SLA: política e estado completo na API/webhook, incluindo pausas e `at_risk`.
- Bot web + WhatsApp: identificação, triagem, deflexão, handoff e fail-open.
- Automação: dry-run e log de uma execução real compreensível pela operação.
- Natureza do ticket: exclusão comprovada de fila humana, SLA e indicadores.

### Gate 2 — Licenciamento e arquitetura

Comparar duas opções legais e operacionais: licenciar Enterprise ou implementar capacidades próprias sem copiar o overlay proprietário. A decisão deve considerar custo total de manutenção e impacto no sync com upstream.

### Gate 3 — Núcleo e dados

Executar os obrigatórios de tickets, filas, calendário, macros, automações, segurança, API e indicadores. Cada story deve declarar os IDs `RF-*`/`RNF-*` cobertos e a evidência de aceite.

### Gate 4 — Migração de ensaio

Migrar uma amostra representativa e depois os ~16 mil tickets em ambiente de teste, reconciliando contagens, anexos, avaliações, timestamps e mapa de IDs. Testar operação paralela por canal.

### Gate 5 — Virada

Somente após os 12 processos críticos serem demonstrados e os nove critérios de aceite terem evidência registrada. O Zendesk permanece disponível até completar 30 dias sem regressão de SLA, CSAT e FCR; só então as licenças antigas podem ser encerradas.

## Decisões abertas

- `DEC-001`: Enterprise licenciado ou implementação MIT independente.
- `DEC-002`: extensão do Dashboard App versus módulo first-party do StayDesk.
- `DEC-003`: bot externo do Matheus como motor canônico versus reconstrução parcial.
- `DEC-004`: modelo de multimarca — uma conta com inboxes/portais ou contas separadas.
- `DEC-005`: autoridade de identidade entre Chatwoot, WHMCS e sistema interno.
- `DEC-006`: modelo de natureza do ticket e sua propagação para SLA/filas/BI.
- `DEC-007`: fonte oficial dos indicadores e contrato de paridade com o BI.
- `DEC-008`: política de retenção, residência, backup, RPO e RTO.

## Próximas entregas do estudo

### Contrato da matriz atômica

Cada linha da matriz terá, no mínimo:

```text
req_id · source_version · source_priority · requirement · linked_processes
acceptance_criterion · chatwoot_capability · edition · verdict · gap · action
evidence_type · evidence_reference · confidence · decision_id · owner
epic_story · dependencies · delivery_gate · status · last_verified_at
verified_against_commit
```

O backlog será derivado desse contrato; ele não substitui a matriz.

- [ ] Atomizar os 115 RFs e 10 RNFs em matriz canônica.
- [ ] Vincular os 12 processos e nove critérios de aceite aos requisitos correspondentes.
- [ ] Registrar edição (`MIT`, `Enterprise`, `custom`, `external`) e evidência por linha.
- [ ] Executar os cinco PoCs eliminatórios e anexar resultados.
- [ ] Transformar gaps aprovados em stories com dependências e owner.
- [ ] Criar decision log para `DEC-001` a `DEC-008`.
- [ ] Definir plano de benchmark e recuperação para os RNFs.

## Handoffs

| Owner | Destino | Entrega e caminho | Gate/estado | Critério de aceite |
|---|---|---|---|---|
| Product | Arquitetura/Backend | Matriz atômica em `docs/staydesk/requirements/coverage-matrix.csv` | Gate 1 · pendente | Cada obrigatório tem status, ação e evidência requerida |
| Matheus | Arquitetura/Backend | Ativos técnicos do bot, app, API e webhooks em repositório com acesso controlado | Gate 0 · pendente | Versão implantada, contratos e dependências identificados |
| Naldo | Product/QA | Exports de campos, macros, regras, SLAs, filas, templates e relatórios | Gate 0 · pendente | Os 12 processos podem ser reproduzidos e validados |
| Arquitetura/Backend | QA | Relatórios em `docs/staydesk/pocs/` e contratos de API/eventos | Gate 1 · pendente | Resultado reproduzível, limitações e rollback documentados |
| Product/Arquitetura | CTO | Decision log em `docs/staydesk/decisions/decision-log.md` | Gate 2 · pendente | `DEC-001` a `DEC-008` têm owner e decisão aprovada |
| QA | CTO/Product | Evidence register em `docs/staydesk/evidence/evidence-register.md` | Gates 1–5 · pendente | Gate com `GO`, `CONDITIONAL_GO` ou `NO_GO` por critério |
| Design | Frontend | Superfícies ligadas aos processos/RFs no front-end spec | Paralelo · em andamento | Rebrand não cria dependência no caminho crítico funcional |

## Limitações desta baseline

Esta primeira versão é uma análise estática. Não foram executados PoCs, benchmark ou migração. O código do bot, do aplicativo e do sistema interno do Matheus não está neste repositório; portanto, as capacidades externas foram tratadas como alegações a validar. A confiança é alta para gaps identificados no fork e média/baixa para a cobertura dos ativos externos.
