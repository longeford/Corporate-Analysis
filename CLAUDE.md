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

- `_config.yml` holds all site-level settings the pages read from: `author.name`/`author.affiliation`, optional `photo` (About page shows it only if set), `cv_dropbox_url`, footer social usernames, `header_pages` (defines the nav tabs and their order), and the blog `permalink` (`/blog/...`).
- Top-level pages: `index.md` (About, `layout: about`, served at `/`), `cv.md`, `research.md`, `teaching.md`, `blog.md` (`layout: home`, which lists `site.posts`).
- `_layouts/` overrides minima's layouts of the same name; all layouts inherit from `base.html`. `about.html` is local-only. Theme includes not overridden locally (`head.html`, `header.html`, `footer.html`, …) come from the minima gem; the nav is minima's `header.html` driven by `header_pages`.
- **CV embed**: `_includes/dropbox-pdf.html` takes a Dropbox shared file link, rewrites `dl=0` → `raw=1` for an `<iframe>` viewer and `dl=1` for a download link. Updating the CV means overwriting the same file in Dropbox — the shared link stays the same.
- **Styles**: `assets/main.scss` imports `minima` and adds custom rules (profile photo, PDF iframe). It needs its empty front matter to be compiled.
- **Math rendering** is split across two files and both are required:
  - `_includes/custom-head.html` sets the `MathJax` config object (inline `$...$` / `\(...\)`, display `$$...$$` / `\[...\]`). It is included explicitly by `base.html` right after minima's `head.html`.
  - `base.html` loads the MathJax v3 `tex-svg.js` script from jsDelivr at the end of the document. The config must be defined before this script runs.
- `_posts/` — posts named `YYYY-MM-DD-slug.md` with front matter `layout: post`, `title`. LaTeX is written directly in Markdown using the delimiters above.

## Repo notes

- `main` is the default branch on GitHub; push changes there.
- A previous `_projects` collection and `project` layout were deleted; don't reintroduce references to them.
