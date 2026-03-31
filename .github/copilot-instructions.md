# Copilot Instructions

This is a Jekyll-based personal blog hosted on GitHub Pages.

## Local Development

```bash
# Serve with drafts visible
bundle exec jekyll serve --drafts

# Serve published posts only
bundle exec jekyll serve
```

The site is served at `http://localhost:4000` by default. The `run-local.sh` script wraps the `--drafts` variant.

## Architecture

- **`_layouts/default.html`** — Master layout. Contains the header (site navigation, author links, quick links sidebar), Google Analytics hook, and jekyll-seo-tag integration. All pages inherit from this.
- **`_layouts/post.html`** — Extends `default`. Renders post title, date, author, content, and a Disqus comments block (currently disabled).
- **`index.html`** — Home page. Iterates `site.posts` to list all published posts in reverse chronological order.
- **`_config.yml`** — Site metadata (author info, social links, theme, plugins).
- **`_posts/`** — Published posts (included in builds).
- **`_drafts/`** — Work-in-progress posts (only included when serving with `--drafts`).

## Post Conventions

**File naming — published posts:**
```
_posts/YYYY-MM-DD-kebab-case-title.md
```

**File naming — drafts** (no date prefix):
```
_drafts/kebab-case-title.md
```

**Front matter** (all three fields required for every post):
```yaml
---
layout: post
title: My Post Title
comments: true
---
```

- `layout` is always `post`
- `comments: true` enables Disqus (wired up in `_layouts/post.html` but currently disabled site-wide)
- No tags, categories, or excerpts are used

## Spell Checker

Custom words are maintained in `.vscode/settings.json` under `cSpell.words`. Add technical terms there to avoid false positives.
