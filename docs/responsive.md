# Responsive design

The system is responsive by **two complementary mechanisms**: `clamp()`
on values that should scale fluidly (type, spacing, paddings) and
`@media` breakpoints for layout shifts that can't be expressed
fluidly (collapsing nav to hamburger, stacking columns, hiding the
ASCII wordmark).

This doc tells you which to reach for, where the existing breakpoints
sit, and what behavior is expected at each one.

---

## 1. Breakpoints

The system uses **four breakpoints**, named for the surface they
shape. Stop adding new ones — anything that doesn't fit one of these
is probably better solved with `clamp()`.

| Name | Threshold | Encoding | Used for |
|---|---|---|---|
| **Container max** | 1060px | `max-width: 1060px` on `.lh` | the terminal-window content cap |
| **Tablet narrow** | ≤ 768px | `@media (max-width: 768px)` | padding tightens, footer wraps, separators clip |
| **Mobile** | ≤ 640px | `@media (max-width: 640px)` | hamburger nav, ASCII hides, post head stacks, meta-grid stacks, footer fully stacks, name shrinks, `.lh__field-row` stacks |
| **Very narrow** | ≤ 380px | `@media (max-width: 380px)` | chrome title hides, padding shrinks further, hero shrinks |

Existing media queries live in `editorial.css`. Search for
`@media (max-width:` to see exact behavior at each threshold.

### Why these specific numbers

- **1060px** — chosen for editorial reading. Wide enough for
  comfortable prose at 76ch (~600px) plus generous side padding on
  desktop, narrow enough that on a 13" laptop the `.lh` window
  doesn't bleed into the bezel.
- **768px** — the iPad-portrait threshold. Below this, single-column
  rules; touch targets matter more than mouse precision.
- **640px** — the practical phone-portrait threshold. The point at
  which the hamburger collapse and stacking become non-negotiable.
- **380px** — small-phone safety net (iPhone SE 1st-gen, older
  Androids). Chrome title hides, hero font drops to 20px.

### Custom property note

CSS custom properties **do not work inside `@media (max-width: var(--bp))`**
in current browsers (lack of property substitution at parse time).
Breakpoint values stay as literals in `@media`. If you need a
component-internal breakpoint, use `@container` (CSS Container
Queries) — but the v0.3.x system doesn't ship any container queries
yet.

---

## 2. Fluid (`clamp()`) vs breakpoint (`@media`)

The system uses both, deliberately.

### Use `clamp()` when

The value should slide **continuously** across viewport sizes. The
visual rhythm is the same; only the magnitude changes.

```css
/* Hero font — 28px on phone, 56px on desktop, smooth between */
font-size: clamp(28px, 5.5vw, 56px);

/* Page padding — 24px on phone, 56px on desktop */
padding: clamp(24px, 3.5vw, 56px);
```

The system already exposes the most-needed fluid scales as tokens
(`--fs-hero`, `--page-pad`, `--section-gap`, `--rule-gap`,
`--sep-gap`, etc.). Reach for these before authoring a new
`clamp()` inline.

### Use `@media` when

The change is **discrete** — direction, visibility, count of items,
layout topology. These can't be expressed fluidly without breaking
the rhythm.

```css
/* Mobile collapse — direction change, not a magnitude change */
@media (max-width: 640px) {
  .lh__nav { flex-wrap: wrap; }
  .lh__nav-toggle { display: block; }
  .lh__nav-links { display: none; flex-direction: column; }
  .lh__nav-links.is-open { display: flex; }
}
```

### Anti-pattern

Don't use `@media` to step type sizes when `clamp()` does it
smoothly. The system has **zero** `font-size` overrides keyed off
`@media` because the type ramp is fluid by design.

---

## 3. Container widths

| Surface | Width | Where |
|---|---|---|
| Terminal window | `max-width: 1060px` | `.lh` |
| Prose paragraph | `max-width: 76ch` | `.lh p` (editorial flow) |
| Wide prose / app rows | `max-width: none` | `.lh__post__content p`, `.lh__field` |
| Field row column | `minmax(220px, 1fr)` | `.lh__field-row` (auto-fit) |

### When to widen `.lh` for app surfaces

Don't. The 1060px cap is a brand commitment — every Arcane Labs
surface, editorial or app, fits inside the terminal window. If a
data-heavy app surface needs more horizontal real estate (a wide
table, a multi-column dashboard), the answer is **don't put it
inside `.lh`** — use a bare app shell with no terminal chrome at
all. v0.4's table primitive will define this convention.

### When to drop `max-width: 76ch`

For app form surfaces, helper text, and any flow that isn't long-form
prose. The `.lh__field__helper` class already opts out (no
`max-width`); the wider data-app patterns will too.

---

## 4. Desktop-first convention

The system is **desktop-first**: base styles target the desktop
appearance; `@media (max-width: ...)` queries shrink down. Every
existing breakpoint is `max-width:`, never `min-width:`.

### Why

Two reasons. First, the originating implementation
(arcanelabs.info) was desktop-primary — the editorial reading
experience is built for a 76ch column on a 13"+ display. Second,
the brand metaphor (a macOS terminal window with traffic-light
chrome) reads first as a desktop affordance; the mobile rendering
is the *adaptation*, not the canonical view.

### What this means for new authors

- Author new components targeting desktop appearance first
- Add `@media (max-width: 640px)` (or 768px) blocks to handle the
  collapses
- **Don't** mix `min-width` and `max-width` queries in the same
  stylesheet — pick one direction and stick with it (the system's
  direction is `max-width:`)

This is a deliberate constraint. If a future v0.x decides to flip
to mobile-first (min-width) — for a mobile-primary app — that's a
MAJOR break (every breakpoint inverts), so think carefully before
proposing it.

---

## 5. What changes at each breakpoint

A complete inventory of the system's responsive behavior, sourced
from `editorial.css`.

### Below 768px (tablet narrow)

- `body.editorial` padding: 12 / 10 (was 16 / 12 on desktop)
- `.lh__page` padding: clamp(20, 3vw, 36) (was 24, 3.5vw, 56)
- `.lh__ascii` font-size shrinks
- `.lh__footer p` wraps as a flex row
- `.lh__sep` overflow clipped with ellipsis

### Below 640px (mobile)

The big collapse:

- `body.editorial` padding: 8 / 6
- `.lh` border-radius: 6px (was 10)
- `.lh__chrome` padding tightens; dot size drops to 10px
- `.lh__chrome-title` font-size: 10px
- `.lh__page` padding: 18 / 14
- **Nav collapses to hamburger.** `.lh__nav-toggle` becomes visible;
  `.lh__nav-links` hides until `.is-open`. Border between brand
  and links removed.
- **`.lh__name` shrinks** to `clamp(22px, 7vw, 28px)`
- **`.lh__ascii` hides entirely** — doesn't scale readably
- `.lh__post__head` stacks vertically
- `.lh__post__body` padding tightens
- `.lh__post__title` shrinks
- `.lh__meta` collapses to single column
- `.lh__footer` p stacks as flex column; dot separators hide; gap
  added between p's
- `.lh__sep` font-size 11px, ellipsis on overflow
- `.lh blockquote` padding tightens
- `.lh pre` and `.lh__post__content pre` padding + font tighten
- `.lh__rules li` columns: 28px / 1fr (was 32 / 1fr); padding tightens
- `.lh__greeting` font-size: 12px
- `.lh__prompt`, `.lh__output` font-size shrinks
- **`.lh__field-row` stacks to single column** (added in v0.3.0)

### Below 380px (very narrow)

- `body.editorial` padding: 4
- `.lh` border-radius: 4
- `.lh__page` padding: 14 / 10
- `.lh__ascii` hides (already hidden at 640px; defensive)
- `.lh__name` font-size: 20px (fixed, not clamp)
- `.lh__post__title` font-size: 18px
- `.lh__chrome-title` hides

---

## 6. Touch density at small sizes

**Touch targets do not relax on mobile.** `--touch-target-min` is
44px at every breakpoint. Buttons and form fields enforce this via
`min-height` regardless of viewport.

What *does* shrink on mobile: padding, font-size, gap. Never the
floor on tap area.

If a desktop-only secondary control (e.g. `.lh__btn--sm` at 32px)
appears on a mobile surface, it's a smell — either upgrade it to
default size or remove it.

---

## 7. Orientation

The system reflows by **viewport width**, not orientation. Landscape
on a phone behaves like a slightly wider phone — same breakpoints
apply. The system has no landscape-specific queries, no
`@media (orientation: landscape)`, and shouldn't gain any.

---

## 8. Container queries (v0.5+ candidate)

Component-level responsive behavior — a card that lays itself out
differently when narrow because it's in a sidebar, vs. wide because
it's in a main column — needs CSS Container Queries (`@container`).
The current system uses **viewport-based `@media` queries only**.

This is fine for v0.3.x components (buttons, fields) because their
layout is straightforward. When v0.4+ introduces tables, dashboards,
or sidebar/main layouts where the same component lives at multiple
container widths, container queries become the right tool. Until
then: viewport queries, named breakpoints, `clamp()` for fluids.

---

## 9. Quick reference

```css
/* Fluid — first reach */
font-size: var(--fs-hero);              /* 28→56px clamp */
padding: var(--page-pad);               /* 24→56px clamp */

/* Discrete — when fluid doesn't fit */
@media (max-width: 768px) { /* tablet narrow */ }
@media (max-width: 640px) { /* mobile */ }
@media (max-width: 380px) { /* very narrow */ }

/* Container — single 1060px cap */
.lh { max-width: 1060px; }

/* Touch — never relaxed */
.lh__btn { min-height: var(--touch-target-min); }   /* 44px floor */
```
