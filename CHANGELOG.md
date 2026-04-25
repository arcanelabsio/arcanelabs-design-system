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

[Unreleased]: https://github.com/arcanelabsio/arcanelabs-design-system/compare/v0.2.0...HEAD
[0.2.0]: https://github.com/arcanelabsio/arcanelabs-design-system/releases/tag/v0.2.0
[0.1.0]: https://github.com/arcanelabsio/arcanelabs-design-system/releases/tag/v0.1.0
