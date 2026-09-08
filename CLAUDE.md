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
