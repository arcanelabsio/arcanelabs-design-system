---
name: Arcane Labs design system
description: Design tokens, component stylesheet, and brand assets shared across every Arcane Labs app and site. Apps wire the classes into their own framework. Terminal-window editorial aesthetic, monospace throughout, dark-only, mint-and-amber accents on near-black.
---

# Arcane Labs — Agent Skill

Use this skill when the user asks for work **in the Arcane Labs visual language** — rebuilding the company site, making new pages for it (about, a new product page, a post), designing an Arcane Labs app UI, or creating marketing collateral.

Everything you need is in this folder:

- **`README.md`** — full system documentation. Brand context, voice & tone, visual foundations, iconography, caveats. **Read this first.**
- **`colors_and_type.css`** — design tokens as CSS custom properties. `@import` or copy.
- **`editorial.css`** — the full component stylesheet. Two families:
  - Editorial (site) classes: `.lh`, `.lh__chrome`, `.lh__nav`, `.lh__list`, `.lh__post`, `.lh__sep`, `.lh__meta`, etc.
  - App-surface classes (added in v0.3.0): `.lh__btn` (primary/secondary/ghost/destructive variants, with hover/focus/disabled/loading states and 3 sizes), `.lh__field` (input/select/textarea + label + helper, with focus/error/disabled states), `.lh__btn-row`, `.lh__field-row`.
  - Both families share tokens; mix freely.
- **`assets/`** — favicon + the four icons (GitHub, Instagram, X, Mail).
- **`preview/`** — 29 self-contained HTML specimens. Open any in a browser to see the live appearance. They double as the visual contract — token or stylesheet changes that alter how these render are breaking.
- **`docs/web-apps.md`** — when to reach for which class family; state conventions; keyboard expectations; markup patterns; voice in app copy.
- **`docs/accessibility.md`** — computed WCAG contrast table; touch-target floor; focus / motion / ARIA / keyboard rules.

This is a **web-only** design system. Mobile apps in the org (longeviti, vael) maintain their own design systems on purpose. Don't try to bend this skill to mobile/Flutter/Swift work.

For a worked example of the system composed into a real app, see the live [arcanelabs.info](https://arcanelabs.info) site (or its source repo). This repo deliberately ships **no implementation code** — apps wire the classes into their own framework.

## How to use it

1. **Read `README.md` in full** before producing anything — voice and micro-conventions (prefix glyphs, bracketed labels, em-dash heavy copy, period-terminated status labels) are load-bearing.
2. **Pick the right class family for the task.** Editorial (site, post, hero, nav, list) for content surfaces. App-surface (`.lh__btn`, `.lh__field`) for interactive web-app surfaces. Both share tokens; mix freely. See `docs/web-apps.md` for the family-by-context table.
3. **Read `docs/accessibility.md` before shipping any interactive surface.** Contrast, touch targets, focus, ARIA, motion. Non-negotiable.
4. **Link both stylesheets** in that order:
   ```html
   <link rel="stylesheet" href="colors_and_type.css">
   <link rel="stylesheet" href="editorial.css">
   ```
   …or copy the files next to your new HTML and adjust paths.
5. **Use the real class names.** Every component is pre-named in `editorial.css` — don't invent parallel class systems. Editorial pages are almost always `.lh > .lh__chrome + .lh__inner > .lh__nav + screens + .lh__footer`. App-surface pages compose `.lh__field` blocks inside `<form>` and use `.lh__btn-row` for action groups.
6. **Fonts**: Load JetBrains Mono from Google Fonts (preconnect + `?family=JetBrains+Mono:ital,wght@0,400;0,500;0,600;0,700;1,400;1,500`). This is what the live site ships; there are no vendored `.woff2` files. If the user needs a local build, flag the decision and ask.
7. **Copy is part of the design.** Match the voice samples in README. Every label, status, rule is period-terminated. Backticks for product names in prose. Em-dashes everywhere. App copy follows the same voice — see `docs/web-apps.md` §5.
8. **No imagery, no illustration, no gradient fills.** The only background texture is the 40-px mint grid at ~1.5% alpha. The only images in the system are the favicon and four inline SVG icons.

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
