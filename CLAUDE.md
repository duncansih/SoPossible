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

## Working conventions
- Before implementing any non-trivial feature, ask clarifying
  questions about scope, edge cases, and constraints first —
  don't propose a plan until you've asked.

## Feature Plan

### Phase 1 — Portal skeleton + 2 tools (in progress)
Single `index.html`, Tailwind CDN, top nav of anchor links to each tool
section (pattern future tools follow). Site title is a placeholder
("Web Tools & Learning Lab") for now.

- [ ] Shell: header, placeholder title, anchor nav, footer
- [ ] Dark/light theme toggle, persisted, no flash-of-wrong-theme on load
- [ ] Tool 1: Atmospheric Altitude Explorer — slider (0–600km, non-linear
      scale) → layer name, approx. temperature, one-line fact
- [ ] Tool 2: Color Palette Generator — generate 5 random hex swatches,
      click a swatch to copy its hex

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
- **Altitude slider**: drag → map slider position through a cubic curve to
  an altitude (km) → interpolate temperature from the reference table →
  look up layer/fact by altitude → update the readout.
- **Palette generate/copy**: click Generate → produce 5 random hex colors
  → render swatches (also run once on page load). Click a swatch →
  `navigator.clipboard.writeText(hex)` → show "Copied!" on that swatch for
  ~1.5s.
