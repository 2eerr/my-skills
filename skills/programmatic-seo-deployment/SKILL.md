---
name: programmatic-seo-deployment
description: "Build-and-publish workflow for a programmatic local-SEO lead-gen site (any niche) built as a static site — the content gate (unique: true) that makes the build equal publishing, the delivery-gate validation scripts (data, SEO, word-count checks), sitemap generation, static-host upload, cache headers, and source-zip packaging. Use when shipping newly-written content or deploying a static Astro/SSG site."
---

# Programmatic SEO Deployment (static build)

For a static-site-generator build (e.g. Astro): the build renders everything into `dist/`, and
that folder **is** the entire site. Deploy as a static upload to any host (Hostinger, Netlify,
Cloudflare Pages, S3, etc.) at the project's configured domain.

## Build = publish (content gate)

`dist/` only ever contains pages whose content is **verified unique** (`unique: true` in the
content frontmatter). Whole regions ship only when fully complete; locations ship per-location;
hubs per-service; in-progress pages simply aren't built. There is no separate publish switch —
**the build IS publishing**; uploading `dist/` deploys exactly what has unique content.

- Gate source of truth: a single `hasPageContent()`-style helper reading the content folder.
- The gate regex `/^unique:\strue\s*$/m` must be **line-ending-agnostic** (CRLF + LF). Do not
  tighten `\s*` to `\s` or a literal space — that breaks on LF checkouts (Linux/CI). Test both
  line endings if you edit it.
- The configured domain (e.g. `SITE.domain`) drives canonicals, sitemap, robots, and OG. If the
  domain changes, update it (+ the robots sitemap line) and rebuild before deploying.

## Delivery gate (run before every upload)

```bash
npm install        # once
npm run build      # renders dist/ + regenerates sitemaps (post-build)
npm run validate   # data sanity — 0 errors
npm run check:seo  # 0 issues expected (page count = unique-content pages)
```

(A word-count check enforces per-type ranges; the tracker generator re-derives statuses.)
Sitemaps are generated post-build; a `gen:sitemaps`-style task regenerates them from existing
`dist/` without a full build.

## Deploy to a static host

1. **Build** locally and pass the delivery gate (never upload a stale `dist/`).
2. **Upload `dist/` contents** (not the folder itself) into the web root (`public_html/`,
   `dist/`, etc.) via the host's file manager or FTP/SFTP/SSH — delete old contents first.
   - SSH alternative: `rsync -avz dist/ user@host:webroot/`
3. **Domain:** point DNS at the host, set the web root, enable HTTPS (e.g. Let's Encrypt).
4. **Redeploy after changes:** rebuild → re-upload → clear the host CDN/cache if used.

## Cache headers

Ship a server config for the host type. For Apache/LiteSpeed (`.htaccess`): gzip
(`mod_deflate`) for HTML/CSS/JS/JSON/SVG/woff/woff2; `mod_expires` — hashed asset bundles
immutable 1 year, images/fonts ~1 week, HTML ~1 hour; custom `ErrorDocument 404`. For Netlify
use `_headers`/`_redirects`; for Cloudflare/S3 set cache rules at the edge.

## Source zip

A `zip`-style task can package source files (via a small script) for remote builds/backup.

## SSG output note

Astro emits `index.html` per route, so web servers serve both `/foo` and `/foo/` with no
redirect rules. Keep CSS code-splitting off (`cssCodeSplit: false`) if the project relies on a
single shared bundle.
