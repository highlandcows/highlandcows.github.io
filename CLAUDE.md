# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A personal blog and website for Highland Cows (highlandcows.com), built with Jekyll using the [Chirpy theme](https://github.com/cotes2020/jekyll-theme-chirpy/). Deployed automatically to GitHub Pages on push to `main`.

## Commands

**Local development (with live reload):**
```bash
bash tools/run.sh
```

**Production mode locally:**
```bash
bash tools/run.sh --production
```

**Build and test (htmlproofer):**
```bash
bash tools/test.sh
```

**Direct Jekyll commands:**
```bash
bundle exec jekyll serve -l    # serve with live reload
bundle exec jekyll build       # build to _site/
```

## Writing posts

Posts live in `_posts/` and follow the naming convention `YYYY-MM-DD-slug.md`.

Required front matter:
```yaml
---
title: Post Title
date: YYYY-MM-DD
categories: [category]
tags: [tag1, tag2]
---
```

The `_plugins/posts-lastmod-hook.rb` plugin automatically sets `last_modified_at` from git history for posts with more than one commit.

## Architecture

- **Theme**: `jekyll-theme-chirpy` gem — layout, assets, and most functionality come from the gem, not this repo.
- **`_config.yml`**: Primary configuration. Site identity, social links, and theme settings are all here.
- **`_tabs/`**: Sidebar navigation pages, ordered by their `order` value (Categories, Tags, Archives, Feeds, Japanese Study, About). `japanese-study.html` is a redirect page: Chirpy builds each sidebar link from the tab's own URL, so an external destination (the Japanese study app on its own GitHub Pages site) needs a tiny page that redirects. It is `.html`, not `.md`, so Jekyll does not run it through Markdown.
- **`_data/share.yml`**: Social sharing button configuration.
- **`_site/`**: Build output — committed to the repo (contains pre-built JS assets from the theme).
- **`tools/`**: Helper scripts excluded from Jekyll build.
- **Deployment**: GitHub Actions (`.github/workflows/pages-deploy.yml`) builds and deploys to GitHub Pages on push to `main`.

## Key config

- Site URL: `https://highlandcows.com`
- Timezone: `Africa/Johannesburg`
- Posts paginate at 10 per page
- Post permalinks: `/posts/:title/`
- PWA enabled
