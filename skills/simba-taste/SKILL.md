---
name: simba-taste
description: Kevin's personal frontend taste. A single locked "refined editorial-premium" style with a swappable palette library (Oxblood default, Evergreen and Aubergine alternates). Every color is a design token so palettes fully propagate. Hard anti-AI-tell rules baked in. Use for landing pages, marketing sites, and portfolios that must look intentional and human-made — not templated.
---

# Simba-Taste — Refined Editorial-Premium Frontend

> One opinionated style, done well, in a small library of palettes.
> This skill does not offer a spectrum of aesthetics — it encodes a single point of view and makes **color** the only dial you turn per project.

Use this for: landing pages, marketing sites, portfolios, redesigns where the brief is "make it look premium and distinctive."
Not for: dashboards, dense data tables, or multi-step product UI.

---

## 0. The one-line design read

Before building, state: *"Building in Simba-Taste, `<palette>` palette."* Default palette is **Oxblood**. Only switch palette if the user names one or the brand demands it. Do not re-derive the aesthetic — it is fixed below.

---

## 1. The locked style (do not renegotiate)

**Feel:** expensive, warm, editorial, calm-confident. Premium through restraint and typography, not decoration.

**Dials (fixed baseline):** `DESIGN_VARIANCE 7 / MOTION 6 / VISUAL_DENSITY 4`.

**Type:**
- Display: **Spectral** (a refined editorial serif). Weights 300–600. Used for the hero headline, section headlines, feature titles, and the closing band.
- Body / UI: **Work Sans**. Weights 400–600.
- Never Fraunces (too defaulted), never Inter-as-identity.
- Section eyebrows: Work Sans, uppercase, ~13px, letter-spacing ~0.14em, in `--primary` or `--accent`.

**Layout system:**
- **Header:** wordmark + logo mark (left), nav links + one solid CTA (right), hairline bottom border.
- **Hero:** two columns. Left = eyebrow, large Spectral headline (one clean statement), body sub, primary + ghost CTA, a small reassurance line. Right = a **floating product panel** built in pure CSS/SVG (rounded, soft-shadowed, palette-tinted) with a couple of floating status chips.
- **Proof strip:** "Trusted by teams at" + 5 text logos, each with a small inline-SVG glyph. Hairline top and bottom.
- **Features:** 3 cards, medium radius, soft shadow, each with a small icon tile, an index numeral (`01 / 03`), a Spectral title, and one body line.
- **Closing CTA band:** full-width **dark** band (`--band-a` → `--band-b` gradient) with a Spectral headline (accent-colored key word), a light button, and a faint grid/glow texture.
- **Footer:** wordmark, © line, a few small links. Hairline top.

**Radius / depth / motion:**
- Radius: `--r-sm 10px / --r-md 16px / --r-lg 22px`. Rounded, but not pill-shaped.
- Shadows: soft and restrained (`--shadow-sm/md/lg`), never glowy.
- Motion (~6/10): gentle hover lifts/scales and soft fade-up reveals via CSS transitions. **No scroll libraries.** If you use a fade-up reveal, its resting state MUST be visible (`animation-fill-mode: forwards`, keyframes ending at `opacity:1`) — never leave content stuck at `opacity:0`.

The canonical implementation is [`reference/template.html`](reference/template.html). Match its structure; do not reinvent it.

---

## 2. Color is the only variable — and it is fully tokenized

**Hard rule: every color in the output is a CSS custom property. Zero hardcoded color literals anywhere except the `:root` block.** This is non-negotiable — it is the whole reason palettes work. A page that hardcodes even one color (a gradient stop, a border tint, an SVG stroke) will only half-swap and is considered broken.

### The token contract (21 tokens)

```css
:root {
  /* surfaces */
  --bg; --bg-deep; --bg-cool; --bg-rgb; --card;
  /* brand */
  --primary; --primary-2; --primary-line; --primary-rgb; --primary-2-rgb;
  --accent; --accent-soft; --accent-rgb;
  /* dark closing band + text on dark */
  --band-a; --band-b; --on-dark;
  /* ink (text) — shared across palettes */
  --ink; --ink-soft; --ink-mute;
  /* derived */
  --hairline: rgba(var(--primary-rgb), 0.12);
  --shadow-rgb;
}
```

Rules that make full propagation work:
- All tints use the `-rgb` triplet tokens: `rgba(var(--primary-rgb), 0.12)`, `rgba(var(--accent-rgb), 0.2)`, `rgba(var(--shadow-rgb), 0.1)`. Never write a raw `rgba(r,g,b,a)` with numbers.
- **Inline SVG colors** (logo mark, proof-strip glyphs, icons) cannot read `var()` from presentation attributes. Drive them with CSS instead — either `stroke="currentColor"` (inherits token-driven `color`) or targeted rules that override the attribute, e.g. `.logo .glyph svg [stroke]{ stroke: var(--primary) }`. CSS beats presentation attributes, so `:root` swaps reskin the marks too.
- The dark closing band uses `--band-a`/`--band-b`, never a hardcoded green/dark.

### The palette library

Swap palettes by replacing the color tokens in `:root` (see [`reference/palettes.css`](reference/palettes.css) for the three complete blocks). Names the user can say: **"Oxblood"**, **"Evergreen"**, **"Aubergine"**.

**Oxblood — default.** Deep burgundy, warm stone, bronze.
```css
--bg:#F2EDE6; --bg-deep:#EAE3D9; --bg-cool:#EDE7DE; --bg-rgb:242,237,230; --card:#F8F5F0;
--primary:#4A1D22; --primary-2:#6E2E34; --primary-line:#833C42; --primary-rgb:74,29,34; --primary-2-rgb:110,46,52;
--accent:#B07A34; --accent-soft:#C6924B; --accent-rgb:176,122,52;
--band-a:#632A2F; --band-b:#2B1114; --on-dark:#F3ECE4;
--ink:#1D1718; --ink-soft:#4E4644; --ink-mute:#746A66; --shadow-rgb:43,17,20;
```

**Evergreen.** Deep forest, warm porcelain, brass.
```css
--bg:#F4F2ED; --bg-deep:#EDEBE3; --bg-cool:#E9EAE3; --bg-rgb:244,242,237; --card:#FBFAF6;
--primary:#1F3D2F; --primary-2:#2E5A44; --primary-line:#33543F; --primary-rgb:31,61,47; --primary-2-rgb:46,90,68;
--accent:#B98A3E; --accent-soft:#C79B52; --accent-rgb:185,138,62;
--band-a:#294F3C; --band-b:#12241B; --on-dark:#F3F1EA;
--ink:#1D1718; --ink-soft:#4E4644; --ink-mute:#746A66; --shadow-rgb:20,38,29;
```

**Aubergine.** Plum, mauve paper, gold.
```css
--bg:#F3EEF1; --bg-deep:#ECE4E9; --bg-cool:#EEE7EC; --bg-rgb:243,238,241; --card:#FBF7FA;
--primary:#3A2340; --primary-2:#5A3A62; --primary-line:#6C4874; --primary-rgb:58,35,64; --primary-2-rgb:90,58,98;
--accent:#C6A24E; --accent-soft:#D4B468; --accent-rgb:198,162,78;
--band-a:#513457; --band-b:#221425; --on-dark:#F1EAF0;
--ink:#1D1718; --ink-soft:#4E4644; --ink-mute:#746A66; --shadow-rgb:40,28,44;
```

To add a palette later, define the same tokens. Keep `--ink*` shared unless there is a reason not to.

---

## 3. Anti-default rulebook (Kevin's tells — never ship these)

These come from direct review. Each is a hard "no."

1. **No italic-last-word headline.** Do not italicize the final word of the hero headline (or any headline). The headline is one coherent statement. Emphasize a single word with **weight** or the **`--accent`/`--primary` color**, never italics.
2. **No Anthropic-adjacent palette.** Never cream/ivory background + terracotta/coral/clay/rust/amber accent. (The default palettes already avoid this — do not drift back to it.)
3. **No AI-purple / violet gradients**, no generic glassmorphism-on-everything, no Inter+slate-900 as the identity.
4. **No default serifs for display.** Spectral (or Newsreader) — not Fraunces.
5. **No half-tokenized color.** If any color is hardcoded outside `:root`, it is a bug. Grep the output for stray hex/`rgba(` before calling it done.
6. **No dead reveals.** Any fade/scroll-in animation must rest visible.

---

## 4. Pre-flight checklist (run before delivering)

- [ ] Design read stated with palette name (Oxblood unless told otherwise).
- [ ] Type is Spectral (display) + Work Sans (body). No Fraunces, no Inter-identity.
- [ ] Structure matches the reference: header · hero+panel · proof strip · 3 index-numbered feature cards · dark CTA band · footer.
- [ ] **Every** color is a token. `grep -nEi '#[0-9a-f]{3,6}|rgba?\([0-9]'` returns hits ONLY inside `:root` and CSS-overridden SVG fallbacks.
- [ ] Swapping the `:root` block to another palette reskins the **entire** page — hero, cards, closing band, footer, logo mark, and proof-strip glyphs.
- [ ] No italic-last-word. No AI-purple. No dead reveals.
- [ ] Motion is gentle (hover + soft fade-up), no scroll libraries.

---

## Reference files

- [`reference/template.html`](reference/template.html) — the canonical Simba-Taste page (Oxblood default), fully tokenized. Start here.
- [`reference/palettes.css`](reference/palettes.css) — the three palette `:root` blocks, copy-paste swappable.
