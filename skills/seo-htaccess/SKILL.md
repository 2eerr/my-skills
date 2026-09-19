---
name: seo-htaccess
description: "Generate, validate, and deploy the .htaccess file for a static Astro site on Hostinger LiteSpeed hosting — trailing-slash canonicals, flat-build URL mapping, compression, cache headers, and error documents. Use when setting up a new Hostinger deployment, auditing server config, fixing 404s or redirect loops, or after build-output changes that affect URL structure."
metadata:
  internal: true
---

# SEO .htaccess (Hostinger LiteSpeed)

The single authoritative server config for a static Astro site deployed to Hostinger's
LiteSpeed hosting. One `.htaccess` file in `public_html/` carries rewrite rules,
compression, and cache headers — no split configs, no extra files. Read the project's
config for domain and build output format — never hardcode.

## When to use it

- Deploying a static Astro site to Hostinger for the first time.
- Auditing or fixing server config (404s, redirect loops, trailing-slash issues).
- After changing Astro's `build.format` or output structure.
- Migrating from Netlify/Cloudflare to Hostinger LiteSpeed.
- **Not for:** canonical/OG/meta tags (see `seo-onpage-seo`), sitemaps/robots (see
  `seo-technical-seo`), or build pipeline (see `astro-ssg`).

## Project config (supply per site, never hardcode)

| Variable | Meaning | Where to read |
|---|---|---|
| `{domain}` | canonical domain | `site.config.js` → `SITE.domain` |
| `{build.format}` | Astro output format | `astro.config.mjs` → `build.format` |
| `{asset_prefix}` | hashed asset path (e.g. `_astro/`) | `astro.config.mjs` or build output |

## What the .htaccess does

1. **Error document** — serves Astro's flat `404.html` for missing URLs.
2. **Trailing-slash canonicals** — redirects `.html` URLs and bare paths to the
   trailing-slash form (the site's canonical URL scheme).
3. **Flat-build mapping** — rewrites trailing-slash requests to the actual `.html`
   files Astro emits (e.g. `/arkansas/` → `arkansas.html`).
4. **Compression** — gzip for HTML, CSS, JS, JSON, SVG, and fonts.
5. **Cache headers** — immutable for hashed assets, 1 week for images/fonts,
   1 hour for HTML (fast deploy propagation).

## The .htaccess file

Copy this to the project root as `.htaccess`; it deploys to `public_html/` with
the build output. Adjust only if the project's build format changes.

```apache
# LiteSpeed / Apache configuration for {Brand} — {domain}
# .htaccess is read by LiteSpeed from the site root (public_html/).
# Astro builds with format: "file" → flat .html files (e.g. arkansas.html).
# These rules make trailing-slash URLs (/arkansas/) serve the flat files.

# Error page (Astro emits a flat dist/404.html)
ErrorDocument 404 /404.html

<IfModule mod_rewrite.c>
  RewriteEngine On

  # --- .html URLs → trailing-slash 301 (canonical URL scheme) ---
  # THE_REQUEST holds the ORIGINAL client request line — immune to internal
  # rewrites, so no redirect loops. /index.html → / and /404.html excluded.
  RewriteCond %{THE_REQUEST} ^[A-Z]+\s/index\.html[\s?] [NC]
  RewriteRule ^ / [R=301,L]

  RewriteCond %{THE_REQUEST} !^[A-Z]+\s/404\.html[\s?] [NC]
  RewriteCond %{THE_REQUEST} ^[A-Z]+\s/([^\s?]+)\.html[\s?] [NC]
  RewriteRule ^ /%1/ [R=301,L]

  # --- No-trailing-slash URLs → trailing-slash 301 (master URL form) ---
  # /path (no slash) → /path/. Real files skipped (!-f), file extensions skipped
  # (so .css/.js/.xml/.txt etc. are not redirected).
  RewriteCond %{REQUEST_FILENAME} !-f
  RewriteCond %{REQUEST_URI} !\.[a-zA-Z0-9]+$
  RewriteRule ^(.*[^/])$ /$1/ [R=301,L]

  # --- Flat build output: map trailing-slash URLs onto .html files ---
  # The build emits page.html (format: "file"), while canonical URLs keep the
  # trailing slash (/path/). These internal rewrites serve the flat files.
  # Uses %{DOCUMENT_ROOT}/$1.html, NOT %{REQUEST_FILENAME}: for directory URLs
  # like /arkansas/ the resolved filename keeps the trailing slash, making
  # the appended ".html" point at arkansas/.html — which never matches.
  RewriteCond %{DOCUMENT_ROOT}/$1.html -f
  RewriteRule ^(.+)/$ $1.html [L]
</IfModule>

# --- Compression ---
<IfModule mod_deflate.c>
  AddOutputFilterByType DEFLATE text/html text/css application/javascript application/json image/svg+xml font/woff2 font/woff
</IfModule>

# --- Cache headers ---
<IfModule mod_headers.c>
  # Hashed build assets under /_astro/ — immutable, cache forever
  <FilesMatch "^_astro/.*\.(js|css)$">
    Header set Cache-Control "public, max-age=31536000, immutable"
  </FilesMatch>

  # Self-hosted images — 1 week cache
  <FilesMatch "\.(webp|png|jpg|jpeg|gif|svg|avif|ico)$">
    Header set Cache-Control "public, max-age=604800, stale-while-revalidate=86400"
  </FilesMatch>

  # Self-hosted fonts — 1 week cache
  <FilesMatch "\.(woff2|woff|ttf|otf)$">
    Header set Cache-Control "public, max-age=604800, stale-while-revalidate=86400"
  </FilesMatch>

  # HTML and everything else — 1 hour so new deploys propagate quickly
  <FilesMatch "\.(html|htm|xml|txt|json)$">
    Header set Cache-Control "public, max-age=3600"
  </FilesMatch>
</IfModule>
```

## Rules

1. **One file only** — no split configs, no extra `.htaccess` files in subdirectories.
2. **Place in project root** — deploys to `public_html/` alongside the `dist/` output.
3. **THE_REQUEST is key** — redirect rules use `%{THE_REQUEST}` (the original client
   request) to avoid loops from internal rewrites.
4. **Never redirect real files** — `!-f` condition skips CSS, JS, images, fonts.
5. **Never redirect file extensions** — the `\.[a-zA-Z0-9]+$` exclusion prevents
   sitemap.xml, robots.txt, etc. from being redirected to trailing-slash.
6. **DOCUMENT_ROOT, not REQUEST_FILENAME** — the flat-build mapping rule uses
   `%{DOCUMENT_ROOT}/$1.html` because `REQUEST_FILENAME` keeps the trailing slash
   for directory-style URLs, causing the `.html` append to miss.
7. **Cache strategy** — hashed assets (`_astro/`) are immutable forever; images/fonts
   get 1 week with stale-while-revalidate; HTML gets 1 hour for fast deploy propagation.

## Deployment checklist

- [ ] `.htaccess` exists in project root (will deploy to `public_html/`).
- [ ] `ErrorDocument 404 /404.html` present and `404.html` exists in `dist/`.
- [ ] Trailing-slash redirect rules present (THE_REQUEST-based, no loops).
- [ ] Flat-build mapping rule present (DOCUMENT_ROOT-based).
- [ ] Compression enabled for HTML, CSS, JS, JSON, SVG, fonts.
- [ ] Cache headers set: immutable for `_astro/`, 1 week for images/fonts, 1 hour for HTML.
- [ ] Test: `curl -I https://{domain}/any-page` returns 200 with correct headers.
- [ ] Test: `curl -I https://{domain}/any-page.html` returns 301 to trailing-slash.
- [ ] Test: `curl -I https://{domain}/any-page` (no slash) returns 301 to trailing-slash.

## Commands

```bash
# Validate .htaccess syntax (if mod_rewrite_test available)
apachectl configtest 2>&1 | grep -v "Could not"

# Test redirect rules locally (requires Apache/LiteSpeed)
curl -I http://localhost/arkansas.html    # should 301 to /arkansas/
curl -I http://localhost/arkansas         # should 301 to /arkansas/
curl -I http://localhost/arkansas/        # should 200 (serve arkansas.html)

# Deploy to Hostinger
npm run build                            # generates dist/
# Upload dist/ contents + .htaccess to public_html/
```

## Notes & gotchas

- **Hostinger uses LiteSpeed**, not Apache — but `.htaccess` syntax is compatible.
  LiteSpeed reads the same rewrite rules and modules.
- **`build: { format: 'file' }`** in `astro.config.mjs` produces flat `.html` files
  (e.g. `arkansas.html`) rather than directory-style (`arkansas/index.html`). The
  `.htaccess` bridges the gap between canonical trailing-slash URLs and flat output.
- **Redirect loops** usually mean a rule is matching the rewritten URL instead of
  `THE_REQUEST`. Always use `%{THE_REQUEST}` for external redirects.
- **Missing trailing slash** → 301 to add it. This is the canonical URL scheme owned
  by `seo-technical-seo` — the `.htaccess` enforces it at the server level.
- **Cache-Control immutable** means the browser will never revalidate the hashed asset.
  This is safe because Astro's hashing changes the filename on every build.
- **stale-while-revalidate** on images/fonts serves the cached version immediately
  while fetching the new one in the background — good for fast deploys without
  breaking the cache.
