# STATE

> Current micro-task tracker for this repo. Updated whenever a local
> commit is made, a test passes, or the active task changes (per the
> GSD framework).

## Phase

**Phase 3 — Web-app reference.** v0.3.0 adds the first app-surface
primitives — `.lh__btn` (4 variants × 5 states × 3 sizes) and
`.lh__field` (input/select/textarea with label + helper, 4 states) —
plus 18 new tokens (spacing scale, state opacities, touch + focus
floor, z-index), `docs/web-apps.md`, `docs/accessibility.md` (with
computed WCAG contrast table). **Web-only**: mobile / Flutter is
formally out of scope; longeviti and vael own their own design
systems. Public at
[`arcanelabsio/arcanelabs-design-system`](https://github.com/arcanelabsio/arcanelabs-design-system),
tagged `v0.3.0`. No web consumer has wired the new classes yet.

## Current micro-task

_None._ Push is done. Next operator: choose one of the **Next
moves** below.

## Next moves (in order of cost-vs-leverage)

1. **Verify in a browser.** Open `preview/btn.html` and
   `preview/field.html` to confirm the new app-surface classes
   render correctly. Spot-check one existing preview card to confirm
   no regression from the additive token block.
2. **Wire `.lh__btn` + `.lh__field` into recallable.** Recallable is
   the obvious first consumer — it's already a web app and already
   shares the editorial CSS. Replace its locally-forked button and
   form-field components with the new design-system classes; record
   the source SHA in recallable's `CHANGELOG.md`.
3. **Decide v0.4 priorities.** The two strongest candidates are
   overlays (modal + toast) and tables. Pick by recallable's actual
   need, not speculation.
4. **Decide on font vendoring.** JetBrains Mono is currently
   Google-Fonts-CDN. Vendor `.woff2`s if any consumer ships
   offline-first.
5. **Future: package publishing.** Add `package.json` exporting
   `colors_and_type.css` + `editorial.css`, publish as
   `@arcanelabs/design-system`. Out of scope today; revisit when
   3+ web consumers are live.

## Open questions for the user

- Is the dual MIT + CC BY 4.0 split correct for the CSS itself, or
  should the stylesheet be CC BY 4.0 too (since it encodes brand
  identity beyond just generic styling)? Currently MIT.
- Which v0.4 component slice ships first — overlays (modal + toast +
  drawer) or table/data display? Driven by which recallable hits
  first when wiring v0.3.0.

## Last update

2026-04-26 — added `docs/responsive.md` (four-breakpoint system,
`clamp()` vs `@media`, container widths, desktop-first stance, full
behavior inventory). Pure documentation; no CSS change. Recorded
under `[Unreleased]` in CHANGELOG; will roll into the next tagged
release.

2026-04-26 — v0.3.0 web-app enrichment. Added `.lh__btn` (4 variants
× 5 states × 3 sizes), `.lh__field` (input/select/textarea + label +
helper, 4 states), `.lh__btn-row`, `.lh__field-row`. 18 new tokens.
Two new preview specimens (`preview/btn.html`, `preview/field.html`).
Two new docs (`docs/web-apps.md`, `docs/accessibility.md` with
computed WCAG contrast table). README + SKILL + CHANGELOG + STATE
updated. Mobile/Flutter scope formally excluded throughout. Tagged
`v0.3.0` and pushed to GitHub.
