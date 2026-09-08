# CLAUDE.md

Guidance for Claude Code (and any other agent) working in this repository.

## Stack & Conventions

- **Single-file project.** The entire project must live in one `index.html`
  file, with all CSS in inline `<style>` tags and all JavaScript in inline
  `<script>` tags — no separate `.css` or `.js` files, and no additional
  HTML pages. Linking to external images, CSS libraries, and JavaScript
  libraries (e.g. via `<link>`/`<script src="https://...">`) is allowed.
  This constraint exists so the finished project can be copy-pasted as a
  single file to share in class and on single-file code platforms
  (e.g. CodePen, JSFiddle).
- **Vanilla only.** Use plain HTML, CSS, and JavaScript only — no
  frameworks or libraries bundled into the project (React, Vue, Tailwind
  build, jQuery as a dependency, etc.) beyond what's linked externally per
  above, and no build step (no bundlers, transpilers, or package manager
  scripts required to run the project). Open `index.html` directly in a
  browser and it should work.

## Tech stack (hard constraints — do not deviate)
- Vanilla HTML, CSS, and JavaScript only. No React, Vue, or any JS framework.
- Tailwind CSS for all styling (via CDN only).
- No backend, no database. Fully static site.
- A toggle for light and dark theme, with the choice remembered
  across visits.

## Design Direction (standing rule — apply to every screen)

Visual direction is settled: **"Terminal Lab"** — dark-mode-first, techy,
monospace-accented. Every section of the portal, including tools added
later, follows this look automatically; don't re-derive or vary it per
tool.

- **Typography**: `Space Grotesk` (500/600/700) for headings and UI chrome;
  `JetBrains Mono` (400/500/700) for nav items, data readouts/labels,
  buttons, and any code-like or comment-style text (`// ...`, `# ...`).
  Both via Google Fonts.
- **Palette** (defined as light/dark pairs, dark is the default state):
  - Dark: `bg oklch(14% 0.015 260)` · `text oklch(92% 0.02 200)` ·
    `subtext oklch(64% 0.03 210)` · `card-bg oklch(19% 0.02 260)` ·
    `card-border oklch(32% 0.02 260)` · `grid-line oklch(28% 0.02 260 / 55%)`
  - Light: `bg oklch(96% 0.01 260)` · `text oklch(18% 0.02 260)` ·
    `subtext oklch(45% 0.02 255)` · `card-bg oklch(100% 0.005 260)` ·
    `card-border oklch(85% 0.02 260)` · `grid-line oklch(88% 0.01 260 / 70%)`
  - Accents (same in both modes for now): `accent` neon cyan `#4dd6d6`,
    `accent2` neon magenta `#e069c7` — alternate between them for
    labels/values so nothing reads monotone.
- **Spacing/shape**: small corner radii only (2–4px, never pill/fully
  rounded), 1px hairline borders in `card-border`, generous card padding
  (~28px), section cards labeled with a small monospace tag overlapping
  the top border (`SECTION://...`) like a fieldset legend.
- **Signature motifs**: a terminal-window chrome bar at the very top
  (three dot "window controls" + `learning-lab@sp-poly:~$` prompt label);
  page heading prefixed with `>` in accent color, suffixed with a blinking
  monospace cursor (`_`); subtle repeating-grid background (32px cells,
  `grid-line` color); nav items as `[01] item-name` with the bracketed
  number colored accent/accent2; buttons styled as CLI commands (e.g.
  `$ generate --palette`); slider thumbs as small glowing diamonds
  (rotated square, `box-shadow` glow in the accent color).
- **Theme toggle**: implemented as a bordered button reading `--light` /
  `--dark` (monospace), not just an icon.

## Working conventions
- Before implementing any non-trivial feature, ask clarifying
  questions about scope, edge cases, and constraints first —
  don't propose a plan until you've asked.

## Feature Plan

### Phase 1 — Portal skeleton + 2 tools (in progress)
Single `index.html`, Tailwind CDN, top nav of anchor links to each tool
section (pattern future tools follow). Site title is a placeholder
("Web Tools & Learning Lab") for now. Styled per "Design Direction"
above (Terminal Lab).

- [x] Shell: header, placeholder title, anchor nav, footer
- [x] Dark/light theme toggle, persisted, no flash-of-wrong-theme on load
- [x] Tool 1: Atmospheric Altitude Explorer — slider (0–600km, non-linear
      scale) → layer name, approx. temperature, one-line fact
- [ ] Tool 2: Color Palette Generator — generate 5 random hex swatches,
      click a swatch to copy its hex
      (card shell + open/close wired; interactive logic pending)

### Phase 2+ — Add more tools (not yet scoped)
Each new tool = one new `<section id="...">` following the Phase 1 shape
(heading, short blurb, tool markup, scoped `<script>` logic) + one new
`<li>` in the top nav. No dedicated phase entry needed until a specific
tool is scoped with the user.

### Data model
Everything is client-side, in `index.html`, no backend:
- **Theme**: single `localStorage` key (`theme`: `"light"` | `"dark"`),
  read before first paint to set the `dark` class on `<html>`.
- **Atmosphere reference table**: JS array of `{altitudeKm, tempC}` points
  (0 → 600km) used for piecewise-linear interpolation, plus a small lookup
  of layer boundaries → `{name, fact}` (troposphere/stratosphere/
  mesosphere/thermosphere). Thermosphere values above ~100km are
  illustrative, not precise.
- **Palette state**: transient in-memory array of 5 hex strings, rebuilt
  each "Generate" click; not persisted.

### Key flows
- **Theme toggle**: click → flip `dark` class on `<html>` → write choice
  to `localStorage`.
- **Nav open/close**: click a nav link → open its card if closed → smooth
  scroll to it. Click a card's close control → hide its body (card stays
  in the page, collapsed to its header). Visibility uses the native
  `hidden` attribute (via `setAttribute`/`removeAttribute`, not the `.hidden`
  DOM property — that property doesn't reliably reflect back to the
  attribute on SVG elements) so it works even if Tailwind fails to load.
- **Altitude slider**: drag → map slider position through a cubic curve to
  an altitude (km) → interpolate temperature from the reference table →
  look up layer/fact by altitude → update the readout.
- **Palette generate/copy**: click Generate → produce 5 random hex colors
  → render swatches (also run once on page load). Click a swatch →
  `navigator.clipboard.writeText(hex)` → show "Copied!" on that swatch for
  ~1.5s.
