# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a personal academic website built with the **al-folio** Jekyll theme, deployed to GitHub Pages. The site showcases research, projects, publications, CV, and blog posts for Yike (Eric) Tan, an AI Engineering graduate student at Carnegie Mellon University.

## Development Commands

### Local Development with Docker (Recommended)

```bash
# Pull and run the site locally (preferred method)
docker compose pull
docker compose up

# View at http://localhost:8080

# Use slim image (smaller size, same functionality)
docker compose -f docker-compose-slim.yml up

# Build from scratch (if needed)
docker compose up --build
```

### Legacy Local Development (Ruby/Bundler)

```bash
# Install dependencies
bundle install
pip install jupyter

# Serve locally
bundle exec jekyll serve

# View at http://localhost:4000
```

### Building and Deployment

```bash
# Build the site
export JEKYLL_ENV=production
bundle exec jekyll build

# Output will be in _site/ directory

# Purge unused CSS (production optimization)
npm install -g purgecss
purgecss -c purgecss.config.js
```

### Code Quality

```bash
# Format code with Prettier
npx prettier --write .

# Check formatting
npx prettier --check .
```

## Architecture

### Jekyll Collections

The site uses Jekyll collections to organize content:

- **`_news/`** - News items/announcements (displayed on homepage)
- **`_projects/`** - Project showcase pages with images and descriptions
- **`_pages/`** - Static pages (about, CV, blog, publications, repositories)
- **`_bibliography/`** - BibTeX files for publications (managed by jekyll-scholar)
- **`_data/`** - Structured data files:
  - `cv.yml` - CV data (fallback if `assets/json/resume.json` not used)
  - `repositories.yml` - GitHub repos and users to display
  - `coauthors.yml` - Publication co-author information
  - `venues.yml` - Publication venue abbreviations

### Layout System

Located in `_layouts/`:

- **`default.liquid`** - Base layout with HTML structure, head, header, footer
- **`about.liquid`** - Homepage/about page layout
- **`page.liquid`** - Simple page layout
- **`post.liquid`** - Blog post layout with metadata, comments, related posts
- **`distill.liquid`** - Academic article layout (distill.pub style)
- **`bib.liquid`** - Individual publication entry rendering
- **`cv.liquid`** - CV page layout

### Key Components

Located in `_includes/`:

- **`header.liquid`** - Navigation bar with search, theme toggle, social links
- **`footer.liquid`** - Site footer
- **`head.liquid`** - HTML head with meta tags, CSS, JS libraries
- **`metadata.liquid`** - Open Graph, Schema.org, and social metadata
- **`projects.liquid`** - Project grid display
- **`news.liquid`** - News/announcements list
- **`latest_posts.liquid`** - Recent blog posts
- **`cv/`** - CV section components (education, work, publications, etc.)

### Styling

Located in `_sass/`:

- **`_variables.scss`** - Theme colors, fonts, and global variables
- **`_themes.scss`** - Color theme definitions (light/dark mode)
- **`_base.scss`** - Base styles and utilities
- **`_layout.scss`** - Layout and component styles
- **`_cv.scss`** - CV page specific styles
- **`_distill.scss`** - Distill post styles

### Configuration

**`_config.yml`** contains all site configuration:

- Site metadata (title, description, author info)
- Social media links (GitHub, LinkedIn, Google Scholar)
- Feature flags (dark mode, math rendering, image lazy loading)
- Jekyll plugins configuration
- Collections and permalink structure
- Third-party library versions

## Important Patterns

### Publications System

Publications are managed via BibTeX files in `_bibliography/papers.bib`. The jekyll-scholar plugin processes these files. Supported BibTeX fields include:

- `pdf` - Path to PDF file (e.g., `assets/pdf/paper.pdf`)
- `abstract` - Paper abstract
- `code` - GitHub repo URL
- `blog` - Blog post URL
- `slides` - Slides URL
- `website` - Project website URL
- `arxiv` - arXiv ID
- `doi` - DOI identifier

### Content Front Matter

**News items** (`_news/`):
```yaml
---
layout: post
title: News title
date: 2025-01-31
inline: true  # For short one-line news
---
```

**Projects** (`_projects/`):
```yaml
---
layout: page
title: Project Name
description: Brief description
img: assets/img/project.jpg
importance: 1  # Sort order (1 is highest)
category: work  # or "fun"
---
```

**Blog posts** (`_posts/`):
```yaml
---
layout: post
title: Post Title
date: 2025-01-31
description: Meta description
tags: formatting images
categories: sample-posts
---
```

### Image Optimization

The site uses jekyll-imagemagick for responsive images. Images in `assets/img/` are automatically converted to WebP format at multiple sizes (480px, 800px, 1400px). Always use the original high-quality images; optimization happens during build.

### Theme Customization

To change the site's color theme, edit the `--global-theme-color` variable in `_sass/_themes.scss`. Additional theme color options are defined in `_sass/_variables.scss`.

## Deployment

The site auto-deploys to GitHub Pages via GitHub Actions (`.github/workflows/deploy.yml`) on every push to `master` or `main` branch. The workflow:

1. Checks out code
2. Sets up Ruby and installs dependencies
3. Builds the site with Jekyll
4. Purges unused CSS
5. Deploys to `gh-pages` branch

Manual deployment can be triggered from the Actions tab.

## Critical Files

- **`_config.yml`** - Main site configuration (modify with care)
- **`_pages/about.md`** - Homepage content
- **`assets/json/resume.json`** - CV data (JSON Resume format)
- **`_data/repositories.yml`** - GitHub repos to display on /repositories/ page
- **`_bibliography/papers.bib`** - Publications database

## External Dependencies

Key Jekyll plugins (defined in `Gemfile`):
- `jekyll-scholar` - Publications management
- `jekyll-imagemagick` - Image optimization
- `jekyll-minifier` - HTML/CSS/JS minification
- `jekyll-paginate-v2` - Blog pagination
- `jekyll-jupyter-notebook` - Jupyter notebook support
- `jemoji` - Emoji support

JavaScript libraries are loaded from CDN (versions configured in `_config.yml` under `third_party_libraries`).

## Notes

- The site supports both light and dark modes with automatic detection
- Search functionality is enabled via the navbar search component
- BibTeX search is available on the publications page
- All content pages should be markdown files in `_pages/`, not standalone HTML files
- Custom Liquid filters and tags may be defined in `_plugins/`
