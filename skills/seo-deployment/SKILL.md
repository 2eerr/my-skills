---
name: seo-deployment
description: "Build-and-publish workflow for a local-SEO lead-gen site (any niche) built as a static site — the content gate (unique: true) that makes the build equal publishing, the delivery-gate validation scripts (data, SEO, word-count checks), sitemap generation, static-host upload, cache headers, and source-zip packaging. Use when shipping newly-written content or deploying a static Astro/SSG site."
metadata:
  internal: true
---

# SEO Deployment (static build)

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

(A word-count check enforces per-type ranges; a mobile check loads key pages in headless
Chrome at mobile widths and fails on horizontal overflow; the tracker generator re-derives
statuses. Sitemap mechanics + regeneration tasks: `seo-technical-seo` / `astro-ssg`.)

Also before calling a phase **done**: 5 random pages reviewed for non-thin content + valid
links + the markdown/UI element floor, an accessibility pass (Lighthouse/axe 0 errors, focus
rings, keyboard menus, contrast), and the **deploy-zip sync rule** — any script added to the
build must also be listed in the packaging script's required files, so `dist/` and the source
zip never drift.

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

Framework mechanics (Astro route output, `cssCodeSplit: false`, `public/` copy, post-build
passes) are owned by the **`astro-ssg`** skill — apply them there. This skill owns the
build-as-publish gate and the deploy workflow.
