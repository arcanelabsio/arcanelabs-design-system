# STATE

> Current micro-task tracker for this repo. Updated whenever a local
> commit is made, a test passes, or the active task changes (per the
> GSD framework).

## Phase

**Phase 2 — Restructured for cross-app reuse.** v0.2.0 restructures
the repo to ship design elements only: tokens, component stylesheet,
brand assets, and preview specimens. Implementation mockups (React
routes, content samples, the website UI kit, the website 404)
removed — apps wire the classes into their own framework. Public at
[`arcanelabsio/arcanelabs-design-system`](https://github.com/arcanelabsio/arcanelabs-design-system),
tagged `v0.2.0`, no consumer wired yet.

## Current micro-task

_None._ Push is done. Next operator: choose one of the **Next
moves** below.

## Next moves (in order of cost-vs-leverage)

1. **Verify in a browser.** Open any of the `preview/*.html` cards
   (e.g. `preview/colors-surfaces.html`, `preview/nav.html`,
   `preview/post-envelope.html`) to confirm the token + stylesheet
   pair render cleanly without CDN/path issues.
2. **Wire the first downstream consumer.** Pick `recallable`
   (cleaner: nothing to migrate from) or `arcanelabs.info` (the
   originating implementation; needs careful diff against the
   internal copy). Pull the two CSS files from this repo's `main`
   raw URLs (see README → "Using this repo"), record the source
   SHA in the consumer's `CHANGELOG.md`.
3. **Decide on font vendoring.** JetBrains Mono is currently
   Google-Fonts-CDN. Vendor `.woff2`s if any consumer ships
   offline-first.
4. **Future: package publishing.** Add `package.json` exporting
   `colors_and_type.css` + `editorial.css`, publish as
   `@arcanelabs/design-system`. Out of scope today; revisit when
   3+ consumers are live.

## Open questions for the user

- Is the dual MIT + CC BY 4.0 split correct for the CSS itself, or
  should the stylesheet be CC BY 4.0 too (since it encodes brand
  identity beyond just generic styling)? Currently MIT.

## Last update

2026-04-26 — v0.2.0 restructure. Removed `src/`, `content/`,
`public/`, `ui_kits/`, `chats/` (implementation mockups). Updated
README, SKILL, LICENSE, CHANGELOG to reflect cross-app focus.
Tagged `v0.2.0` and pushed to GitHub.
