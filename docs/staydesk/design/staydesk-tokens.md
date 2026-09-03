# StayDesk — Design Tokens

> Status: v1 draft · 2026-08-28 · Taxonomia canônica do StayDesk para implementação e calibração no Histoire.
> Proveniência: valores iniciais adaptados da referência externa histórica `staycloud-ui-guardian/references/brand-tokens.md`, que não é versionada neste repositório. Este documento passa a ser a referência auditável do projeto; divergências devem ser resolvidas aqui antes de chegar ao código.

## Princípios

- A arquitetura tem três camadas, sempre nesta direção: **Primitive → Semantic → Component**.
- Primitives guardam valores absolutos. Semantic tokens descrevem intenção e só referenciam primitives.
- Component tokens descrevem partes de componentes e só referenciam semantic tokens.
- Dark mode redireciona aliases semânticos; não cria uma segunda taxonomia.
- Valores são documentados no formato de alias do W3C DTCG (`{caminho.do.token}`), preparando uma futura exportação por Style Dictionary.
- Mais cinza que cor, mais espaço que decoração; indigo é assinatura, não enchimento.

---

## 1. Primitive tokens

Primitives não devem ser usados diretamente em componentes.

### Cor — neutros

| Token | Valor |
|---|---|
| `primitive.color.neutral.0` | `#ffffff` |
| `primitive.color.neutral.25` | `#fafafa` |
| `primitive.color.neutral.50` | `#f7f7f5` |
| `primitive.color.neutral.100` | `#f1f1ef` |
| `primitive.color.neutral.200` | `#ededed` |
| `primitive.color.neutral.300` | `#e6e6e3` |
| `primitive.color.neutral.400` | `#e4e4e7` |
| `primitive.color.neutral.500` | `#a1a1aa` |
| `primitive.color.neutral.600` | `#71717a` |
| `primitive.color.neutral.700` | `#27272a` |
| `primitive.color.neutral.800` | `#262626` |
| `primitive.color.neutral.850` | `#1f1f1f` |
| `primitive.color.neutral.900` | `#18181b` |
| `primitive.color.neutral.925` | `#161616` |
| `primitive.color.neutral.950` | `#111111` |
| `primitive.color.neutral.1000` | `#0a0a0a` |

### Cor — marca

| Token | Valor |
|---|---|
| `primitive.color.brand.25` | `#f5f6ff` |
| `primitive.color.brand.50` | `#eef0ff` |
| `primitive.color.brand.75` | `#e3e6ff` |
| `primitive.color.brand.100` | `#d6daff` |
| `primitive.color.brand.200` | `#b3b9ff` |
| `primitive.color.brand.300` | `#8b93ff` |
| `primitive.color.brand.400` | `#6a72ff` |
| `primitive.color.brand.500` | `#545dff` |
| `primitive.color.brand.600` | `#4149e6` |
| `primitive.color.brand.700` | `#2c3bd6` |
| `primitive.color.brand.800` | `#232fa8` |
| `primitive.color.brand.900` | `#1a2378` |

### Cor — estado

Os steps `500` são acentos visuais; os steps `700` são foregrounds sobre superfícies claras.

| Token | Valor |
|---|---|
| `primitive.color.success.50` | `#ecfdf5` |
| `primitive.color.success.500` | `#1a9f63` |
| `primitive.color.success.700` | `#047857` |
| `primitive.color.warning.50` | `#fffbeb` |
| `primitive.color.warning.500` | `#d97706` |
| `primitive.color.warning.700` | `#92400e` |
| `primitive.color.danger.50` | `#fef2f2` |
| `primitive.color.danger.500` | `#dc2626` |
| `primitive.color.danger.700` | `#b91c1c` |

### Tipografia, raio, sombra e motion

| Token | Valor |
|---|---|
| `primitive.font.family.sans` | `Geist, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif` |
| `primitive.font.weight.regular` | `400` |
| `primitive.font.weight.medium` | `500` |
| `primitive.font.weight.semibold` | `600` |
| `primitive.radius.1` | `0.375rem` |
| `primitive.radius.2` | `0.5rem` |
| `primitive.radius.3` | `0.625rem` |
| `primitive.radius.4` | `0.75rem` |
| `primitive.radius.5` | `0.875rem` |
| `primitive.radius.full` | `9999px` |
| `primitive.shadow.1` | `0 1px 0 rgb(0 0 0 / 0.08)` |
| `primitive.shadow.2` | `0 1px 0 rgb(0 0 0 / 0.08), 0 8px 24px -10px rgb(0 0 0 / 0.08)` |
| `primitive.shadow.3` | `0 1px 0 rgb(0 0 0 / 0.08), 0 16px 40px -12px rgb(0 0 0 / 0.12)` |
| `primitive.motion.duration.fast` | `120ms` |
| `primitive.motion.duration.modal` | `160ms` |
| `primitive.motion.easing.standard` | `ease` |

Geist deve ser distribuída como WOFF2 self-hosted em produção. Ferramentas locais podem usar uma fonte instalada no sistema, mas não devem depender de Google Fonts nem de rede em runtime.

---

## 2. Semantic tokens

### Superfície, texto e borda — redirects de tema

| Token | Light | Dark | Uso |
|---|---|---|---|
| `semantic.color.surface.canvas` | `{primitive.color.neutral.50}` | `{primitive.color.neutral.1000}` | Fundo principal |
| `semantic.color.surface.default` | `{primitive.color.neutral.0}` | `{primitive.color.neutral.925}` | Card, modal, dropdown |
| `semantic.color.surface.subtle` | `{primitive.color.neutral.100}` | `{primitive.color.neutral.950}` | Header de tabela, footer |
| `semantic.color.surface.strong` | `{primitive.color.neutral.300}` | `{primitive.color.neutral.850}` | Track, divisor pesado |
| `semantic.color.text.primary` | `{primitive.color.neutral.900}` | `{primitive.color.neutral.25}` | Texto principal |
| `semantic.color.text.secondary` | `{primitive.color.neutral.700}` | `{primitive.color.neutral.400}` | Texto secundário forte |
| `semantic.color.text.muted` | `{primitive.color.neutral.600}` | `{primitive.color.neutral.500}` | Texto secundário e terciário |
| `semantic.color.text.disabled` | `{primitive.color.neutral.500}` | `{primitive.color.neutral.600}` | Apenas estado desabilitado; não usar para informação essencial |
| `semantic.color.border.default` | `{primitive.color.neutral.400}` | `{primitive.color.neutral.800}` | Border hairline |
| `semantic.color.border.subtle` | `{primitive.color.neutral.200}` | `{primitive.color.neutral.850}` | Divisor interno |

Dark mode usa preto neutro. Não introduzir fundos azulados ou roxeados.

### Ação, marca e foco

| Token | Light | Dark | Uso |
|---|---|---|---|
| `semantic.color.action.primary.background` | `{primitive.color.brand.500}` | `{primitive.color.brand.400}` | CTA primário |
| `semantic.color.action.primary.foreground` | `{primitive.color.neutral.0}` | `{primitive.color.neutral.1000}` | Texto/ícone do CTA |
| `semantic.color.action.primary.hover` | `{primitive.color.brand.600}` | `{primitive.color.brand.300}` | Hover do CTA |
| `semantic.color.action.primary.pressed` | `{primitive.color.brand.700}` | `{primitive.color.brand.200}` | Active/pressed |
| `semantic.color.action.link.foreground` | `{primitive.color.brand.700}` | `{primitive.color.brand.300}` | Link em texto |
| `semantic.color.brand.surface.subtle` | `{primitive.color.brand.25}` | `{primitive.color.brand.900}` | Fundo de ícone ou pill |
| `semantic.color.brand.surface.hover` | `{primitive.color.brand.50}` | `{primitive.color.brand.800}` | Hover de área de marca |
| `semantic.color.focus.ring` | `{primitive.color.brand.500}` | `{primitive.color.brand.300}` | Focus visible |

### Estados

| Token | Light | Dark | Uso |
|---|---|---|---|
| `semantic.color.status.success.surface` | `{primitive.color.success.50}` | `{primitive.color.neutral.850}` | Fundo de status |
| `semantic.color.status.success.foreground` | `{primitive.color.success.700}` | `{primitive.color.neutral.25}` | Label acessível |
| `semantic.color.status.success.accent` | `{primitive.color.success.500}` | `{primitive.color.success.500}` | Dot/ícone, nunca sozinho |
| `semantic.color.status.warning.surface` | `{primitive.color.warning.50}` | `{primitive.color.neutral.850}` | Fundo de status |
| `semantic.color.status.warning.foreground` | `{primitive.color.warning.700}` | `{primitive.color.neutral.25}` | Label acessível |
| `semantic.color.status.warning.accent` | `{primitive.color.warning.500}` | `{primitive.color.warning.500}` | Dot/ícone, nunca sozinho |
| `semantic.color.status.danger.surface` | `{primitive.color.danger.50}` | `{primitive.color.neutral.850}` | Fundo de status |
| `semantic.color.status.danger.foreground` | `{primitive.color.danger.700}` | `{primitive.color.neutral.25}` | Label acessível |
| `semantic.color.status.danger.accent` | `{primitive.color.danger.500}` | `{primitive.color.danger.500}` | Dot/ícone, nunca sozinho |

Texto normal deve atingir contraste WCAG AA de 4.5:1. Acentos que não alcançam esse limiar ficam restritos a dots, ícones grandes, bordas ou fundos e sempre aparecem com label textual.

---

## 3. Component tokens

| Token | Alias semântico |
|---|---|
| `component.button.primary.background` | `{semantic.color.action.primary.background}` |
| `component.button.primary.foreground` | `{semantic.color.action.primary.foreground}` |
| `component.button.primary.border-radius` | `{semantic.radius.control}` |
| `component.input.background` | `{semantic.color.surface.default}` |
| `component.input.border` | `{semantic.color.border.default}` |
| `component.input.focus-ring` | `{semantic.color.focus.ring}` |
| `component.input.border-radius` | `{semantic.radius.control}` |
| `component.card.background` | `{semantic.color.surface.default}` |
| `component.card.border` | `{semantic.color.border.subtle}` |
| `component.card.border-radius` | `{semantic.radius.card}` |
| `component.badge.unread.background` | `{semantic.color.surface.strong}` |
| `component.badge.unread.foreground` | `{semantic.color.text.primary}` |
| `component.status.success.foreground` | `{semantic.color.status.success.foreground}` |
| `component.status.success.accent` | `{semantic.color.status.success.accent}` |
| `component.conversation.canvas.background` | `{semantic.color.surface.canvas}` |
| `component.context-panel.background` | `{semantic.color.surface.default}` |
| `component.modal.shadow` | `{semantic.shadow.overlay}` |
| `component.modal.border-radius` | `{semantic.radius.modal}` |

Aliases estruturais usados acima:

| Token | Alias primitive |
|---|---|
| `semantic.radius.control` | `{primitive.radius.2}` |
| `semantic.radius.list-item` | `{primitive.radius.3}` |
| `semantic.radius.card` | `{primitive.radius.4}` |
| `semantic.radius.modal` | `{primitive.radius.5}` |
| `semantic.radius.pill` | `{primitive.radius.full}` |
| `semantic.shadow.overlay` | `{primitive.shadow.2}` |
| `semantic.motion.interaction.duration` | `{primitive.motion.duration.fast}` |
| `semantic.motion.interaction.easing` | `{primitive.motion.easing.standard}` |

---

## Regras absolutas

1. Máximo de dois acentos indigo visíveis por tela e um botão primary.
2. Ícones banidos: `Sparkles`, `Wand`, `Wand2`, `WandSparkles`; sem emojis decorativos.
3. Componentes do design system são obrigatórios; não usar `<select>` nativo.
4. Mock ou placeholder não vai para produção.
5. Zero vazamento de infraestrutura na interface.
6. Sem container duplo; `max-width` pertence ao layout pai.
7. Sentence case sempre; sem exclamação ou hype.
8. Status comercial e status runtime usam famílias de component tokens distintas.
9. Estrutura canônica: eyebrow + h1 + tabs opcionais + conteúdo.
10. Icon-button tem `aria-label`; status usa cor + dot + label; loading usa `inert`.
11. Motion respeita `prefers-reduced-motion`; sem bounce, parallax, confetti ou pulse contínuo acima de 2s.

---

## Ponte de implementação no Chatwoot

O repositório possui dois contratos de cor que precisam permanecer sincronizados durante a migração:

| Superfície | Contrato atual | Arquivos de implementação |
|---|---|---|
| Legado | Classes baseadas na escala `woot` | [theme/colors.js](../../../theme/colors.js) + [tailwind.config.js](../../../tailwind.config.js) |
| `components-next/` | Classes `n.*`, incluindo `n-brand`, `n-blue` e `n-iris`, apoiadas por variáveis CSS | [theme/colors.js](../../../theme/colors.js) + [_next-colors.scss](../../../app/javascript/dashboard/assets/scss/_next-colors.scss) + [tailwind.config.js](../../../tailwind.config.js) |

Trocar apenas `colors.woot` rebranda consumidores legados, mas não cobre `components-next/`. A implementação deve mapear os semantic tokens deste documento para `n.*` e para os redirects light/dark em `_next-colors.scss`, mantendo `woot` como camada de compatibilidade enquanto houver superfícies antigas.

### Escala de compatibilidade `woot`

| Step | Alias primitive |
|---|---|
| 25 | `{primitive.color.brand.25}` |
| 50 | `{primitive.color.brand.50}` |
| 75 | `{primitive.color.brand.75}` |
| 100 | `{primitive.color.brand.100}` |
| 200 | `{primitive.color.brand.200}` |
| 300 | `{primitive.color.brand.300}` |
| 400 | `{primitive.color.brand.400}` |
| 500 | `{primitive.color.brand.500}` |
| 600 | `{primitive.color.brand.600}` |
| 700 | `{primitive.color.brand.700}` |
| 800 | `{primitive.color.brand.800}` |
| 900 | `{primitive.color.brand.900}` |

### Ordem de ataque (EPIC-003)

1. **3.1a** — Mapear a marca nos dois contratos: `woot` para legado e `n.*`/variáveis CSS para Next; configurar Geist self-hosted.
2. **3.1b** — Validar light/dark e estados no Histoire; calibrar escala, aliases e contraste.
3. **3.2** — Inventariar componentes e superfícies antes de avançar para as stories 3.3–3.9 descritas em [front-end-spec.md](front-end-spec.md).
