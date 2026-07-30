# DESIGN.md — Simba-Taste Design System

> This is the design-system document Simba-Taste produces for every project. It is the single source of truth for foundations, components, content voice, and theming, written so a designer or another agent can extend the product without re-deriving decisions. The version below documents the reference build (Northwind, Oxblood palette); a real project replaces the product name, copy, and any brand-specific values while keeping the structure.

- **System:** Simba-Taste (refined editorial-premium)
- **Version:** 1.0
- **Default palette:** Oxblood · **Alternates:** Evergreen, Aubergine
- **Fonts:** Spectral (display) · Work Sans (text)
- **Themes:** Light (default) + Dark
- **Stack:** _resolved per project — HTML/CSS · Tailwind · React/Next (see §8)_

---

## 1. Design principles

1. **Premium through restraint.** Space, type, and hierarchy do the work; decoration does not.
2. **Editorial, not templated.** Every page should read as deliberately composed, never like a stock generator.
3. **One system, many palettes.** Layout and type are fixed; color is a swappable token set.
4. **Tokens are law.** Every color, radius, and shadow is a token. No hardcoded values — this is what makes theming and dark mode reliable.
5. **Human voice.** Copy avoids the tells that mark AI-generated work (see §6).
6. **Accessible by default.** AA contrast, real focus states, keyboard-navigable, reduced-motion aware.

---

## 2. Foundations — Color

Color is expressed as **design tokens** in `:root`. Swapping a palette means replacing these values; dark mode overrides them under `[data-theme="dark"]` and `@media (prefers-color-scheme: dark)`.

### Token contract

| Token | Role |
|---|---|
| `--bg`, `--bg-deep`, `--bg-cool`, `--bg-rgb` | Page background family (+ rgb triplet for tints) |
| `--card` | Card / panel surface |
| `--primary`, `--primary-2`, `--primary-line` | Brand color + variants |
| `--primary-rgb`, `--primary-2-rgb` | Triplets for `rgba()` tints |
| `--accent`, `--accent-soft`, `--accent-rgb` | Secondary accent (metallic) |
| `--band-a`, `--band-b` | Dark closing-CTA band gradient |
| `--on-dark` | Text/marks on dark surfaces |
| `--ink`, `--ink-soft`, `--ink-mute` | Text primary / secondary / tertiary |
| `--btn-bg`, `--btn-fg` | Solid button surface + label (decoupled so themes can flip it) |
| `--hairline` | 1px dividers — `rgba(var(--primary-rgb), 0.12)` |
| `--shadow-rgb` | Shadow tint triplet |

**Tint rule:** never write raw `rgba(r,g,b,a)`. Use `rgba(var(--primary-rgb), 0.12)` etc. **SVG rule:** inline SVG marks are colored via CSS (`stroke`/`fill: var(--…)` or `currentColor`), never hardcoded attributes, so themes reskin them too.

### Palettes (light → dark)

**Oxblood (default)** — deep burgundy, warm stone, bronze.
`--primary #4A1D22` · `--accent #B07A34` · `--bg #F2EDE6` · dark `--bg #1D1311`, `--btn-bg #B68544`.

**Evergreen** — forest, warm porcelain, brass. `--primary #1F3D2F` · `--accent #B98A3E` · `--bg #F4F2ED`.

**Aubergine** — plum, mauve paper, gold. `--primary #3A2340` · `--accent #C6A24E` · `--bg #F3EEF1`.

Full values (light + dark for all three) live in [`palettes.css`](palettes.css). To add a palette, define the same tokens and a dark counterpart.

---

## 3. Foundations — Typography

| Role | Font | Notes |
|---|---|---|
| Display (h1, section h2, feature titles, band) | **Spectral** 300–600 | Editorial serif. Emphasis via weight or `--accent` color — never italic-last-word. |
| Body / UI | **Work Sans** 400–600 | |
| Eyebrows / labels | Work Sans | Uppercase, ~13px, letter-spacing ~0.14em, in `--primary`/`--accent`. |

- Fluid sizing via `clamp()` on mobile.
- Body measure ~60–70ch; headings sentence case.

---

## 4. Foundations — Layout, spacing, elevation, motion

- **Grid:** centered `.wrap`, max-width 1200px, 40px gutters (32 at ≤1024, 20 at ≤600).
- **Radius:** `--r-sm 10px`, `--r-md 16px`, `--r-lg 22px`.
- **Elevation:** three soft shadows tinted by `--shadow-rgb`. Restrained; never glowy.
- **Hairlines:** 1px `--hairline` dividers between major sections.
- **Motion:** gentle only — hover lifts/scales and soft fade-ups via CSS transitions. No scroll libraries, no JS for these effects. Reveals rest visible. Honor `prefers-reduced-motion`.
- **Breakpoints:** ≤1024 (tighten), ≤900 (hero → 1 col, nav condenses, features → 2 col), ≤760 (drop decorative hero panel), ≤700 (features → 1 col), ≤600 (fluid type, 44px tap targets, 20px gutters).

---

## 5. Components

Documented and demonstrated in [`components.html`](components.html):

- **Buttons** — primary (`--btn-bg`/`--btn-fg`), ghost/secondary (outlined via `--primary-line`), sizes sm/md, states hover/active/disabled.
- **Forms** — labeled input, textarea, select, checkbox, inline subscribe; focus rings via `rgba(var(--accent-rgb), …)`.
- **Navigation** — wordmark + links + CTA over a hairline; condenses under 640px.
- **Cards** — content card + stat/metric card (Spectral numerals).
- **Badges / tags** — pill labels in primary and accent tints.
- **Pricing** — three tiers, one featured (accent border/lift), Spectral prices, per-tier CTA; 3 cols → 1 on mobile.

Canonical landing page: [`template.html`](template.html) (header · hero + product panel · proof strip · 3 index-numbered features · dark CTA band · footer).

---

## 6. Content & Voice — anti-AI-tell rulebook

Copy and composition must not carry the signatures of machine-generated work. Hard rules:

**Punctuation & structure**
- **No em-dashes.** Rewrite with a comma, period, or colon.
- No triadic slogans ("Fast. Simple. Reliable.") or reflexive rule-of-three.
- No "It's not just X, it's Y", "Say goodbye to…", "The best part?", "But here's the thing:".

**Openers & filler**
- Banned openers: "In today's fast-paced world", "In the ever-evolving landscape of…", "Whether you're X or Y".
- Banned verbs/adjectives as filler: unlock, unleash, elevate, supercharge, revolutionize, seamless, robust, cutting-edge, game-changing, effortlessly, simply.

**Tone**
- Sentence case for headings (not Title Case Everything). Specific nouns and numbers over vague superlatives. Contractions are fine — write like a person.
- No emoji in headings, CTAs, or UI labels.
- No placeholder residue ("Lorem ipsum", "Company Name", "Your Text Here").

**Typographic tells**
- No italic-last-word emphasis. No random ALL-CAPS beyond small tracked eyebrows.
- No three identical icon+title+line feature cards; vary rhythm (index numerals, asymmetry).

**Visual tells**
- No AI-purple/violet or mesh-gradient hero blobs; no cream+terracotta default; no all-Inter+slate identity.
- No glassmorphism-on-everything; no dead-center-on-dark hero by reflex; no stock 3D gradient blobs (prefer CSS/SVG or real product UI).
- Consistent single icon family and stroke width; never emoji as icons.

**Motion tells**
- No infinite looping micro-animations everywhere; no auto-parallax on everything.

---

## 7. Accessibility

- Text contrast meets WCAG AA in both themes (verify `--ink` on `--bg`, `--btn-fg` on `--btn-bg`).
- Visible focus rings on all interactive elements. Labels programmatically tied to inputs.
- Tap targets ≥44px. Keyboard operable. `prefers-reduced-motion` disables non-essential motion.
- Semantic HTML; meaningful `alt`; logical heading order.

---

## 8. Theming & stack

- **Switch palette:** replace the `:root` token block with one from `palettes.css`.
- **Dark mode:** ships via `[data-theme="dark"]` (manual) mirrored under `@media (prefers-color-scheme: dark)` (automatic).
- **Add a palette:** define the full token set + a dark counterpart; document it here.
- **Stack:** the reference is framework-free HTML/CSS. For Tailwind, map tokens to `theme.extend.colors` referencing the CSS variables; for React/Next, expose tokens via a `:root` stylesheet or CSS-in-JS theme and keep the same names.

---

## 9. Changelog

- **1.0** — Initial system: Oxblood/Evergreen/Aubergine palettes, light+dark, Spectral+Work Sans, full component set, anti-AI-tell rulebook, responsive foundations.
