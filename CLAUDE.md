# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a static GitHub Pages personal website (served at droppedmic.github.io) built from the Start Bootstrap "Heroic Features" HTML template. There is no build system, package manager, test suite, or linter — the site is plain HTML/CSS/JS committed directly to the repository.

The default branch is `master`, and GitHub Pages serves it as-is: whatever is pushed to `master` is deployed automatically.

## Running Locally

There are no build steps. Open `index.html` directly in a browser, or serve the directory with any static file server, e.g.:

```bash
python3 -m http.server 8000
```

Note: the IE8 shims in `index.html` (html5shiv/respond.js) don't work over `file://`, so prefer a local server if that matters.

## Structure

- `index.html` — the entire site; a single landing page (navbar, jumbotron header, four feature cards, footer). Most nav links and buttons are placeholders (`href="#"`).
- `css/heroic-features.css` — the only custom stylesheet (template spacing/layout tweaks). Site-specific style changes go here.
- `css/bootstrap.css` / `css/bootstrap.min.css` — vendored Bootstrap 3.3.4. Do not hand-edit.
- `js/jquery.js`, `js/bootstrap.js`, `js/bootstrap.min.js` — vendored jQuery 1.11.1 and Bootstrap JS. Do not hand-edit.
- `fonts/` — Glyphicons font files required by Bootstrap 3.

## Conventions

- This site uses Bootstrap 3 (not 4/5): grid classes are `col-md-*`/`col-sm-*`, navbar uses `navbar-inverse`/`navbar-fixed-top`, and components like `jumbotron` and `thumbnail` are Bootstrap 3 idioms. Keep new markup consistent with Bootstrap 3 class names.
- The page loads the minified vendor assets (`bootstrap.min.css`, `bootstrap.min.js`); the unminified copies are kept for reference only.
- Some feature-card images point at external placeholders (`placehold.it`, a Google Drive link) that may no longer resolve; replacing them with real, locally hosted images is a safe improvement when touching that section.
