# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a Jekyll-based slideshow site built on [reveal.js](https://github.com/hakimel/reveal.js/), originally created as a GitHub Learning Lab course template. It is intended to be hosted on GitHub Pages.

## Commands

```sh
script/setup      # Install Ruby gem dependencies and init git submodules
script/server     # Run the local dev server (bundle exec jekyll serve)
script/cibuild    # Build the site and run html-proofer against index.html
script/stage      # Build and deploy to an internal staging environment
```

There are no test commands beyond `script/cibuild`, which builds with Jekyll and validates HTML links.

## Architecture

### How slides are built

`index.html` is the single entry point. It uses the `presentation` layout and iterates `site.posts` in reverse chronological order, including `_includes/slide.html` for each post. Each post in `_posts/` becomes one `<section>` element inside the reveal.js `.slides` container.

Slide ordering is controlled by the date in the filename (`YYYY-MM-DD-title.md`). Lower dates render first because posts are iterated in `reversed` order.

### Layouts

- `_layouts/presentation.html` — outer HTML shell for the full slideshow; wraps reveal.js `.reveal > .slides`
- `_layouts/slide.html` — standalone single-slide HTML page (used for individual slide previews)
- `_layouts/print.html` — print/PDF export variant; no JS initialization

### Front matter for slides

| Key | Effect |
|-----|--------|
| `layout: slide` | Required for every post |
| `title` | Rendered as `<h1>` at the top of the slide |
| `slide-id` | Sets the `id` attribute on the `<section>` |
| `classes` | Array of CSS classes added to the `<section>` |
| `data` | Key/value pairs added as `data-*` attributes (reveal.js options per slide) |

### reveal.js integration

reveal.js lives in `node_modules/reveal.js/`. Assets are referenced directly from there (no build step copies them). `_includes/script.html` initialises `Reveal` with the markdown, highlight, and notes plugins. Global reveal.js settings (transition, dimensions, controls, etc.) are configured in `_config.yml` under the `reveal:` key.

The `solarized.theme` config key (`dark`/`light`) is rendered as a class on `<html>` in `_layouts/presentation.html`.

## Code style

EditorConfig is enforced (`.editorconfig`):
- HTML, JS, CSS, SCSS, YML, JSON: 2-space indent
- Markdown (`_posts/`): 4-space indent, trailing whitespace **not** trimmed, final newline required
- Everything else: tabs, 4-space width
