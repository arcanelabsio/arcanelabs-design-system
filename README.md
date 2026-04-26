# Arcane Labs — Design System

> **New here?** Read **[`docs/getting-started.md`](docs/getting-started.md)** first
> — quick start, worked example, reading order, common pitfalls.
> 5 minutes; you'll know everything you need to ship.

> Terminal-window editorial. Monospace, dark, hairline-ruled, with
> mint-and-amber accents on a near-black background. The whole
> vocabulary is one idea — *this page is a zsh session in a macOS
> terminal window* — rendered with restraint.

Arcane Labs is an independent software studio building privacy-first,
local-first applications and developer tools. The studio publishes
apps (Longeviti, Vael), Dart/Flutter libraries (`cloud_sync`,
`drive_sync_flutter`), CLIs (`forge`, `shellcraft`), and the
[arcanelabs.info](https://arcanelabs.info) company site.

This repo is the **canonical source of truth** for the Arcane Labs
visual language — design tokens, component stylesheet, brand assets,
reference components, and the UI kit. It was extracted from the
company site (`arcanelabsio/arcanelabs.info`, a Vite + React +
TypeScript SSG) so multiple consumers can vendor the same visual
language without each one re-discovering it.

Today's consumers:

- [`arcanelabsio/arcanelabs.info`](https://github.com/arcanelabsio/arcanelabs.info)
  — the live company site. The originating implementation.
- [`arcanelabsio/recallable`](https://github.com/arcanelabsio/recallable)
  — privacy-first recall app. Vendors the tokens + stylesheet.

When more consumers come online, the longer-term shape (out of scope
today) is to publish this as `@arcanelabs/design-system` on npm or pin
it as a git submodule. For now, vendoring + `CHANGELOG.md` discipline
is sufficient — see **Using this repo** below.

---

## Index

Root files:

- **`README.md`** — this file. Brand context, content fundamentals,
  visual foundations, iconography.
- **`SKILL.md`** — Agent Skill entry point. Also doubles as the
  front door when this folder is used as a Claude Code skill.
- **`colors_and_type.css`** — all design tokens (colors, type,
  spacing, radii, shadows, motion) exposed as CSS custom properties,
  plus semantic defaults (body / h1 / h2 / p / a / code / pre).
- **`editorial.css`** — full component stylesheet ported from the
  live site. Class-based (`.lh`, `.lh__chrome`, `.lh__nav`,
  `.lh__list`, etc). Depends on the tokens in `colors_and_type.css`.
- **`assets/`** — favicon and the four inline-extracted brand icons
  (GitHub, Instagram, X, Mail) as standalone SVGs.
- **`preview/`** — 29 self-contained HTML specimens, one per token
  or component. Open any of them in a browser to see the live
  appearance. Together they document the visual contract:
  consumers should treat changes that alter how these specimens
  render as breaking.
- **`docs/`** — operator and consumer documentation:
  - **`getting-started.md`** — front door. Quick start, worked
    example, reading order, common pitfalls, the contract. Read
    this first.
  - `web-apps.md` — when to reach for which class family; state
    conventions; keyboard expectations; markup patterns.
  - `accessibility.md` — contrast table (computed, not handwaved),
    touch-target floor, focus / motion / keyboard rules.
  - `responsive.md` — the four-breakpoint system, `clamp()` vs
    `@media`, container widths, desktop-first convention, full
    inventory of behavior at each threshold.
- **`CHANGELOG.md`** — every change to tokens, stylesheet, or assets.
  Consumers that vendor this should pin to a row.
- **`STATE.md`** — operator-local current-phase tracker. Not part
  of the design contract; safe to ignore from a consuming app.

Reference implementations are in the consuming web apps. The first
is [`arcanelabsio/arcanelabs.info`](https://github.com/arcanelabsio/arcanelabs.info)
(Vite 5 + React 18 + TypeScript + react-router 6 + SSG prerender) —
look there if you need to see how the classes are wired into a real
React app's JSX. The second is `arcanelabsio/recallable`. Both are
web apps; this repo ships **no implementation code**.

> ⚠️ **Mobile / Flutter is out of scope.** The org's mobile apps
> (`longeviti-app`, `vael`) maintain their own design systems by
> design — Material 3, different palettes, different type families.
> Don't try to bend this repo to serve them. This is a **web-only**
> design system.

---

## Using this repo

This repo is **vendored, not installed**. There is no `package.json`,
no build step, no published artifact. The two ways to consume it:

### A. Copy the stylesheet pair (lightest)

For most consumers, the deliverable is two CSS files:

```bash
# from the consuming repo's root
curl -sSL https://raw.githubusercontent.com/arcanelabsio/arcanelabs-design-system/main/colors_and_type.css \
  > src/styles/arcane-tokens.css
curl -sSL https://raw.githubusercontent.com/arcanelabsio/arcanelabs-design-system/main/editorial.css \
  > src/styles/arcane-editorial.css
```

Then link them in that order — tokens first, then components:

```html
<link rel="stylesheet" href="/styles/arcane-tokens.css">
<link rel="stylesheet" href="/styles/arcane-editorial.css">
```

Record the source SHA in your consumer's `CHANGELOG.md` so the next
sync diff is mechanical.

### B. Vendor the whole tree (richest)

Drop the entire repo in as `vendor/arcanelabs-design-system/` and
import from there. You get the tokens, stylesheet, brand assets,
and the preview specimens — useful when you need to inspect
component appearance in isolation while debugging an integration.

Either way: when this repo's `CHANGELOG.md` ships a row, the consumer
ships the matching update. No silent drift.

### What this repo is not

- **Not a runnable app.** No build step, no entry point, no JSX.
  Apps wire the classes into their own framework of choice.
- **Not an npm package.** Yet. See the note in the intro.
- **Not a place to land app-specific implementations.** Routes,
  content, and feature code belong in the consuming repo. This
  one ships shared visual vocabulary only.
- **Not a cross-platform reference.** Web only. Mobile apps in the
  org maintain their own design systems on purpose.

---

## Web-app consumption

v0.3.0 added the first **app-surface primitives** on top of the
editorial system. If you're building a web app surface (forms,
dashboards, settings) inside an arcanelabsio repo, reach for these
first; they layer on the same tokens as the editorial classes, so
mixing is free.

### Today's app primitives

- **`.lh__btn`** — buttons. Variants: `--primary`, default
  (`secondary`), `--ghost`, `--destructive`. Sizes: `--sm` (32px),
  default (44px), `--lg` (52px). States: hover, focus, active,
  `[disabled]`, `[data-loading="true"]`. See `preview/btn.html`.
- **`.lh__field`** — form fields. Wrapper + `__label` + `__input`
  (input/select/textarea) + `__helper`. States: default, focus,
  `aria-invalid="true"` (error), `[disabled]`/`[readonly]`. See
  `preview/field.html`.
- **`.lh__btn-row`** and **`.lh__field-row`** — responsive layout
  helpers that enforce the 8px touch-spacing floor and the 220px
  field-min-width breakpoint.

### Class family by context

| Context | Reach for |
|---|---|
| Studio site, blog post, landing | `.lh`, `.lh__post`, `.lh__hero`, `.lh__list` |
| Web app interactive surface | `.lh__btn`, `.lh__field` (+ editorial chrome optional) |
| Pure data app | `.lh__btn`, `.lh__field` only |

Full guidance — state conventions, keyboard expectations, markup
patterns, voice in app copy — lives in
[`docs/web-apps.md`](docs/web-apps.md). Accessibility floor
(contrast table, touch targets, ARIA, motion) lives in
[`docs/accessibility.md`](docs/accessibility.md). Responsive
behavior (breakpoints, `clamp()` vs `@media`, container widths,
desktop-first convention) lives in
[`docs/responsive.md`](docs/responsive.md). Read all three before
shipping interactive surfaces.

### What v0.3.0 deferred

Modals / sheets / toasts / tables / tabs / breadcrumbs / app shell
are all **deferred to v0.4+** and prioritised by real consumer
demand, not speculation. If you hit one of these gaps in a real
consumer, file an issue or open a PR.

---

## Fonts

The site ships **JetBrains Mono** only — no sans, no serif. Weights in
use: **400 / 500 / 600 / 700**, regular + italic (400i, 500i). Loaded
from Google Fonts at runtime in `index.html`:

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:ital,wght@0,400;0,500;0,600;0,700;1,400;1,500&display=swap"
      rel="stylesheet">
```

> ⚠️ **Flagged substitution / missing asset.** The source repo does
> not vendor `.woff2` files; the live site loads JetBrains Mono from
> the Google Fonts CDN. There is no designer-approved fallback
> declared either — the system font stack is the CSS default
> (`ui-monospace, SFMono-Regular, Menlo, Consolas, monospace`).
> **Ask the user** whether to vendor JetBrains Mono locally (adds
> ~120 KB woff2) or keep the CDN dependency.

The one decorative type moment is the ASCII-art wordmark on the
homepage (`███████╗` block letters built from box-drawing
characters), rendered as `<pre>` at ~8–10 px inside a `.lh__hero`.

---

## Content fundamentals

### Voice & tone

- **Plain, declarative, confident, technical.** No hype, no jargon,
  no marketing scaffolding. Sentences land hard and stop.
- **First-person plural or impersonal** — "we," or the studio name
  itself, never "I." User is addressed directly as "you" (rarely).
- **Short sentences stacked into short paragraphs.** Paragraph
  rhythm mirrors the terminal window's `max-width: 76ch`; few
  paragraphs run longer than three sentences.
- **Humour is dry, rare, and used for emphasis, not warmth.**
  ("The catalog keeps itself honest, so if you followed a link that
  landed here, please let us know.")

### Example voice samples (verbatim from the site)

- Greeting: *"An independent software studio. Privacy-first,
  local-first software."*
- Home body: *"Projects graduate into this catalog once they're
  maintained, release-ready, and externally consumable — as an app,
  a library, or a tool anyone can install."*
- Values (three-rules block, full-stopped statements that function
  as section titles):
  - *"Your data should stay yours."*
  - *"The best features don't need a server."*
  - *"Complexity should live in the architecture — not in the
    user's face."*
- 404 page: *"`/path` isn't here. Either the URL has a typo or
  something was linked that no longer exists."*

### Casing & punctuation

- **Sentence case** for page titles, section headings, list items.
  No title-case marketing headers.
- **Em-dashes ( — ) everywhere** — clause separators, list item
  annotations (`**label** — description`), thought continuations.
  They are the studio's favourite mark.
- **Italics for soft emphasis** (render as mint `em`); **bold for
  hard emphasis** (renders amber `strong`). Both sparingly.
- **Period-terminated bullets and status labels** — "Shipping.",
  "Active build.", "Frozen at v1.2.0." The period is part of the
  label.
- Product names are always `backtick-styled` in prose (`forge`,
  `cloud_sync`, `drive_sync_flutter`, `shellcraft`) and render in
  amber code style. Product *pages*' heroes drop the backticks and
  use the `.lh__name` treatment — `forge.` (with a trailing period).

### Terminal metaphors in copy

- URLs are presented literally and framed as paths: `/`, `/writing`,
  `/company`, `/contact`. Back-links use `$ cd ..` styling.
- Back button prefix: `$ cd …`. 404 offers a `$ ls`-style list.
- Breadcrumb/meta prefixes: `// comment-style` (`// 2026-04-19`).
- Footer tags: `[LICENSE] MIT`, `[BUILD] 2026-04-18`, mimicking
  shell option flags.

### Emoji policy

- **Emoji are used sparingly and only in UI chrome, never in prose.**
  The one place in the codebase: the post-envelope crumb uses a
  single `📄` glyph as a file-icon (`.lh__post__crumb::before`).
  No face emoji, no sparkles, no celebration emoji, ever.

### Content examples to emulate

- Project cards are a **tagline + status label + install command +
  prose + structured release notes + links** — never a screenshot,
  never a video hero.
- Posts are framed as a **file in a viewer**: `.lh__post__head` shows
  a filename crumb and ISO date, `.lh__post__body` holds the prose.

---

## Visual foundations

The whole design is a "terminal window on a subtly-gridded void."
Every component either *is* the terminal window (`.lh`) or lives
inside it.

### Palette

- **Background stack** — `#080b10` (page void) → `#0d1117`
  (terminal fill) → `#1a1f28` (cards / chrome / code wells). Each
  step up is slightly bluer and lighter; the whole palette is
  blue-cast.
- **Text stack** — `#e5e7eb` (body), `#a0a6ae` (secondary),
  `#707683` (tertiary / muted). Pure grey, no warm cast.
- **Accents** — `#00f0a0` mint (primary, all links, the prompt
  cursor) and `#ffb347` amber (secondary emphasis, the `hr` rule
  line, code text, bold). `#ff6b6b` red is reserved for errors.
- **Traffic lights** — `#ff5f56 / #ffbd2e / #27c93f` — the classic
  macOS circles live in every `.lh__chrome`. The *only* saturated
  hue combination on the page outside the mint/amber system.

### Background

- Page void is `#080b10` with **two faint cross-hatched gradients**
  at `rgba(0, 240, 160, 0.015)` (mint at ~1.5% alpha), tiled every
  40 × 40 px. You feel the grid before you see it.
- No imagery, no illustrations, no photography anywhere on the site.
- No gradients on fills — gradients are only used for the backdrop
  grid.

### Typography

- JetBrains Mono, 14px body, 1.7 line-height. Full stop.
- The hero (`.lh__name`) is a fluid `clamp(28px, 5.5vw, 56px)` 700-weight
  monospace set tight at `-0.02em`. `#` prefix in dim amber makes it
  read as a markdown heading.
- ASCII wordmark on the homepage: `font-size: clamp(6px, 1.1vw, 10px)`,
  color `--mint`, `white-space: pre`, hidden on narrow viewports.

### Spacing, rhythm & layout

- Page width pins at `max-width: 1060px`, centred.
- Fluid paddings everywhere via `clamp(min, vw-rel, max)`.
- Internal prose paragraphs wrap at `max-width: 76ch`.
- Section gaps scale with viewport: `clamp(28px, 5vw, 48px)`.
- **No cards-overlapping, no bento grids, no multi-column flows.**
  A single centred column with the occasional `display: grid` list.

### Cards & containers

Three elevation tiers, no more:

1. **Window** — the `.lh` terminal container: `border: 1px solid
   var(--border)`, `border-radius: 10px`, `box-shadow: 0 20px 60px
   -20px rgba(0,0,0,0.5), 0 0 0 1px rgba(0, 240, 160, 0.03)` — a
   deep drop plus a 1-px mint ring.
2. **Card** — `.lh__nav` / `.lh__footer` / `.lh__post`: subtle-bg,
   1-px border, `border-radius: 6px`. No shadow.
3. **Row** — `.lh__list li` / `.lh__meta` / `pre`: subtle-bg, no
   border, **2–3-px mint left border** (the signature accent
   treatment). `border-radius: 0 4px 4px 0` — flat on the accent
   side.

### Borders, rules & separators

- **Primary rule** — `<hr class="lh__rule">` is a 2-px solid amber
  band, full width. One per page, used to terminate the hero.
- **Hair rule** — `.lh__rule--hair`: 1-px border-color, wider
  margin, used to break footer zones from content.
- **ASCII section separator** — `.lh__sep` renders `── <strong>WORD</strong>
  ──────────────────────────────────────` in 13-px mono. The dashes are
  literal U+2500 box-drawing characters; the inner word is amber
  700-weight tracked at 0.16em. This is the studio's replacement
  for big section headers.

### Borders on accent elements

Cards use `border-left: 2px solid var(--mint)` as their signature
rhythm. On hover that accent shifts to amber and the row translates
2px to the right: `transform: translateX(2px)` — a tiny but telling
motion.

### Motion

- **Transitions**: all 140–240 ms, `ease`. Named in tokens as
  `--t-fast` (140), `--t-mid` (160), `--t-slow` (240). No spring,
  no bounce, no overshoot.
- **Only three things move on hover**:
  1. Links shift from mint to mint-soft; dotted border goes solid.
  2. Nav links shift from fg-mute to amber.
  3. List cards translate 2px right + accent-border shifts to amber.
- **One animated element site-wide**: `.lh__cursor` — a 9×16 px
  mint block, `animation: lh-blink 1.1s steps(2) infinite`. Stops
  under `prefers-reduced-motion: reduce`.
- **No fades on route change, no page transitions, no scroll
  animations.** The SPA snaps.

### Hover / press states

- Link hover: `color: var(--mint-soft); border-bottom-color:
  var(--mint);` (dotted → implied-solid via colour).
- Nav hover: `color: var(--amber);` — the one place amber is used
  for interaction rather than emphasis.
- Active nav route: `.is-current { color: var(--mint); font-weight:
  500; }`.
- Focus ring: `outline: 2px solid var(--mint); outline-offset: 3px;`
  — a11y-correct, visible against every surface.
- **No press/active states are defined** — the interaction
  vocabulary is intentionally minimal.

### Radii

- `10px` windows, `6px` cards, `4px` rows/controls, `3px` inline
  code, `0` for the left edge of left-accent rows (`border-radius:
  0 4px 4px 0`). No full-pill, no circles except traffic lights.

### Transparency / blur

- The only rgba-alpha uses in the system: (a) the `--grid-line`
  background grid at 1.5% alpha, (b) the 3% mint ring inside the
  window shadow, (c) dotted link underlines at 40% mint. No
  `backdrop-filter`, no acrylic, no frosted glass.

### Imagery

- **No photography.** No illustration. No gradient hero. The only
  imagery in the repo is the `favicon.svg` — a flat representation
  of the terminal chrome (three traffic-light dots + the letters
  `al` in mint + a mini wordmark in amber).

### Tables, code, diagrams

- Tables use GFM defaults; no heavy styling.
- Code blocks: subtle-bg fill, `border-left: 3px solid var(--mint)`,
  no shadow, Rouge/highlight.js tokens mapped to palette (keywords
  → amber, strings → mint-soft, numbers → blue, comments → muted
  italic).
- Diagrams (Mermaid / PlantUML) get `.lh__diagram` — same subtle
  fill with amber left border, centred inline SVG.

---

## Iconography

- **Custom inline SVGs.** Four icons ship with the system —
  `github.svg`, `instagram.svg`, `x.svg`, `mail.svg` in
  `assets/icons/`. 14×14 px source, rendered with
  `fill="currentColor"` so they follow link-hover transitions.
  These were extracted from the originating `arcanelabs.info`
  site footer and are now the canonical copy for all consumers.
- **No icon font.** No Lucide, Heroicons, Phosphor, etc. in the
  repo. If you need to extend the icon set, match the existing
  style: 14×14 viewBox, single-weight glyph, fills-only (no strokes
  for brand icons; 2px strokes are OK for generic UI icons like
  `IconMail`).
- **No emoji in prose.** One system emoji is used as a file-icon
  pseudo-element (`.lh__post__crumb::before { content: "📄 "; }`)
  on post-envelope headers.
- **Unicode as iconography** — the system leans hard on box-drawing
  and shape characters: `▸` for bullet markers, `[1]` for ordered
  markers, `──` and `╚═╝` for ASCII art and separators, `#`, `##`,
  `###`, `>`, `$`, `//` as prefix glyphs for headings, greetings,
  prompts, and comments. Learn the vocabulary; most "icons" are
  ASCII prefixes.

> 💡 **Recommendation.** If you need more icons than the four in
> footer, use **Lucide** (`https://unpkg.com/lucide@…`) as a stylistic
> match — same 24×24 viewBox, same 2-px stroke weight, same
> functional-but-quiet feel. Flag the substitution to the user so
> they can decide whether to vendor-in the SVGs.

---

## What's shipped in this design-system project

See the **Design System tab** for a card-by-card browse. Short list:

- **Colors** — surface stack, text stack, accent swatches, traffic
  lights, usage guidance.
- **Type** — family specimen, scale ramp, prose samples, ASCII
  wordmark, prefix glyphs.
- **Spacing & radii** — token scales + usage.
- **Shadow / elevation** — the three-tier system.
- **Components** — nav, terminal chrome, cards, list rows, code
  blocks, rules, separators, meta grid, buttons-as-commands,
  post envelope.
- **Brand** — logomarks (wordmark, ASCII, favicon), icons.

For full reference renderings of the system in a real app, see
[arcanelabs.info](https://arcanelabs.info) — the Home / Writing /
Project / Company / Contact / 404 screens there are the canonical
worked examples. A future consumer building, e.g., Recallable's
admin shell will produce its own worked example in *its own* repo;
those examples don't live here.

---

## Caveats & open questions

1. **Fonts not vendored.** JetBrains Mono is loaded from Google
   Fonts CDN. See **Fonts** above.
2. **No design-system definition existed in the repo.** This
   README was authored by reverse-engineering `editorial.css` (1041
   lines) and reading the live markdown content. Classifications
   (three card tiers, five prefix glyphs, motion catalogue) are my
   synthesis, not taken from a pre-existing spec.
3. **No logo assets beyond `favicon.svg`.** The site has no
   standalone horizontal wordmark file — the `.lh__nav-brand`
   `arcanelabs` text is set in CSS. If the studio has a richer
   wordmark SVG, please drop it in `assets/`.
4. **No photography / imagery direction.** Because the site uses
   none, there's nothing to document. If Arcane Labs ever uses
   marketing imagery (e.g. for app store screenshots), that style
   would need a separate document.

