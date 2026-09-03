# StayDesk — Epics & Backlog Inicial

> Status: Draft v1 · 2026-08-28 · Plano FABLE CTO · Prioridade: P0 (agora) → P3 (depois)

## EPIC-001 — Ambiente & Fundação (P0)

| # | Story | Dono | Status |
|---|---|---|---|
| 1.1 | Fork `stay-cloud-br/staydesk` + clone + upstream remote | devops | ✅ Done |
| 1.2 | Subir Chatwoot local via Docker (`localhost:3020`) para avaliação | devops | 🔄 In progress |
| 1.3 | Criar conta admin + workspace de teste, tour guiado com Naldo | Naldo + Luiz | ⬜ |
| 1.4 | Ambiente dev frontend nativo (Ruby 3.2.2 + Node 20 + pnpm + overmind, hot-reload Vite) | devs | ⬜ |
| 1.5 | CI básico no fork (lint + vitest no GitHub Actions, sem herdar workflows pesados do upstream) | devops | ⬜ |
| 1.6 | Protocolo de sync com upstream documentado e testado (1º rebase) | devs | ⬜ |

## EPIC-002 — Discovery & Decisão (P0)

| # | Story | Dono | Status |
|---|---|---|---|
| 2.1 | Incorporar levantamento v1.7 do Matheus como baseline; Naldo valida processos e fornece exports | Product + Naldo | ✅ Baseline incorporada; validação operacional pendente |
| 2.2 | Matriz atômica requisito → capacidade → gap → ação → evidência → story | Product + Arquitetura | 🔄 [Estudo iniciado](../requirements-coverage-study.md) |
| 2.3 | Decisão de licenciamento Enterprise (MIT-only vs pagar vs construir) | CTO + Luiz | 🔶 **Parcial** — avaliação local pode usar tudo; decisão pagar-vs-construir ocorre após 2.5 e antes do delivery dependente. Story 6.4 confirma no go-live |
| 2.4 | Auditoria de automações/triggers do Zendesk atual (export) | Naldo + analyst | ⬜ |
| 2.5 | PoCs eliminatórios: app real, SLA/API, bot web+WhatsApp, automações e natureza do ticket | Arquitetura + QA | ⬜ **bloqueia decisão de adoção** |
| 2.6 | Spike de migração: limites Zendesk, anexos, timestamps, IDs e sincronização incremental | Backend + QA | ⬜ **bloqueia decisão de adoção** |

## EPIC-003 — Frontend StayDesk (P1 — trilha do Luiz)

| # | Story | Notas |
|---|---|---|
| 3.1a | Mapear design tokens StayDesk nos contratos `woot` (legado) e `n.*`/variáveis CSS (Next) + Geist self-hosted | Base de tudo; cobre `theme/colors.js`, `_next-colors.scss` e `tailwind.config.js` |
| 3.1b | Validar tokens light/dark no Histoire e calibrar escala, aliases e contraste | Depende de 1.4; valida 3.1a antes do redesign estrutural |
| 3.2 | Explorar `components-next/` no Histoire e mapear cobertura das superfícies do redesign | Upstream já está migrando — surfar essa onda |
| 3.3 | Redesign do shell do dashboard (sidebar, header, account switcher) | Maior impacto estrutural diário |
| 3.4 | Redesign da lista de conversas (hierarquia, badges, densidade) | Preserva a fila como superfície operacional |
| 3.5 | Redesign da conversa + composer (proporção, bolhas, ações rápidas) | Região dominante da experiência do agente |
| 3.6 | Context panel com abas Cliente/StayCloud/Notas | Prepara EPIC-004; implementação depende da `DEC-002` |
| 3.7 | Rebrand do widget de chat (cara StayCloud nos sites) | Visível pro cliente final |
| 3.8 | Rebrand do Help Center/portal | |
| 3.9 | Login/onboarding com identidade StayDesk | Depende da `DEC-001`; ver flag `disable_branding` (premium) |

## EPIC-004 — Integração StayCloud (P1/P2 — diferencial vs Zendesk)

| # | Story | Notas |
|---|---|---|
| 4.1 | Sidebar de contexto do cliente no ticket (plano, servidores, status, faturas via API do painel) | Dashboard Apps é hipótese condicionada ao PoC 2.5 e à `DEC-002` |
| 4.2 | Identificação automática do contato (e-mail ↔ conta StayCloud) | |
| 4.3 | Ações rápidas no ticket (ex.: link mágico de login, reiniciar serviço) | Requer API painel + permissões |
| 4.4 | Webhooks StayCloud → conversa (alerta de servidor, suspensão, churn risk) | |

## EPIC-005 — Migração Zendesk (P2 — após stories 2.5, 2.6 e 2.3)

| # | Story | Notas |
|---|---|---|
| 5.1 | Inventário e mapeamento Zendesk → StayDesk (campos, tags, agentes, marcas e IDs) | Derivado do spike 2.6 |
| 5.2 | Importador de contatos/organizações e artigos da Central de Ajuda | API Zendesk → framework de importação Chatwoot |
| 5.3 | Importador do histórico completo: tickets, notas, anexos, campos, timestamps e avaliações | Preservar IDs ou mapa de-para |
| 5.4 | Carga incremental e operação em paralelo por canal | Manter sincronizações externas durante a transição |
| 5.5 | Reconciliação por contagem, checksums e amostragem funcional | Critério: zero perda |
| 5.6 | Ensaio completo, plano de rollback e cutover progressivo | Zendesk passa a somente leitura apenas após o gate |

## EPIC-006 — Produção (P3)

| # | Story | Notas |
|---|---|---|
| 6.1 | Deploy em VPS StayCloud (compose hardened, backups PG, SSL) | |
| 6.2 | E-mail transacional (SMTP) + inbound email | |
| 6.3 | Monitoramento + alertas | |
| 6.4 | Gate de go-live e estabilização com Naldo | Confirmar licença Enterprise ou substituições antes de produção; encerrar licenças Zendesk somente após 30 dias sem regressão de SLA, CSAT e FCR, com evidência no Evidence Register |

## Sequência crítica

```
                                  ┌──▶ 2.5 ──┐
1.2 ──▶ 1.3 ──▶ 2.1 ──▶ 2.2 ────┤          ├──▶ 2.3 ─┬─▶ EPIC-004 (devs)
                                  └──▶ 2.6 ──┘         └─▶ EPIC-005 ──▶ EPIC-006

1.4 ──▶ EPIC-003 (Luiz, em paralelo; não substitui os gates de viabilidade funcional)
```
