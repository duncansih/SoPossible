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
