# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

Personal website for Christos Markou, served by GitHub Pages from the `main` branch at https://chrsmark.github.io. There is no build step, no framework, and no package manager — the entire site is a single static `index.html`.

## Architecture

Everything lives in `index.html`:

- **HTML, CSS, and JS are all inline.** No external stylesheets or scripts other than Google Fonts (Inter, JetBrains Mono). When making styling changes, edit the `<style>` block; when adding behavior, prefer extending the existing inline `<script>` blocks rather than introducing new files.
- **Theming uses CSS custom properties.** The dark palette is defined in `:root` and overridden by `[data-theme="light"]` on `<html>`. Every color is referenced via `var(--name)` in the rules — **do not hardcode hex literals** in selectors; add a new variable to both blocks instead. The light palette is calibrated for WCAG AA contrast; preserve that when adjusting.
- **Theme toggle flow.** Two inline `<script>` blocks: (1) a pre-paint script in `<head>` reads `localStorage.theme` and sets `document.documentElement.dataset.theme` before the body renders (prevents flash); (2) a script before `</body>` wires the `#theme-toggle` button to flip the attribute and persist the choice. First visit defaults to dark; `prefers-color-scheme` is intentionally **not** honored.
- **Icons are inline SVGs using `fill="currentColor"`** so they inherit theme colors automatically. Match this pattern when adding new icons rather than loading an icon font.
- **One breakpoint** at `@media (max-width: 600px)`. Mobile-specific overrides live there.

## Local development

The site is fully static; just open `index.html` in a browser, or serve it:

```bash
python3 -m http.server 3000
```

This matches the `.claude/launch.json` config (untracked but kept on disk). There is no test suite, linter, or build pipeline to run.

## Deployment

GitHub Pages auto-publishes from `main` after each push. No release process; pushing to `main` is the release. There is no staging environment.

## Conventions worth respecting

- **Content lists (talks, interviews, blog posts)** follow the same `<ul class="items"><li>` structure with `.item-main`, `.item-title`, `.item-link`, and `.item-meta`. When adding a new entry, reuse this markup; don't introduce a new pattern.
- **Commit messages are short lowercase imperative** (e.g. `add echo news episode`, `fix link`). Match that style.
- **`.claude/` is gitignored** along with macOS/editor noise. Don't commit local tooling configs.
