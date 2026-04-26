# Accessibility floor

Web a11y floor for any consumer of this design system. Cross-platform
(mobile/Flutter) is **out of scope** — those apps maintain their own
design systems and a11y policies.

This is the contract every consumer is expected to honor. Component
appearance is the easy part; the rules below are the part that's
actually load-bearing for users.

---

## 1. Contrast

All foreground / surface pairings in the system, computed via the
WCAG 2.1 relative-luminance formula. Thresholds: **AAA ≥ 7.0**,
**AA ≥ 4.5**, **AA-large ≥ 3.0** (large text only — 18px+ regular or
14px+ bold).

| Foreground | `--bg` (#080b10) | `--bg-2` (#0d1117) | `--subtle` (#1a1f28) |
|---|---|---|---|
| `--fg` (#e5e7eb) | **15.92:1 AAA** | **15.29:1 AAA** | **13.35:1 AAA** |
| `--fg-mute` (#a0a6ae) | 8.03:1 AAA | 7.71:1 AAA | 6.74:1 AA |
| `--muted` (#707683) | 4.32:1 AA-large | 4.15:1 AA-large | 3.63:1 AA-large |
| `--mint` (#00f0a0) | **13.11:1 AAA** | **12.59:1 AAA** | **11.00:1 AAA** |
| `--amber` (#ffb347) | 11.06:1 AAA | 10.63:1 AAA | 9.28:1 AAA |
| `--red` (#ff6b6b) | 7.10:1 AAA | 6.82:1 AA | 5.96:1 AA |
| `--blue` (#7ab8ff) | 9.50:1 AAA | 9.13:1 AAA | 7.97:1 AAA |

### Rules

- **Body text**: use `--fg` on any surface. Never use `--muted` for
  body — it only meets AA-large. The system uses `--muted` only for
  small visual elements (timestamps, dots, separators) where the
  meaning is carried by adjacent text.
- **Secondary text**: `--fg-mute` is fine on `--bg` and `--bg-2` (AAA);
  on `--subtle` it meets AA but not AAA.
- **Accents**: all of `--mint`, `--amber`, `--blue` clear AAA on every
  surface. They can be used freely for body-size text.
- **Error red**: `--red` clears AAA on `--bg` only; on `--bg-2` and
  `--subtle` it's AA. Acceptable for body, but if regulatory guidance
  ever demands AAA across the board, swap to `#ff8585` (which the
  destructive button hover already uses).

### Verifying new accents

When adding a new accent token, recompute against every surface using
the WCAG 2.1 formula:

```
For each channel c in {R, G, B}, normalised to [0, 1]:
  c_lin = c / 12.92                       if c ≤ 0.03928
  c_lin = ((c + 0.055) / 1.055) ^ 2.4     otherwise

L = 0.2126 * R_lin + 0.7152 * G_lin + 0.0722 * B_lin

ratio = (L_light + 0.05) / (L_dark + 0.05)
```

The script in this repo's history (the contrast computation that
generated the table above) is reproducible — see the v0.3.0 commit
message for the Python source.

---

## 2. Touch targets

`--touch-target-min: 44px` is the floor for any tappable element on
the web. Mobile breakpoints inherit the same floor (no relaxation —
the editorial system runs on mobile web too, e.g. arcanelabs.info on
a phone).

- Buttons: `.lh__btn` enforces `min-height: var(--touch-target-min)`
  by default. `.lh__btn--sm` is 32px and is **not** a primary touch
  target — only use it inside dense desktop surfaces or as a
  secondary action paired with a 44px+ primary.
- Form inputs: `.lh__field__input` enforces the same floor. Textareas
  default to 2× the floor; can grow.
- Icon-only controls: if the visual icon is smaller than 44×44, the
  hit area must be expanded (padding, or an invisible larger
  positioned-absolute overlay). Never rely on a 14×14 SVG to be the
  entire tappable region.

Adjacent touch targets must have **≥ 8px** spacing. The `.lh__btn-row`
class enforces `gap: var(--space-2)` (8px); use it.

---

## 3. Focus

Focus is non-negotiable. Never disable `:focus-visible`.

- The default focus ring is set in `colors_and_type.css`:
  `outline: 2px solid var(--mint); outline-offset: 3px;`. Tokens for
  width and offset are exposed as `--focus-ring-width` and
  `--focus-ring-offset` for components that need to tune them.
- App-surface components (`.lh__btn`, `.lh__field__input`) carry
  their own `:focus-visible` rule using these tokens. If you author a
  new interactive component, do the same.
- The focus ring is mint on every surface — high contrast (AAA on all
  three surfaces, see table above), high signal.

---

## 4. ARIA & semantics

- **Form labels**: every `<input>` / `<select>` / `<textarea>` must
  have an associated `<label>` (the `for=""` ↔ `id=""` pattern, or
  wrap-style). The `.lh__field` markup pattern enforces this — see
  `docs/web-apps.md`.
- **Required fields**: pair the `data-required` attribute on the label
  (which renders the amber `*`) with `aria-required="true"` on the
  input itself.
- **Error fields**: `aria-invalid="true"` on the input; the helper
  text reference goes via `aria-describedby="..."` to the helper's
  `id`. The helper carries `data-error="true"` to switch to red.
- **Toasts**: `aria-live="polite"` for non-urgent ("Saved.");
  `role="alert"` (which implies `aria-live="assertive"`) for urgent
  ("Save failed — retry.").
- **Form-level error summary** at the top of the form: use
  `role="alert"` so screen readers announce it. After submit error,
  auto-focus the first invalid field.
- **Icon-only buttons**: every one needs `aria-label`. No exceptions.
  The system already enforces this in voice — labels are everywhere.

---

## 5. Color is not the only signal

The brand uses color heavily, but color must always pair with
non-color signal:

- **Status labels** carry meaning in the *text* — "Shipping.",
  "Active build.", "Frozen at v1.2.0." — not just amber.
- **Errors** combine the red border *and* the helper text describing
  the recovery path. A red border alone is insufficient.
- **Required fields** pair the amber `*` with `aria-required="true"`.
- **Hover** changes color, but in the case of `.lh__list` rows it
  also translates 2px right — a non-color signal (motion).

If you find yourself communicating something via *only* color, fix it
before shipping.

---

## 6. Motion

`prefers-reduced-motion: reduce` is honored globally in
`colors_and_type.css` — every transition and animation collapses to
near-zero (`0.01ms`).

- **App-surface components** must inherit this. The button
  loading-spinner explicitly disables its keyframe under
  reduced-motion. New components must do the same.
- Never run scroll-driven animations or auto-advancing carousels —
  the system has no examples of either, and the brand commitment is
  "the SPA snaps."
- Animation duration: 140–240ms (`--t-fast`, `--t-mid`, `--t-slow`).
  No spring, no bounce, no overshoot.

---

## 7. Keyboard

- **Tab order** must match visual order. If you reorder visually with
  CSS, audit that the DOM order still tells the same story.
- **Enter / Space** activate buttons (the browser does this for
  `<button>` automatically — don't break it with custom event
  handlers).
- **Enter** submits forms (let the browser's default form submission
  work; intercept only when there's a reason).
- **Escape** dismisses overlays (when overlay components ship in
  v0.4+).
- **Arrow keys** for tab strips, radio groups, menus — wire when those
  components ship.

---

## 8. Reduced text scaling

Don't disable browser zoom or user font scaling. The viewport meta
tag must be `width=device-width, initial-scale=1` — never with
`maximum-scale=1` or `user-scalable=no`.

The system already specifies a 16px base on mobile (avoids iOS
auto-zoom on input focus). Headings use `clamp(...)` so they scale
fluidly between viewport sizes.

---

## 9. Pre-ship checklist

Before merging an interactive surface change in a consumer:

- [ ] Every interactive element has a visible focus ring
- [ ] Every form input has a paired visible label
- [ ] Every error message is announced via `aria-live` or `role="alert"`
- [ ] Every icon-only button has `aria-label`
- [ ] No tab-order surprises (DOM order matches visual)
- [ ] All text contrast verified against this doc's table
- [ ] Touch targets ≥ 44×44px; adjacent ≥ 8px apart
- [ ] Reduced-motion verified (toggle in OS settings, smoke-test the page)
- [ ] Screen-reader smoke test on at least one flow (VoiceOver / NVDA)
