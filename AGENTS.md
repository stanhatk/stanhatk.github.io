# AGENTS.md

## What this is

Jekyll personal site (fork of [al-folio](https://alshedivat.github.io/al-folio/)) deployed to GitHub Pages. Ruby + Jekyll toolchain, not a JS app.

## Quick commands

```bash
# Local dev (Docker, recommended)
docker compose pull && docker compose up    # full image
docker compose -f docker-compose-slim.yml up  # slim image

# Format check (requires pnpm for prettier)
pnpm install  # first time only
npx prettier . --check

# Format fix
npx prettier . --write
```

## Build & deploy

- Production build: `bundle exec jekyll build --lsi` (requires `JEKYLL_ENV=production`)
- CSS purge after build: `purgecss -c purgecss.config.js`
- Deploy script: `bin/deploy` (interactive, builds + pushes to `gh-pages`)
- CI auto-deploys on push to `master`/`main` via `.github/workflows/deploy.yml`
- CI requires: Ruby 3.2, imagemagick, jupyter (for notebook posts)

## Content structure

- `_pages/` — static pages (about, CV, blog index, publications, projects)
- `_posts/` — blog posts (Markdown, date-prefixed filenames)
- `_news/` — announcements shown on homepage
- `_projects/` — project cards
- `_bibliography/` — publication bib files (scholar plugin)
- `_data/` — YAML data (cv.yml, repositories.yml, coauthors.yml)
- `assets/json/resume.json` — JSON Resume feed into `_data/resume`
- `_layouts/` and `_includes/` — Liquid templates (theme overrides)

## Formatting

- Prettier with `@shopify/prettier-plugin-liquid` for `.liquid` files
- Print width: 150 (in `.prettierrc`)
- Pre-commit hooks: trailing-whitespace, end-of-file-fixer, check-yaml, check-added-large-files

## Gotchas

- `Gemfile.lock` is gitignored — run `bundle install` after cloning
- `node_modules/` is gitignored
- `_site/` is the build output, gitignored
- ImageMagick must be installed for responsive image generation (`convert -version` to verify)
- Jupyter posts require `jupyter-nbconvert` (in Docker image, not local by default)
- The `JEKYLL_ENV=production` env var is required for production builds
- `_config.yml` changes require a Jekyll restart (Docker entry point watches for this)
