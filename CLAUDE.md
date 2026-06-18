# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Personal blog and portfolio for Nick Barrett ([nickbarrett.me](https://nickbarrett.me)), built with Jekyll and the [Chirpy theme](https://github.com/cotes2020/jekyll-theme-chirpy) v7.0. Deployed to GitHub Pages via GitHub Actions.

## Build & Development Commands

```bash
bundle                          # Install Ruby gem dependencies
bundle exec jekyll serve        # Local dev server (http://localhost:4000)
bundle exec jekyll b            # Build site to _site/
JEKYLL_ENV=production bundle exec jekyll b  # Production build
bundle exec htmlproofer         # HTML validation (runs on _site/)
bash tools/deploy.sh            # Full build + test + deploy to gh-pages branch
```

The `assets/lib` directory is a git submodule (`chirpy-static-assets`). After a fresh clone, run `git submodule update --init` if assets are missing.

## Architecture

### Content Model

- **`_posts/`** — Blog posts. Filenames must be `YYYY-MM-DD-title-slug.md`. Front matter requires at minimum `layout: post`, `title`, `categories`, and `tags`.
- **`_tabs/`** — Static pages that appear as navigation tabs (About, Profile, Resume, Pi-hole, Archives, Categories, Tags).
- **`_layouts/`** — Custom layouts extending the Chirpy theme. `profile.html` is a bespoke layout for the Profile tab.
- **`_data/`** — Structured site data: `contact.yml` (social links), `share.yml` (share buttons), `locales/` (i18n strings).
- **`_plugins/`** — `posts-lastmod-hook.rb` automatically sets `last_modified_at` on posts by reading git log timestamps.

### Deployment Pipeline

Pushes to `main` trigger `.github/workflows/pages-deploy.yml`, which:
1. Builds with `JEKYLL_ENV=production`
2. Validates with html-proofer (external links skipped)
3. Deploys by force-pushing the `_site/` contents to the `gh-pages` branch

The `_site/` directory and `Gemfile.lock` are not committed to `main`; they're generated at build time.

### Theme Customization

This repo uses Chirpy as a gem (`jekyll-theme-chirpy`), so theme files live inside the gem — they are not in this repo. To override a theme file, copy it from the gem into the matching path here (e.g., `_layouts/`, `_includes/`, `assets/`). The `_layouts/profile.html` is one such override.

### Configuration

`_config.yml` is the single source of truth for site-wide settings: URL, author info, social links, Giscus comments (GitHub Discussions), pagination (10/page), and timezone (America/Chicago). Changes here affect the entire site.

## Content Conventions

- **Post front matter:** `layout: post`, `title`, `date` (optional, inferred from filename), `categories: [Category]`, `tags: [tag1, tag2]`, `comments: true`
- **Images:** Store in `assets/images/`. Reference as `/assets/images/filename.ext`.
- **Line endings / indentation:** EditorConfig enforces LF line endings and 2-space indentation across all file types.
- **Sass:** Output is compressed; custom styles go in `assets/css/` following Chirpy conventions.
