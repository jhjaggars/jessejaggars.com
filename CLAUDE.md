# jessejaggars.com - Site Documentation

This is a Hugo-based static site blog hosted on Cloudflare Pages.

## Building the Site

Build the site using Hugo:

```bash
hugo
```

This generates static files in the `public/` directory.

For development with live reload:

```bash
hugo server -D
```

Then visit http://localhost:1313/

## Theme Management

This site uses the **Ed theme** (gohugo-theme-ed) with custom patches for Hugo v0.155+ compatibility.

### Theme Strategy: Vendored Files

- Themes are **vendored** (copied directly into the repository), not managed as git submodules
- This approach simplifies deployment and allows us to maintain local patches
- Theme files are located in `themes/gohugo-theme-ed/`

### Applied Patches

The Ed theme has been patched to work with Hugo v0.155.1:
- Updated `themes/gohugo-theme-ed/layouts/partials/styles.html` to use `css.Sass` instead of deprecated `resources.ToCSS`

### Hugo Modules

The site uses Hugo modules for theme dependencies (lunr.js search functionality):
- `go.mod` and `go.sum` manage module dependencies
- The theme is replaced with the local vendored version via `go.mod` replace directive:
  ```toml
  replace github.com/sergeyklay/gohugo-theme-ed => ./themes/gohugo-theme-ed
  ```

## Deployment

### Cloudflare Pages Hosting

The site is hosted on **Cloudflare Pages** and serves static files from the `public/` directory.

### Deployment Workflow

1. **Build the site locally:**
   ```bash
   hugo
   ```

2. **Commit changes including the `public/` directory:**
   ```bash
   git add public/
   git commit -m "rebuild site with updates"
   ```

3. **Push to GitHub:**
   ```bash
   git push
   ```

4. **Cloudflare automatically deploys** the updated `public/` directory

### Important Notes

- **Always commit the `public/` directory** - Cloudflare serves these pre-built files
- Cloudflare does not build the site; it only serves the static files from `public/`
- The `public/` directory contains generated HTML, CSS, and JavaScript
- Changes to content or theme require rebuilding and committing the `public/` directory

## Repository Structure

```
jessejaggars.com/
├── content/              # Markdown content files
│   ├── post/            # Blog posts
│   └── search.md        # Search page
├── themes/              # Vendored themes
│   └── gohugo-theme-ed/ # Ed theme (vendored with patches)
├── public/              # Generated static site (committed to git)
├── config.toml          # Hugo configuration
├── go.mod               # Hugo module dependencies
└── CLAUDE.md            # This file
```

## Adding Themes

When trying new themes:

1. Clone the theme into `themes/`:
   ```bash
   git clone --depth=1 https://github.com/user/theme-name themes/theme-name
   rm -rf themes/theme-name/.git
   ```

2. Update `config.toml` to use the theme:
   ```toml
   theme = "theme-name"
   ```

3. Test locally with `hugo server -D`

4. If keeping the theme, vendor it (no git submodules) and commit it directly

## Content Management

- Blog posts go in `content/post/`
- Use Hugo front matter for metadata (title, date, tags, etc.)
- Build and commit `public/` after adding new content
