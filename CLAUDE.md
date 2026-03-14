# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A [Hugo](https://gohugo.io/) static website using the [Blowfish theme](https://blowfish.page/) (installed as a git submodule at `themes/blowfish/`). The site is in German (`defaultContentLanguage = "de"`).

## Commands

```bash
# Start dev server with live reload
hugo server

# Build site to public/
hugo

# Create new content
hugo new content/posts/my-post/index.md
```

## Architecture

**Configuration** lives in `config/_default/`:
- `hugo.toml` — site-wide settings (language, pagination, taxonomies)
- `params.toml` — Blowfish theme options (layout, color scheme `avocado`, homepage style `background`)
- `languages.de.toml` / `languages.en.toml` — per-language author info and metadata
- `menus.de.toml` / `menus.en.toml` — navigation menu items

**Content** in `content/` uses page bundles (folder + `index.md`) for posts with images, or single `.md` files for simpler pages:
- `content/posts/` — blog posts with front matter (`title`, `date`, `draft`, `summary`, `tags`)
- `content/fotos/` — photo albums using raw `<img>` tags referencing local images
- `content/videos/` — video pages using the `{{< youtube id="..." >}}` shortcode

**Assets**: `assets/profile.jpg` is referenced in language config as the author image (`profile.jpg`). Background image `background.jpg` is referenced in `params.toml` but must exist at the site root assets level.

**Theme customization**: Do not edit files in `themes/blowfish/` — override by creating matching files in the site root (e.g., `layouts/`, `assets/`). The theme is a git submodule.

## Content front matter

Posts support: `title`, `date`, `draft`, `summary`, `tags`, `categories`, `series`, `authors`

Videos use YouTube shortcode: `{{< youtube id="VIDEO_ID" >}}`

Photo albums place images next to `index.md` in a page bundle and reference them with relative paths.
