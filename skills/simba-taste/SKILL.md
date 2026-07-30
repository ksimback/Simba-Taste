---
name: simba-taste
description: Kevin's personal frontend taste as a complete design system. One locked "refined editorial-premium" style (Spectral + Work Sans, dials 7/6/4) with a swappable, fully-tokenized palette library (Oxblood default; Evergreen, Aubergine alternates), light + dark themes, responsive foundations, and a full component set. Adapts to the project's stack (HTML/CSS, Tailwind, or React/Next). Every build ships a DESIGN.md. Hard anti-AI-tell rules baked in. Use for landing pages, marketing sites, portfolios, and redesigns that must look intentional and human-made.
---

# Simba-Taste — Refined Editorial-Premium Design System

> One opinionated style, done well, in a small library of palettes — now a full system: light + dark, responsive, real components, and a design doc on every build.
> Color is the only aesthetic dial you turn per project. Everything else is fixed below.

Use for: landing pages, marketing sites, portfolios, redesigns where the goal is "premium and distinctive."
Not for: dashboards, dense data tables, multi-step product UI.

---

## 0. Start every build here

1. **Resolve the stack** (§1).
2. **State the design read:** *"Building in Simba-Taste, `<palette>` palette, `<stack>`, light+dark."* Default palette is **Oxblood**.
3. Build from the reference (`reference/template.html` + `reference/components.html`), applying the locked style (§2), token system (§3), responsive + dark rules (§4), and the anti-AI-tell rulebook (§6).
4. **Emit a `DESIGN.md`** for the project (§7).

---

## 1. Stack resolution (adaptive)

The skill outputs in the project's real stack — do not default to generic HTML blindly.

- **If there is existing code:** detect the framework and match it. Signals: `package.json` deps (`next`, `react`, `vue`), `tailwind.config.*` / `@tailwind` directives, `.tsx/.jsx` files, existing component conventions. Emit code that fits (component files, Tailwind classes mapped to tokens, etc.).
- **If greenfield / unclear:** ask a short interview — "What are we building this in: plain HTML/CSS, Tailwind, or React/Next (+Tailwind)? Any existing components or design tokens to honor?" Then proceed.
- **All three targets are first-class.** The token contract (§3) is the bridge:
  - **HTML/CSS:** tokens in `:root`, as in the reference.
  - **Tailwind:** map tokens to `theme.extend.colors` that reference the CSS variables; keep the same names.
  - **React/Next:** expose tokens via a `:root` stylesheet or theme object; same token names.

---

## 2. The locked style (do not renegotiate)

**Feel:** expensive, warm, editorial, calm-confident. Premium through restraint and typography.

**Dials (fixed):** `DESIGN_VARIANCE 7 / MOTION 6 / VISUAL_DENSITY 4`.

**Type:** **Spectral** (display: hero, section heads, feature titles, band) + **Work Sans** (body/UI). Eyebrows: Work Sans uppercase ~13px, letter-spacing ~0.14em, in `--primary`/`--accent`. Never Fraunces, never Inter-as-identity.

**Layout:** header (wordmark + mark, nav, one CTA) · hero (two columns: copy left, pure-CSS product panel right) · proof strip (label + 5 glyph logos) · 3 feature cards with index numerals (`01 / 03`) · dark closing CTA band · footer. Radius `--r-sm/md/lg` = 10/16/22. Soft restrained shadows.

**Motion (~6):** gentle hover lifts/scales + soft fade-ups via CSS transitions. No scroll libraries, no JS for these. Reveals rest visible (`animation-fill-mode: forwards`). Honor `prefers-reduced-motion`.

Canonical implementation: [`reference/template.html`](reference/template.html). Match it; don't reinvent it.

---

## 3. Token system (color is fully tokenized)

**Hard rule: every color is a token. Zero hardcoded color literals except in `:root` and the theme override blocks.** A single hardcoded color is a bug — it will not theme or dark-swap. Grep before delivering.

**Token contract** (see [`reference/DESIGN.md`](reference/DESIGN.md) §2 for the full table):
surfaces `--bg / --bg-deep / --bg-cool / --bg-rgb / --card`; brand `--primary / --primary-2 / --primary-line / --primary-rgb / --primary-2-rgb`; accent `--accent / --accent-soft / --accent-rgb`; band `--band-a / --band-b`; text `--ink / --ink-soft / --ink-mute`; on-dark `--on-dark`; interactive `--btn-bg / --btn-fg`; derived `--hairline` (= `rgba(var(--primary-rgb),0.12)`) and `--shadow-rgb`.

- Tints use the `-rgb` triplets: `rgba(var(--primary-rgb), 0.12)` — never raw numbers.
- Solid buttons use `--btn-bg`/`--btn-fg` (decoupled so dark mode flips them).
- Inline SVG colors are set via CSS (`stroke`/`fill: var(--…)` or `currentColor`), never hardcoded attributes.

**Palette library** (full light+dark values in [`reference/palettes.css`](reference/palettes.css); names the user can say):
- **Oxblood** — default (burgundy / warm stone / bronze).
- **Evergreen** — forest / porcelain / brass.
- **Aubergine** — plum / mauve / gold.

Swap a palette by replacing the `:root` block. Add one by defining the same tokens + a dark counterpart.

---

## 4. Responsive + dark mode (required on every build)

**Dark mode** ships on every build. Provide dark tokens in BOTH:
- `[data-theme="dark"] { … }` (manual toggle), and
- `@media (prefers-color-scheme: dark){ :root:not([data-theme="light"]) { … } }` (automatic).
Dark values for all palettes are in `palettes.css`. Verify: body text light, cards dark surfaces, primary buttons use the accent `--btn-bg` with dark `--btn-fg`, hairlines lighten.

**Responsive** down to 360px. Breakpoints: ≤1024 (tighten, reduce hero size) · ≤900 (hero → 1 col, nav condenses to wordmark+CTA, features → 2 col) · ≤760 (drop the decorative hero panel) · ≤700 (features → 1 col) · ≤600 (fluid `clamp()` type, ≥44px tap targets, 20px gutters). Add `min-width:0` to flex/grid children so they shrink. No horizontal overflow. No JS.

---

## 5. Components

Full set demonstrated in [`reference/components.html`](reference/components.html), all tokenized + responsive + dark-capable: buttons (primary/ghost, sizes, states), forms (input/textarea/select/checkbox/inline subscribe, focus rings), navigation, cards (content + stat), badges/tags, and a 3-tier pricing table (one featured). Reuse these patterns; keep the same token names.

---

## 6. Anti-AI-tell rulebook (never ship these)

The full catalog lives in [`reference/DESIGN.md`](reference/DESIGN.md) §6. Summary of hard "no"s:

**Copy / voice**
- **No em-dashes** — rewrite with comma / period / colon.
- No triadic slogans or reflexive rule-of-three; no "It's not just X, it's Y", "Say goodbye to…", "The best part?".
- No filler openers ("In today's fast-paced world", "In the ever-evolving landscape") or filler words (unlock, unleash, elevate, supercharge, seamless, robust, cutting-edge, game-changing, effortlessly).
- Sentence case headings; specific nouns/numbers over superlatives; no emoji in headings/CTAs/labels; no placeholder residue.

**Typography**
- No italic-last-word emphasis (use weight or `--accent`). No random ALL-CAPS beyond small tracked eyebrows. No three identical feature cards — vary rhythm.

**Visual**
- No AI-purple/violet or mesh-gradient blobs; no cream+terracotta default; no all-Inter+slate identity; no glassmorphism-on-everything; no dead-center-on-dark hero by reflex; no stock 3D gradient blobs. Consistent icon family, never emoji-as-icons.

**Motion**
- No infinite looping micro-animations everywhere; no auto-parallax.

**Code**
- No hardcoded colors (full tokenization); no dead reveals; no leftover TODO/placeholder comments; semantic HTML, `alt` text, labeled inputs, visible focus, AA contrast, ≥44px tap targets.

---

## 7. DESIGN.md output (required deliverable)

Every build emits a project `DESIGN.md` documenting the design system, so the work is extensible for future development and branding. Use [`reference/DESIGN.md`](reference/DESIGN.md) as the skeleton and fill it for the project: Overview → Principles → Foundations (color tokens + palettes + dark, type, layout/spacing, radius, elevation, motion) → Components → Content & Voice (with the anti-tell rules) → Accessibility → Theming & Stack → Changelog. Replace product name, copy, and any brand-specific values; keep the structure and token names.

---

## 8. Pre-flight checklist

- [ ] Stack resolved (detected or interviewed); output matches it.
- [ ] Design read stated with palette (Oxblood unless told otherwise).
- [ ] Spectral + Work Sans. Structure matches the reference.
- [ ] **Every** color is a token; grep shows literals only in `:root`/theme blocks/CSS-overridden SVG.
- [ ] Swapping `:root` reskins the entire page; dark mode present in both `[data-theme]` and `@media`.
- [ ] Responsive to 360px, no horizontal overflow, ≥44px tap targets.
- [ ] Anti-AI-tell rulebook honored (em-dashes gone, no italic-last-word, no filler, etc.).
- [ ] `DESIGN.md` emitted for the project.

---

## Reference files

- [`reference/template.html`](reference/template.html) — canonical landing page: tokenized, responsive, light+dark. Start here.
- [`reference/components.html`](reference/components.html) — component gallery (buttons, forms, nav, cards, badges, pricing), light+dark.
- [`reference/palettes.css`](reference/palettes.css) — Oxblood / Evergreen / Aubergine, light + dark, copy-paste swappable.
- [`reference/DESIGN.md`](reference/DESIGN.md) — the design-system doc and the skeleton every build emits.
