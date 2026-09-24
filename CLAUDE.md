# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Personal academic website (About, CV, Research, Teaching, Blog) built with Jekyll and the remote `minima` theme, hosted from the GitHub repo `longeford/Corporate-Analysis` (GitHub Pages). The blog covers corporate finance theory and real analysis. There is no Gemfile, build script, linter, or test suite; GitHub Pages builds the site on push.

## Local preview

No Gemfile is committed. To preview locally you need Ruby + Jekyll with the `minima` gem (or the `github-pages` gem), e.g.:

```
gem install jekyll minima
jekyll serve
```

Output goes to `_site/` (gitignored). Always build internal links/assets with `relative_url` — as a project site, GitHub Pages serves it under a `/Corporate-Analysis` baseurl that it injects automatically.

## Architecture

- `_config.yml` holds all site-level settings the pages read from: `author.name`/`author.email`, contact links (`linkedin_username`, `scholar_url`, `institution_url`, `github_username`), `footer.address`/`footer.disclaimer`, optional `photo` and `banner` (About page shows them only if set), `cv_dropbox_url`, `header_pages` (defines the nav tabs and their order), and the blog `permalink` (`/blog/...`). Optional features are driven by whether a key is set, not by flags.
- Top-level pages: `index.md` (About, `layout: about`, served at `/`), `cv.md`, `research.md`, `teaching.md`, `blog.md` (`layout: home`, which lists `site.posts`). The site title (the owner's name) is the only place the name appears; the About page deliberately has no name heading.
- `_layouts/` overrides minima's layouts of the same name; all layouts inherit from `base.html`. `about.html` is local-only. Local `_includes/head.html` and `footer.html` replace minima's; `header.html` (the nav, driven by `header_pages`) still comes from the minima gem.
- Local includes: `contact-icons.html` (inline-SVG icon row under the bio), `figure.html` (image with optional caption for use in page Markdown), `dropbox-pdf.html` (CV embed).
- **CV embed**: `_includes/dropbox-pdf.html` takes a Dropbox shared file link, rewrites `dl=0` → `raw=1` for an `<iframe>` viewer and `dl=1` for a download link. Updating the CV means overwriting the same file in Dropbox — the shared link stays the same.
- **Styles**: `assets/main.scss` sets minima's Sass variables (font, colours, `$content-width`) *before* `@import "minima"`, then adds custom rules. It needs its empty front matter to be compiled.
- **`_includes/custom-head.html`** (included inside `<head>` by the local `head.html`) loads the Lato Google Font and MathJax v3. The `MathJax` config object (inline `$...$` / `\(...\)`, display `$$...$$` / `\[...\]`) must stay before the `tex-svg.js` script tag.
- `_posts/` — posts named `YYYY-MM-DD-slug.md` with front matter `layout: post`, `title`. LaTeX is written directly in Markdown using the delimiters above.

## Repo notes

- `main` is the default branch on GitHub; push changes there.
- A previous `_projects` collection and `project` layout were deleted; don't reintroduce references to them.
