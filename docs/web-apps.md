# Web-app consumption

This design system was originally extracted from
[arcanelabs.info](https://arcanelabs.info) — an editorial site. Its
first vocabulary covers *site* surfaces: nav, hero, list rows, post
envelope, footer. v0.3.0 starts adding *app* surfaces — interactive
primitives every web app needs (buttons, form fields, with more
coming in v0.4+).

This doc tells you which class family to reach for.

---

## 1. Class family by context

| Context | Reach for | Example |
|---|---|---|
| Studio site, blog post, landing | `.lh`, `.lh__post`, `.lh__hero`, `.lh__list`, `.lh__sep`, `.lh__rule` | `arcanelabs.info` |
| Web app interactive surface | `.lh__btn`, `.lh__field` (+ editorial chrome optional) | `recallable` settings page |
| App inside terminal chrome | both — wrap the app surface in `.lh` + `.lh__chrome` if the brand framing helps | a settings dashboard themed as a terminal session |
| Pure data app (dashboard, table-heavy) | `.lh__btn`, `.lh__field` only — skip the editorial chrome | future arcanelabsio analytics |

Editorial classes (`.lh__post`, `.lh__hero`) carry an *editorial
metaphor* — "you are reading a file in a terminal." App classes
(`.lh__btn`, `.lh__field`) are *interactive primitives* with no
metaphor. Mix freely; the visual language carries either way because
they share tokens.

---

## 2. State conventions

Apply consistently across consumers — components that don't share
these conventions will feel off-brand even with the right colors.

### Loading

- **Async button**: set `data-loading="true"` on `.lh__btn` while a
  request is in flight. The label hides; an inline spinner takes
  over. `pointer-events: none` is automatic.
- **Async page section**: skeleton placeholders ship in v0.4. Until
  then, use a centered `.lh__btn--ghost[data-loading="true"]` as a
  loading affordance, or the `.lh__cursor` blinking block from the
  editorial set.
- **Never**: a click with no response. Latency budget is 100ms before
  the user thinks the click was lost.

### Disabled

- Use the **semantic** `[disabled]` attribute on `<button>`,
  `<input>`, `<select>`, `<textarea>`. The CSS keys off it.
- For non-form elements that can't take `[disabled]`, use
  `aria-disabled="true"`.
- Disabled controls render at `--state-disabled-opacity` (0.4),
  `cursor: not-allowed`, no hover effects, no `pointer-events`.
- Never style something to look disabled but leave it interactive.

### Error

- Form fields: `aria-invalid="true"` on the input. The CSS adds the
  red left-border. The helper carries `data-error="true"` to switch
  to red and prefix `!`.
- Buttons: rare — destructive variant is the only colored button.
  Errors usually surface as a toast or a form helper, not as a red
  button.
- Helper text must describe **the recovery path**, not just the
  failure. "Use a valid address" beats "Invalid input."

### Focus

- Never disable `:focus-visible`. Tokens: `--focus-ring-width`,
  `--focus-ring-offset`. Components inherit them.
- The focus ring is mint on every surface — high contrast (see
  `docs/accessibility.md` §1).

### Touch

- Minimum `--touch-target-min` (44px) on any tappable element. If
  the visual is smaller, expand the hit area.
- Adjacent touch targets ≥ 8px apart. `.lh__btn-row` enforces this.

---

## 3. Keyboard expectations

- **Tab order matches visual order.** If CSS reorders, audit the DOM.
- **Enter / Space** activates buttons (browser default — don't break
  it).
- **Enter** submits forms (browser default — intercept only when
  necessary).
- **Escape** dismisses overlays (overlay components ship in v0.4+).
- **Arrow keys** for tab strips, radio groups, menus — wire when
  those components ship.

After form-submit error, **auto-focus the first invalid field**. This
is a hard rule, not a polish item.

---

## 4. Markup patterns

### Button

```html
<button class="lh__btn lh__btn--primary">Save</button>
<button class="lh__btn">Cancel</button>
<button class="lh__btn lh__btn--destructive" data-loading="true">
  Delete
</button>

<!-- Inline button row with safe touch spacing -->
<div class="lh__btn-row">
  <button class="lh__btn lh__btn--primary">Save changes</button>
  <button class="lh__btn">Discard</button>
  <button class="lh__btn lh__btn--ghost">Help</button>
</div>
```

### Form field

```html
<div class="lh__field">
  <label class="lh__field__label" for="email" data-required>
    Email
  </label>
  <input class="lh__field__input"
         id="email"
         type="email"
         required
         aria-required="true">
  <p class="lh__field__helper">We'll send a confirmation link.</p>
</div>
```

### Field row (side-by-side on wide, stacked on narrow)

```html
<div class="lh__field-row">
  <div class="lh__field">…</div>
  <div class="lh__field">…</div>
</div>
```

### Error pattern

```html
<div class="lh__field">
  <label class="lh__field__label" for="email" data-required>Email</label>
  <input class="lh__field__input"
         id="email"
         type="email"
         value="not.an.email"
         aria-invalid="true"
         aria-describedby="email-err">
  <p class="lh__field__helper" id="email-err" data-error="true">
    Use a valid address — domain looks malformed.
  </p>
</div>
```

---

## 5. Voice in app copy

The editorial voice extends to app surfaces:

- **Period-terminated labels and statuses.** "Saved.", "Active.",
  "Frozen at v1.2.0." The period is part of the label.
- **Em-dashes for clauses.** Helper text often: "Min 12 characters
  — tabular numerals OK."
- **Backticks for product names** in app copy: ``You're connected to
  `forge`.``
- **No emoji.** Use the SVG icons in `assets/icons/`. No 🎉 on save,
  no ⚠️ on error. The visual language carries the emotional load.
- **Confirm destructive actions** with text, not just iconography:
  "Delete project — this can't be undone."

---

## 6. What's not in v0.3.0

Named explicitly so you know what to expect from this slice:

- **Modals / sheets / toasts / drawers** → v0.4 candidate
- **Tables / data display** → v0.4 candidate
- **Empty / loading-skeleton / error state components** → v0.4
  candidate
- **Tabs / breadcrumbs / badges / chips** → v0.5+
- **App shell (sidebar, topbar variants)** → v0.5+
- **Light mode** — explicit non-goal. The brand is dark-only.
- **Mobile / Flutter / Swift / RN** — out of scope. Mobile apps in
  the org (longeviti, vael) maintain their own design systems.

When you hit a real consumer need that this list defers, file an
issue or open a PR. v0.4 prioritises by demand, not speculation.
