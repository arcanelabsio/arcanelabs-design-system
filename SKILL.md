---
name: Arcane Labs design system
description: Design tokens, components, and UI kit ported from the arcanelabs.info company site (Vite + React + TypeScript). Use for any Arcane Labs brand work — studio site content, project pages, app UI in the Arcane Labs visual language. Terminal-window editorial aesthetic, monospace throughout, dark-only, mint-and-amber accents on near-black.
---

# Arcane Labs — Agent Skill

Use this skill when the user asks for work **in the Arcane Labs visual language** — rebuilding the company site, making new pages for it (about, a new product page, a post), designing an Arcane Labs app UI, or creating marketing collateral.

Everything you need is in this folder:

- **`README.md`** — full system documentation. Brand context, voice & tone, visual foundations, iconography, caveats. **Read this first.**
- **`colors_and_type.css`** — design tokens as CSS custom properties. `@import` or copy.
- **`editorial.css`** — the full component stylesheet ported verbatim from `src/styles/editorial.css` in the source repo. Class-based (`.lh`, `.lh__chrome`, `.lh__nav`, `.lh__list`, `.lh__post`, `.lh__sep`, `.lh__meta`, etc). Depends on tokens in `colors_and_type.css`.
- **`ui_kits/website/`** — a working, static rebuild of the company site. Six screens + 404, hash-routed. Use as a vocabulary reference and as a starting skeleton for new screens.
- **`assets/`** — favicon + the four icons (GitHub, Instagram, X, Mail).
- **`preview/`** — small spec cards for each token and component. Useful if the user wants to see a specific piece in isolation.

## How to use it

1. **Read `README.md` in full** before producing anything — voice and micro-conventions (prefix glyphs, bracketed labels, em-dash heavy copy, period-terminated status labels) are load-bearing.
2. **Link both stylesheets** in that order:
   ```html
   <link rel="stylesheet" href="colors_and_type.css">
   <link rel="stylesheet" href="editorial.css">
   ```
   …or copy the files next to your new HTML and adjust paths.
3. **Use the real class names.** Every component is pre-named in `editorial.css` — don't invent parallel class systems. New pages are almost always `.lh > .lh__chrome + .lh__inner > .lh__nav + screens + .lh__footer`.
4. **Fonts**: Load JetBrains Mono from Google Fonts (preconnect + `?family=JetBrains+Mono:ital,wght@0,400;0,500;0,600;0,700;1,400;1,500`). This is what the live site ships; there are no vendored `.woff2` files. If the user needs a local build, flag the decision and ask.
5. **Copy is part of the design.** Match the voice samples in README. Every label, status, rule is period-terminated. Backticks for product names in prose. Em-dashes everywhere.
6. **No imagery, no illustration, no gradient fills.** The only background texture is the 40-px mint grid at ~1.5% alpha. The only images in the system are the favicon and four inline SVG icons.

## Quick starters

**New screen in the UI kit pattern:**
```html
<main class="lh">
  <div class="lh__chrome">
    <span class="lh__dot lh__dot--red"></span>
    <span class="lh__dot lh__dot--amber"></span>
    <span class="lh__dot lh__dot--green"></span>
    <span class="lh__chrome-title"><strong>arcanelabsio</strong> — zsh — {page}</span>
  </div>
  <div class="lh__inner">
    <nav class="lh__nav">…</nav>
    <div class="lh__hero">
      <h1 class="lh__name"># {Title}</h1>
      <p class="lh__greeting">&gt; {one-sentence lead}</p>
      <hr class="lh__rule">
    </div>
    {content}
    <footer class="lh__footer">…</footer>
  </div>
</main>
```

**Section break inside content:**
```html
<div class="lh__sep">── <strong>LABEL</strong> ──────────────────────────────────────</div>
```

**Project meta block:**
```html
<dl class="lh__meta">
  <dt>[STATUS]</dt>  <dd>Shipping.</dd>
  <dt>[VERSION]</dt> <dd>v1.2.3 — 2026-03-20</dd>
</dl>
```

## Guardrails

- **No emoji in prose.** The one allowed emoji is `📄` as a file-icon pseudo-element in `.lh__post__crumb::before` — and only there.
- **Don't introduce new accent hues.** Mint and amber do everything; red is reserved for errors; blue only appears inside code tokens.
- **Don't add shadows beyond the `--shadow-window` token.** Cards sit flat, rows use a 2-px mint left border, that's it.
- **No motion beyond the three tokens** (`--t-fast`, `--t-mid`, `--t-slow`). No spring, no bounce, no scroll-driven animation. The only infinite animation is `.lh__cursor`.
- **Don't invent new icons.** If you need one beyond the four in `assets/icons/`, match their weight (fill brand marks, 2-px stroke UI icons, 24×24 viewBox at source).

## When the user asks for something this skill can't cover

If the ask is a fundamentally different medium (an app, a slide deck, a printable PDF) in the Arcane Labs language — use the tokens and vocabulary from this skill, but you'll need to build new components. Flag that to the user and propose a direction before building.

If the ask is for a **different brand** entirely, don't use this skill — pick or create a design system that fits.
