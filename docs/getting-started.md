# Getting started

You're about to build something on top of the Arcane Labs design
system. This is the front door. Read this once, then you'll know
which deeper doc to go to next.

> **Scope.** This is a **web-only** design system. Mobile / Flutter
> apps in the org (longeviti, vael) maintain their own design
> systems by design. If you're building anything other than an
> arcanelabsio **web** app or **web** site, stop — this isn't your
> tool.

---

## 1. Quick start (60 seconds)

Vendor the stylesheet pair into your consuming app:

```bash
# from your consuming repo's root, e.g. arcanelabs.info or recallable
mkdir -p src/styles
curl -sSL https://raw.githubusercontent.com/arcanelabsio/arcanelabs-design-system/main/colors_and_type.css \
  -o src/styles/arcane-tokens.css
curl -sSL https://raw.githubusercontent.com/arcanelabsio/arcanelabs-design-system/main/editorial.css \
  -o src/styles/arcane-editorial.css
```

Link them in your HTML — **tokens first, components second**:

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:ital,wght@0,400;0,500;0,600;0,700;1,400;1,500&display=swap"
      rel="stylesheet">
<link rel="stylesheet" href="/src/styles/arcane-tokens.css">
<link rel="stylesheet" href="/src/styles/arcane-editorial.css">
```

Pin the source SHA in your consuming app's `CHANGELOG.md` so the
next sync is mechanical:

```markdown
## [your-version] — 2026-04-26
### Added
- Vendored `colors_and_type.css` + `editorial.css` from
  arcanelabsio/arcanelabs-design-system@<short-sha>
```

That's it. You can now use any `.lh__*` class. Continue to §2 to
pick your path.

---

## 2. Pick your path

What are you building?

| Building this… | Reach for… | Read next |
|---|---|---|
| Editorial site, blog post, landing page | `.lh`, `.lh__chrome`, `.lh__nav`, `.lh__hero`, `.lh__post`, `.lh__list`, `.lh__sep`, `.lh__rule`, `.lh__footer` | the relevant `preview/*.html` specimens; arcanelabs.info source as worked example |
| Web app interactive surface (form, dashboard, settings) | `.lh__btn`, `.lh__field`, `.lh__btn-row`, `.lh__field-row` | [`docs/web-apps.md`](web-apps.md) |
| Mixed (app inside terminal chrome) | both | [`docs/web-apps.md`](web-apps.md) |
| 404 / error page | `.lh__name`, `.lh__greeting`, `.lh__rule`, `.lh__list`, `.lh__sep` | the `arcanelabs.info` source `Home.tsx` / `NotFound.tsx` |

The class families share tokens and visual language; mixing them
is a first-class pattern, not a compromise.

---

## 3. A complete worked example

A settings page with a form, an action row, and a helper line. Drop
this into any HTML file with the two `<link>`s above and it will
render correctly.

```html
<body class="editorial">
  <main class="lh">
    <div class="lh__chrome">
      <span class="lh__dot lh__dot--red"></span>
      <span class="lh__dot lh__dot--amber"></span>
      <span class="lh__dot lh__dot--green"></span>
      <span class="lh__chrome-title">
        <strong>recallable</strong> — zsh — settings
      </span>
    </div>
    <div class="lh__page">
      <section class="lh__section">
        <h1 class="lh__name">Settings.</h1>
        <p class="lh__greeting">Account preferences and recovery.</p>
        <hr class="lh__rule">

        <form>
          <div class="lh__field-row">
            <div class="lh__field">
              <label class="lh__field__label" for="display"
                     data-required>Display name</label>
              <input class="lh__field__input" id="display"
                     type="text" required aria-required="true"
                     value="Ada Lovelace">
              <p class="lh__field__helper">
                Shown next to your posts.
              </p>
            </div>
            <div class="lh__field">
              <label class="lh__field__label" for="email"
                     data-required>Email</label>
              <input class="lh__field__input" id="email"
                     type="email" required aria-required="true"
                     value="ada@studio.dev">
              <p class="lh__field__helper">
                Used for recovery — em-dash separated values OK.
              </p>
            </div>
          </div>

          <div class="lh__btn-row" style="margin-top: var(--space-5)">
            <button class="lh__btn lh__btn--primary" type="submit">
              Save changes
            </button>
            <button class="lh__btn" type="button">Discard</button>
            <button class="lh__btn lh__btn--ghost" type="button">
              Cancel
            </button>
          </div>
        </form>
      </section>
    </div>
  </main>
</body>
```

This composes editorial chrome (`.lh__chrome` traffic lights,
`.lh__name`, `.lh__rule`) with app primitives (`.lh__field`,
`.lh__btn`). No new CSS needed.

---

## 4. The reading order

Read these in order, by need:

1. **This doc** — you're here.
2. **[`README.md`](../README.md)** — brand context, voice & tone,
   visual foundations, iconography. Voice is load-bearing; skim at
   minimum.
3. **[`docs/web-apps.md`](web-apps.md)** — when to reach for which
   class family, state conventions, keyboard expectations, markup
   patterns, voice in app copy.
4. **[`docs/accessibility.md`](accessibility.md)** — read **before
   shipping any interactive surface**. Computed WCAG contrast
   table, touch-target floor, focus / motion / ARIA / keyboard.
   Non-negotiable.
5. **[`docs/responsive.md`](responsive.md)** — read **when
   authoring layout**. Four named breakpoints, `clamp()` vs
   `@media`, container widths, desktop-first convention.
6. **`preview/*.html`** — 29 self-contained visual specimens. Open
   in a browser when you need to see a token or component in
   isolation. They double as the **visual contract** — changes
   that alter how these render are breaking.
7. **[`CHANGELOG.md`](../CHANGELOG.md)** — what shipped when. Pin
   to a row.

---

## 5. Common pitfalls

The top mistakes that show up when consumers wire this in:

1. **Using emoji as icons.** Don't. The system ships SVG icons in
   `assets/icons/`. The one allowed emoji is `📄` as a
   pseudo-element on `.lh__post__crumb` — and only there.
2. **Inventing parallel class names.** Don't add `.btn-primary` or
   `.form-input`. Reach for `.lh__btn` / `.lh__field`. If the
   primitive you need doesn't exist yet (modal, toast, table),
   open a v0.4 issue rather than fork.
3. **Adding new accent hues.** Mint and amber do everything; red is
   reserved for errors; blue only appears in code tokens. New
   accents would dilute the brand.
4. **Adding light mode.** Explicit non-goal. The brand is dark-only.
5. **Disabling focus rings.** Never. Use `--focus-ring-width` and
   `--focus-ring-offset` to tune the appearance, not to remove it.
6. **`@media` for type sizes.** Use `clamp()` for fluid scaling;
   `@media` is for layout shifts (collapse, stack, hide), not for
   stepping a font from 14 → 16px.
7. **Mixing `min-width` and `max-width` queries.** The system is
   desktop-first (`max-width:` only). Don't add `min-width:`
   queries — that flips the convention and breaks the cascade
   ordering.
8. **Forgetting the stylesheet order.** `colors_and_type.css` must
   load before `editorial.css`. Components reference tokens; the
   tokens must be defined first.
9. **Breaking the 76ch prose width.** Don't override
   `max-width: 76ch` on `.lh p` — that's the brand reading
   measure. App surfaces (helpers, fields) already opt out.
10. **Skipping `aria-` attributes on form fields.**
    `aria-invalid="true"` drives the red error state;
    `aria-describedby` couples helper to input. The CSS won't paint
    the error state without the attribute, by design.

---

## 6. Stability and versioning

This repo is vendored, not installed. The SemVer commitments,
restated for consumers:

| Level | What changes | What stays the same |
|---|---|---|
| **MAJOR** | Token name rename (`--mint` → `--accent-1`), class name rename (`.lh__nav` → `.al-nav`), removed asset | Nothing in the public surface — every consumer needs to migrate |
| **MINOR** | New tokens, new classes, new variants, new states | All existing tokens and classes; their values; their visual appearance |
| **PATCH** | Visual fix, value tweak preserving the contract, documentation | Token names, class names, semantic mappings |

Pin to a tag (`v0.3.1`, not `main`) for production consumers.
Recompute / re-vendor when this repo's `CHANGELOG.md` ships a row.

The current contract (v0.3.x):

- 7 surface / text / accent token families (see `colors_and_type.css`)
- 8 spacing scale tokens (`--space-1` .. `--space-8`)
- 3 state opacity tokens
- Touch + focus floor tokens
- 4 z-index tokens
- 6 motion durations / radii / shadow tokens
- ~25 editorial classes (`.lh__*` site surface)
- 4 app-surface classes (`.lh__btn`, `.lh__field`, plus their row helpers)
- 4 brand assets (favicon + 4 icons)
- 4 documented breakpoints (1060 / 768 / 640 / 380)

---

## 7. When to stop reading and start asking

If you hit something this system doesn't cover:

- **Component you need doesn't exist** (modal, toast, table, tabs,
  empty state, etc.) — open an issue or PR against this repo.
  v0.4+ will be driven by real consumer demand, not speculation.
- **You think you need a new token** (a sixth accent, a different
  type weight) — open an issue. Probably the answer is "use what's
  there"; sometimes it's "yes, let's add it." Don't add it
  locally in your consumer.
- **You think you need to break the brand** (light mode, mobile,
  Flutter, gradient fill, photography) — re-read §scope at the
  top of this doc. The answer is no. If you have a strong reason,
  bring it to the studio.
- **The spec is ambiguous** — ask. Cheaper than building wrong.

---

## 8. What's not here yet

Named explicitly so you know what to expect:

**Deferred to v0.4** (overlay + data-display slice — driven by
recallable's first wiring):

- Modals, sheets, drawers
- Toasts (with `aria-live`, auto-dismiss, queue handling)
- Tables (sortable, sticky head, zebra optional)
- Empty states, loading skeletons, error states

**Deferred to v0.5+:**

- Tabs, breadcrumbs, badges, chips, pills
- App shell variants (sidebar, topbar)
- Container queries (component-level responsive behavior)

**Explicit non-goals** (will not happen):

- Light mode
- Mobile / Flutter / Swift / RN consumers
- Component implementations in any framework
- Photography, illustration, gradient fills

---

## 9. Now go build

You have:

- The two stylesheets vendored
- A worked example (§3)
- The reading order (§4)
- The pitfalls (§5)
- The contract (§6)

Open `preview/btn.html` and `preview/field.html` in a browser to
confirm the system renders correctly in your environment. Then
ship.
