# Changelog

All notable changes to the Arcane Labs design system are recorded
here. Consumers who vendor this repo (see `README.md` → **Using this
repo**) should pin to a row in this file and bump when a new row
ships.

Format: [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).
Versions follow [SemVer](https://semver.org/spec/v2.0.0.html), where:

- **MAJOR** — breaking change to a token name, a class name, or a
  semantic mapping (`--mint` → `--accent-1`, `.lh__nav` → `.al-nav`).
- **MINOR** — new token, new component class, new asset; existing
  consumers still work.
- **PATCH** — visual fix, value tweak that preserves the contract
  (e.g. nudging `--border` from `#2a3342` to `#2b3344`).

---

## [Unreleased]

_Nothing yet._

---

## [0.3.1] — 2026-04-26

Documentation polish that concludes the v0.3.x design system slice.
Pure docs additions; no CSS, no token, no asset change. Consumers
on v0.3.0 see no behavioral difference; the new docs make it
faster to onboard new builders (human or AI agent).

### Added

- **`docs/getting-started.md`** — front-door doc. Quick start
  (vendor + link), pick-your-path table, complete worked example
  (a settings page mixing editorial chrome with app primitives),
  reading order, top 10 common pitfalls, the SemVer contract
  restated for consumers, deferred-vs-non-goal map, "now go build"
  closing. README and SKILL elevated to point at this as the
  first read.
- **`docs/responsive.md`** — codifies the four-breakpoint system
  (1060 / 768 / 640 / 380), the `clamp()` vs `@media` philosophy,
  container width conventions, the desktop-first stance, and a
  full inventory of behavior at each breakpoint. Captures what
  `editorial.css` already implements so future contributors don't
  have to grep `@media` to learn the conventions. SKILL gains a
  step-4 instruction to read this when authoring layout.

### Changed

- **`README.md`** — top of file now points at `docs/getting-started.md`
  as the first read. Index entry added for both new docs.
  Web-app consumption section updated to reference all three
  guideline docs.
- **`SKILL.md`** — first bullet now points at `getting-started.md`
  before `README.md`. Step 1 of "How to use it" updated. New
  step 4 covers responsive guidance.

---

## [0.3.0] — 2026-04-26

Web-app enrichment: ship the first app-surface primitives so any
arcanelabsio web app (recallable today, future ones tomorrow) can
build interactive surfaces in the brand language without re-forking
buttons and form fields. Strictly additive.

This is a MINOR bump. No existing token, class, or asset value
changed. v0.2.0 consumers see no behavioral difference unless they
opt into the new classes.

**Mobile / Flutter is now formally out of scope.** Arcanelabsio's
mobile apps (longeviti, vael) maintain their own design systems by
design — Material 3, different palettes, different type families.
The README, SKILL, and `docs/web-apps.md` say this explicitly to
prevent future contributors from quietly cross-pollinating.

### Added

- **`.lh__btn`** — buttons with 4 variants (`--primary`,
  `--secondary` / default, `--ghost`, `--destructive`), 5 states
  (default, hover, focus-visible, `[disabled]`,
  `[data-loading="true"]` with CSS-only spinner that respects
  `prefers-reduced-motion`), and 3 sizes (`--sm` 32px, default
  44px, `--lg` 52px). Built on the editorial `.lh__list` row
  treatment — mint left-border, amber on hover, 2px rightward
  translate.
- **`.lh__btn-row`** — inline button group with 8px gap (touch
  spacing floor).
- **`.lh__field`** — form fields. Wrapper + `__label` (with
  `[data-required]` rendering an amber `*`) + `__input` (input,
  select, textarea — select gets a mint chevron via inline SVG
  background) + `__helper` (with `[data-error="true"]` switching
  to red and a `!` prefix). States: default, focus (amber border +
  mint ring), `aria-invalid="true"` (red left-border),
  `[disabled]`, `[readonly]`.
- **`.lh__field-row`** — responsive field grid; auto-fit columns
  at 220px min, stacks under 640px.
- **`preview/btn.html`** — 4 variants × 5 states + 3 sizes + an
  inline-row example. Self-contained; loads `card.css` and
  `../editorial.css`.
- **`preview/field.html`** — 6 specimens (text, email, password
  focused, email error, select, textarea disabled) + a field-row
  example. Self-contained.
- **`docs/web-apps.md`** — class family by context, state
  conventions (loading / disabled / error / focus / touch),
  keyboard expectations, markup patterns, voice in app copy,
  explicit v0.3.0 deferred list.
- **`docs/accessibility.md`** — computed WCAG 2.1 contrast table
  for every accent on every surface (mint AAA on all surfaces;
  red drops to AA on `--bg-2` / `--subtle`), the formula in
  Python so future contributors can reproduce, plus rules for
  touch targets, focus, ARIA, color-not-only, motion, keyboard,
  zoom, and a pre-ship checklist.
- **18 new CSS tokens** in `colors_and_type.css`:
  - 8 spacing scale: `--space-1` (4px) through `--space-8` (64px),
    on the 4-step / 8dp rhythm
  - 3 interactive state opacities: `--state-disabled-opacity` (0.4),
    `--state-pressed-opacity` (0.7), `--state-loading-opacity` (0.6)
  - Touch + focus floor: `--touch-target-min` (44px),
    `--focus-ring-width` (2px), `--focus-ring-offset` (3px)
  - Z-index scale: `--z-base` (0), `--z-overlay` (100),
    `--z-modal` (1000), `--z-toast` (1100)

### Deferred to v0.4+

Modal / sheet / toast / drawer; tables and data display;
empty / loading-skeleton / error-state components; tabs /
breadcrumbs / badges / chips; app shell (sidebar, topbar);
light mode (explicit non-goal — brand is dark-only).

### Migration

None. Existing consumers continue to work unchanged. Opt into the
new classes when building interactive web-app surfaces.

---

## [0.2.0] — 2026-04-26

Restructure: focus the repo on cross-app design elements only. The
v0.1.0 seed bundled pure design vocabulary together with
arcanelabs.info-specific implementation mockups (React routes, sample
markdown content, the website UI kit, the website 404 template).
Consumers wire the design vocabulary into their own frameworks; they
don't need someone else's app code in the package.

This is a MINOR bump, not MAJOR — the exported surface (tokens,
class names, brand assets) is byte-identical to v0.1.0. Only the
surrounding reference material was removed, and no consumer was wired
to those paths.

### Removed

- `src/` — React route + component snippets that targeted
  arcanelabs.info specifically (imported `RouteEffects`, `Diagram`,
  `content/loader` — none of which shipped here).
- `content/` — markdown samples for arcanelabs.info's pages, posts,
  and projects.
- `public/` — website-specific 404 template and duplicated favicon.
- `ui_kits/website/` — hash-routed static rebuild of arcanelabs.info.
  For a worked example of the system in a real app, see the live
  arcanelabs.info site instead.
- `chats/` — handoff conversation transcript. Archived locally;
  no longer relevant once the design system has its own README and
  versioning discipline.

### Changed

- `README.md` — Index lists design elements only; "Reference
  implementations" section reframed to point at consuming apps
  rather than describing in-repo references. Iconography section
  updated since the icons are now standalone SVGs in `assets/icons/`,
  not extracts from a React component.
- `SKILL.md` — UI kit reference removed; description rewritten as
  cross-app design system.
- `LICENSE` — paths under MIT and CC BY 4.0 sections trimmed to match
  the new tree; trademark note updated.

### Migration

No migration required for token / class consumers. If you were
preparing to vendor `src/` or `content/`, look at arcanelabs.info
instead — those files belong to that consumer's implementation,
not to the design system.

---

## [0.1.0] — 2026-04-26

Initial vendoring from the [Claude Design](https://claude.ai/design)
handoff bundle. Authored by reverse-engineering
[`arcanelabsio/arcanelabs.info`](https://github.com/arcanelabsio/arcanelabs.info)'s
`src/styles/editorial.css` (1041 lines) and the live markdown
content. This is the first artifact of the system as a *standalone*
repo, separate from the company-site implementation.

### Added

- **Tokens** — `colors_and_type.css` exposes the full palette (surface
  stack, text stack, mint/amber/red/blue accents, traffic-light
  trio), the JetBrains Mono type ramp (hero / post-title / section /
  body / small / xsmall / micro), radii, the single window shadow,
  spacing rhythm tokens, and the three motion durations. Plus
  semantic defaults that bind the tokens onto `body`, `h1`–`h3`,
  `p`, `a`, `em`, `strong`, `code`, `pre`, `blockquote`, `hr`, and
  `:focus-visible`.
- **Components** — `editorial.css` ports the full `.lh__*`
  vocabulary: terminal `.lh` window with chrome, `.lh__nav` (incl.
  mobile hamburger collapse), `.lh__hero` / `.lh__name` /
  `.lh__greeting`, the amber `.lh__rule`, prose styles, `.lh__list`
  (with `.lh__list--linkable` row-card variant), `.lh__sep` ASCII
  separator, `.lh__rules` principles block, `.lh__footer`,
  `.lh__post` envelope, `.lh__meta` project grid,
  `.lh__backlink`, `.lh__diagram`, `.lh__cursor` blink animation,
  `.lh__sr-only`, plus highlight.js / Rouge token mappings.
- **Assets** — `assets/favicon.svg` and four inline-extracted icons
  (`github.svg`, `instagram.svg`, `x.svg`, `mail.svg`) at 14px /
  24px viewBox.
- **Preview cards** — 27 self-contained HTML specimens under
  `preview/`, one per token / component, sharing `preview/card.css`.
- **UI kit** — `ui_kits/website/index.html`, a hash-routed static
  rebuild of the company site (Home / Writing / Post / Project /
  Company / Contact / 404). Self-contained; no build step.
- **React reference** — `src/` houses the route + component
  extracts (`TerminalShell`, `Nav`, `Footer`, `Markdown`, all six
  routes). Reference material only — see README.
- **Sample content** — `content/pages/`, `content/posts/`,
  `content/projects/` show the prose shapes that pair with the
  components (project meta blocks, post-envelope head/body, page
  greeting + rule + body).
- **404 template** — `public/404.html`.
- **Brand bible** — `README.md` (visual foundations, voice & tone,
  iconography, caveats), `SKILL.md` (Claude Code skill entry
  point), this `CHANGELOG.md`, `LICENSE` (dual MIT + CC BY 4.0),
  `STATE.md` (current micro-task tracker).

### Caveats inherited from the source

- **Fonts not vendored.** JetBrains Mono is loaded from the Google
  Fonts CDN. Vendoring decision deferred — see README.
- **No standalone wordmark.** The horizontal `arcanelabs` brand is
  set in CSS in `.lh__nav-brand`; the only file asset is the
  favicon. The ASCII wordmark on the homepage is a `<pre>` literal
  in `src/routes/Home.tsx`.
- **No photography or marketing imagery.** The system has no fill
  imagery and no gradient hero. If that scope expands, it gets a
  separate document.

[Unreleased]: https://github.com/arcanelabsio/arcanelabs-design-system/compare/v0.3.1...HEAD
[0.3.1]: https://github.com/arcanelabsio/arcanelabs-design-system/releases/tag/v0.3.1
[0.3.0]: https://github.com/arcanelabsio/arcanelabs-design-system/releases/tag/v0.3.0
[0.2.0]: https://github.com/arcanelabsio/arcanelabs-design-system/releases/tag/v0.2.0
[0.1.0]: https://github.com/arcanelabsio/arcanelabs-design-system/releases/tag/v0.1.0
