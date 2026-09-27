# AGENTS.md

## Project overview

This repository contains the source for [blog.renzheng.me](https://blog.renzheng.me), a Chinese Hugo blog.

- **Source control:** GitHub (`renzhengzhang/blog.renzheng.me`)
- **Hosting and deployment:** Vercel
- **Static-site generator:** Hugo (the extended edition is required for the Diary theme's SCSS assets)
- **Theme:** `themes/diary`, tracked as the `renzhengzhang/hugo-theme-diary` Git submodule
- **Site configuration:** `hugo.toml`

Do not commit generated output from `public/`; it is intentionally ignored.

## Getting started

Clone the theme submodule before running Hugo:

```sh
git submodule update --init --recursive
```

Use the local development server for previewing changes:

```sh
hugo server --buildDrafts
```

Build the production site before submitting changes:

```sh
hugo --minify
```

This command writes the generated site to `public/`. Do not edit generated files or add them to Git.

## Content conventions

- Create posts under `content/posts/` as Markdown files. Use `hugo new posts/<slug>.md` to start from `archetypes/default.md`.
- Keep front matter in TOML (`+++` delimiters), matching existing content.
- New posts are drafts by default. Set `draft = false` before publishing.
- Supply a clear `title` and date with the `+08:00` timezone. Add `description`, `categories`, `tags`, and `featured_image` when appropriate.
- Use absolute site paths for static assets, for example `/images/example.jpg`. Put those assets in `static/images/`.
- Compress large images before adding them (resize to the display width and prefer WebP); a 5 MB photo used as `featured_image` dominates page weight.
- Preserve the site's Chinese-first content and existing Markdown conventions, including `<!--more-->` when an article needs an explicit summary break.

## Configuration and theme

- Keep the canonical production URL as `https://blog.renzheng.me/` in `hugo.toml`.
- Make site-level configuration changes in `hugo.toml`; do not modify theme files for a site-specific setting unless no supported theme parameter exists.
- The site-level `layouts/index.rss.xml` overrides the theme RSS template for Hugo compatibility. Optional RSS author metadata belongs under `[params.author]` (`name` and `email`).
- Treat `themes/diary` as an external dependency. Theme changes should normally be made in its own repository and recorded here by updating the submodule pointer.
- Do not put credentials, tokens, or private keys in `hugo.toml`, content files, or Git history. Use the hosting platform's environment-variable or secret settings where a secret is needed.

## Deployment

Vercel builds and serves the site from this repository. Changes merged or pushed to the configured production branch are deployed by Vercel; validate the generated site locally with `hugo --minify` before publishing.

Keep deployment configuration compatible with Hugo's default output directory (`public/`) unless the Vercel project configuration is updated at the same time.

### Vercel build environment

- Vercel currently uses Hugo `0.145.0` extended on Linux. This version is older than the local Hugo installation, so templates and configuration must remain compatible with it.
- The Vercel build command initializes the theme dependency before building:

  ```sh
  git submodule update --init --recursive && hugo --gc
  ```

- Use `languageCode` in `hugo.toml` and `.Site.Language.Lang` in templates. Hugo `0.145.0` does not support the newer `locale` configuration key or `.Site.Language.Locale`.
- The site-level RSS override must continue to avoid the Diary theme's removed `.Site.Author` API. Use the optional `[params.author]` values instead.
- If the Hugo version is changed in the Vercel project, run a production build with that exact version and verify the homepage, RSS (`/index.xml`), sitemap, and taxonomy pages before deploying.

### Performance-related site overrides

- `layouts/partials/head.html` overrides the Diary theme head. It self-hosts Material Icons from `static/fonts/` (the upstream `fonts.googleapis.com` stylesheet is render-blocking and unreachable in parts of China) and loads KaTeX only on pages whose rendered text contains math delimiters (`$`, `\(`, `\[`) or whose front matter sets `enableLaTeX = true`, so the ~270 kB CDN bundle is not fetched on every page.
- `vercel.json` defines cache headers: fingerprinted `/scss/` output is immutable for one year; `/fonts/`, `/images/`, `/vendor/`, and `/js/` use long-lived revalidating caches.

## Change expectations

- Keep changes focused and avoid reformatting unrelated posts or generated resources.
- For content-only updates, preview the affected page locally.
- For changes to `hugo.toml`, theme references, templates, or assets, run a production build and check the affected pages, taxonomy pages, RSS, and sitemap as applicable.
- Document any new authoring workflow, build requirement, or deployment behavior in this file or `README.md`.
