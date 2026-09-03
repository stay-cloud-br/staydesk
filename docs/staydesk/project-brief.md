# StayDesk — Project Brief

> Status: Draft v2 · 2026-09-03 · Autor: FABLE CTO (orquestração SINAPSE)

## Visão

StayDesk é o helpdesk da StayCloud, construído como **fork do Chatwoot v4.17.1** ([stay-cloud-br/staydesk](https://github.com/stay-cloud-br/staydesk)). Objetivo: **substituir o Zendesk** com uma ferramenta self-hosted, customizável e com custo previsível, desacoplado do volume de tickets automáticos, evoluindo o frontend para o padrão visual StayCloud.

## Por que sair do Zendesk

| Driver | Detalhe |
|---|---|
| Custo | A base Chatwoot self-hosted é MIT; o custo-alvo depende da decisão de licenciar ou substituir as capacidades Enterprise exigidas |
| Controle | Dados de clientes na nossa infra (LGPD, soberania) |
| Customização | Frontend Vue 3 próprio — podemos moldar a UX ao fluxo do CS da StayCloud |
| Integração | API aberta → integração futura com painel StayCloud, checkout, billing |

## Papéis

| Quem | Responsabilidade |
|---|---|
| **Luiz** | Frontend — redesign visual, UX, design system StayDesk |
| **Devs** | Backend, infra, integrações, features novas |
| **Matheus** | Autor/origem do levantamento v1.7 e entregador dos ativos técnicos existentes |
| **Product** | Custodiante da matriz de requisitos, decisões e rastreabilidade do backlog |
| **Naldo** (Coord. CS) | Aprovador operacional — fornece exports do Zendesk e valida os 12 processos e a paridade funcional |

## Escopo (IN)

1. Rodar Chatwoot local (Docker) → ambiente de avaliação
2. Estudo de cobertura: requisitos v1.7 do Matheus vs Chatwoot OSS/Enterprise
3. Ambiente de dev frontend (Vite + Vue 3) pro Luiz
4. Roadmap de melhorias de frontend + features CS
5. Migração progressiva do Zendesk (dados e operação) — fase posterior

## Fora de escopo (OUT) — por enquanto

- Deploy em produção (depende do gate de validação do Naldo)
- Uso de features **enterprise** do Chatwoot sem decisão de licenciamento (ver `architecture-map.md` § Licenciamento)
- Execução da migração de histórico do Zendesk (obrigatória, mas posterior aos PoCs eliminatórios)

## Riscos principais

| Risco | Severidade | Mitigação |
|---|---|---|
| SLA/relatórios avançados são código Enterprise (não-MIT) | **Alta** | Provar o contrato obrigatório e decidir entre licenciar ou construir capacidades próprias |
| Divergência do upstream (fork drift) | Média | Manter `upstream` remote, rebase periódico, customizações isoladas |
| Requisitos formalizados, mas ainda sem rastreabilidade atômica | Alta | Baseline em `requirements-coverage-study.md`; atomizar 115 RFs + 10 RNFs e validar com Matheus/Naldo |
| Framework atual de Dashboard Apps abaixo do RF-8 | **Alta** | Executar PoC eliminatório com o app real antes do delivery amplo |

## Próximos gates

1. ✅ Fork + ambiente local no ar
2. ✅ Levantamento v1.7 do Matheus incorporado como baseline
3. 🔄 Matriz de cobertura atômica + PoCs eliminatórios
4. ⬜ Decisão de licenciamento Enterprise versus implementação própria
5. ⬜ Backlog rastreável priorizado → sprints
