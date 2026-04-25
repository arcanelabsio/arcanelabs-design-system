# STATE

> Current micro-task tracker for this repo. Updated whenever a local
> commit is made, a test passes, or the active task changes (per the
> GSD framework).

## Phase

**Phase 1 — Repo published.** Seeded from the Claude Design handoff
bundle and pushed to GitHub at
[`arcanelabsio/arcanelabs-design-system`](https://github.com/arcanelabsio/arcanelabs-design-system)
on 2026-04-26 with tag `v0.1.0`. Visual artifacts are in place;
public; ready for vendoring; no downstream consumer wired yet.

## Current micro-task

_None._ Push is done. Next operator: choose one of the **Next
moves** below.

## Next moves (in order of cost-vs-leverage)

1. **Verify in a browser.** Open `ui_kits/website/index.html` and
   click through the six screens to confirm the stylesheet pair
   renders cleanly without CDN/path issues.
2. **Wire the first downstream consumer.** Pick `arcanelabs.info`
   or `recallable`. Replace the consumer's local `editorial.css`
   with a vendored copy from this repo (Option A in README), record
   the source SHA in the consumer's `CHANGELOG.md`.
3. **Decide on font vendoring.** JetBrains Mono is currently
   Google-Fonts-CDN. Vendor `.woff2`s if any consumer ships
   offline-first.
4. **Future: package publishing.** Add `package.json` exporting
   `colors_and_type.css` + `editorial.css`, publish as
   `@arcanelabs/design-system`. Out of scope today; revisit when 3+
   consumers are live.

## Open questions for the user

- Should `chats/chat1.md` (the handoff conversation transcript)
  remain in the repo long-term, or be moved to a separate `docs/
  history/` once the team grows?
- Is the dual MIT + CC BY 4.0 split correct for the design tokens
  themselves, or should the CSS be CC BY 4.0 too (since it encodes
  the brand)? Currently CSS is MIT.

## Last update

2026-04-26 — repo seeded from handoff bundle, top-level README
rewritten for vendoring consumers, LICENSE paths updated, CHANGELOG
seeded with v0.1.0, `git init` + initial commit + `v0.1.0` tag,
`gh repo create --public --push` to
[`arcanelabsio/arcanelabs-design-system`](https://github.com/arcanelabsio/arcanelabs-design-system).
