# ZBOS UI Scaffold — Lane G deliverable

Static HTML + CSS wireframe shells for the May 18 demo storyboard.
No JavaScript, no framework — these are layout intent + visual language only.

## Open

After `pnpm dev` from `apps/web/` (Lane F's Next setup), the shells are served at:

- `/scaffold/index.html` — index page linking everything below
- `/scaffold/dashboard.html` — Daily Briefing (storyboard frame 2)
- `/scaffold/pos.html` — Point of Sale (frame 3)
- `/scaffold/kds.html` — Kitchen Display, dark theme (frame 4)
- `/scaffold/roster.html` — Roster + AU Modern Award penalty drawer (frame 5)
- `/scaffold/inventory.html` — Recipe cost + low-stock auto-flag (frame 6)
- `/scaffold/insights.html` — AI Copilot grounded answer (frame 7)
- `/scaffold/components.html` — Top-Nav · Side-Nav · Tenant-Switcher in isolation

## Tokens

`./tokens.css` is a copy of the canonical source at `apps/web/src/styles/tokens.css`.
Update both together until the Next.js build pipeline takes over the public copy.

## Out of scope for v1 scaffold

- Login / PIN entry (storyboard frame 1) — covered by the Figma reference
- Wrap roadmap slide (frame wrap) — produced as a static slide deck, not in the web app

## Brand intent

Square Dashboard / Lightspeed minimal. Monochrome chrome. Blue 600 reserved for
primary CTAs and active states. Status colours (red / orange / green) used
sparingly and only for state. KDS is the only intentionally-dark surface in v1
(reads at 3 metres).

## Accessibility

Phase-0 baseline targets WCAG 2.2 AA. Closed in this scaffold:

- 2.4.1 — `Skip to content` link (visible-on-focus) at the top of every shell.
- 2.4.7 / 2.4.13 — keyboard focus is a solid 2px outline at `var(--focus-ring-color)` with 2px offset; never `outline: none`.
- 1.4.1 — status colour is never the sole signal: pills carry `●`/`⚠` glyphs, KPI deltas carry `▲`/`▼`, status text uses `+`/`−` prefixes.
- 1.4.3 — body text uses 700-variant tokens (`--text-positive` / `--text-warning` / `--text-negative`) which clear 4.5:1 on white. The 600 variants live on `--text-*-large` for ≥18pt headings + icons.
- 2.5.8 — `.zb-chip` minimum 30px tall (clears the 24x24 floor new in 2.2).
- 2.5.7 — drag-only flows (Roster open-shift pool) include an inline `Assign` button alternative.
- 1.3.1 — Roster uses a real `<table role="grid">` with `scope="col"` / `scope="row"` headers; POS search has an explicit `aria-label`.
- Forced colours — `tokens.css` carries a `@media (forced-colors: active)` block that hands focus and borders to system colours.
- Reduced motion — animations clamped to 0.01ms when `prefers-reduced-motion: reduce`.

Tracked to Phase 1 (per Gate G enhance items):

- 3.3.7 / 3.2.6 / 3.3.7 — login + auth UI accessibility.
- Tenant-switcher `aria-haspopup="listbox"` + `aria-expanded` once the JS wire-up lands.
- Toast / error pattern with `role="status"` / `aria-live="polite"`.

## CSP / inline styles

Each scaffold HTML file currently carries a per-shell `<style>` block. This is
**scaffold-only**. A production CSP of `style-src 'self'` will reject inline
styles unless a hash or nonce is added. The Phase 1 Next.js build replaces
these with externalised CSS modules (or a per-route nonce via
`next/headers`); update `secops/CSP.md` when that lands.

## Tokens duplication

`./tokens.css` mirrors the canonical `apps/web/src/styles/tokens.css` so the
static HTML scaffolds work without a build step. **Update both together until
the Next.js build pipeline takes over** — at which point the public mirror
should be deleted and the HTML scaffolds either retired or pre-rendered.
