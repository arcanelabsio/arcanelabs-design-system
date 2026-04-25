# Website UI kit

A working rebuild of the [arcanelabs.info](https://arcanelabs.info) company site, using the real `.lh__*` class names from `editorial.css`. This is plain HTML + a small hash-router, so every screen is directly visible in source.

## Screens

All screens live in `index.html`, switched via a URL-hash router:

| Hash | Screen |
|---|---|
| `#/` | Home — catalog list + ASCII wordmark + prompt block |
| `#/writing` | Writing — posts index |
| `#/post` | A single post (file-envelope chrome) |
| `#/project` | A project page (`forge.`) with meta grid |
| `#/company` | Company — principles |
| `#/contact` | Contact — meta-grid of channels |
| `#/anything-else` | 404 — with `$ ls` recovery |

Use the in-page nav or click any link. The `chrome-title` updates per route and the nav highlights the current section in mint.

## Files

- `index.html` — full app (six screens + 404).
- `editorial.css` — full port of the site stylesheet.
- `assets/favicon.svg` — site favicon.

## Divergences from the live site

- Uses a `#hash` router instead of `react-router`'s pathname router — makes the kit work as a static file with no server.
- Loads JetBrains Mono from Google Fonts CDN (same as the live site).
- No actual MDX posts; one representative post page is included as reference.
