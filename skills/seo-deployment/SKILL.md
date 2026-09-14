---
name: seo-deployment
description: "Build-and-publish workflow for a static local-SEO lead-gen site (any niche) — the content gate (unique: true) that makes build= publishing, delivery-gate validation scripts, sitemap generation, static-host upload, cache headers, and source-zip packaging. Use when shipping content or deploying a static SSG site."
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
npm run build      # prebuild check → Astro build → fetchpriority → sitemaps
npm run validate   # data sanity — 0 errors
npm run check:seo  # 0 issues expected (page count = unique-content pages)
npm run check:wordcounts  # every page within its type's word-count range
```

### Build pipeline scripts

| Script | Location | Runs when | Purpose |
|---|---|---|---|
| `prebuild-check.js` | `source/scripts/` | `prebuild` hook (before Astro) | Clears stale content cache, logs content counts, verifies `unique: true` on all files |
| `build.js` | `source/scripts/` | `npm run build:states` | Scoped build — sets `BUILD_STATES` env var for `getStaticPaths()` filtering |
| `add-fetchpriority.js` | `source/scripts/` | Post-build (after Astro) | Adds `fetchpriority="high"` to stylesheet `<link>` tags in `dist/` HTML |
| `generate-sitemaps.js` | `source/scripts/` | Post-build (after Astro) | Walks `dist/`, produces `sitemap-index.xml` + per-region child sitemaps; uses source file `mtime` for `<lastmod>` |
| `validate-data.js` | `source/scripts/` | `npm run validate` | Validates `states.json`, `cities.json`, `services.json` — required fields, population ≥1K, unique slugs |
| `check-seo.js` | `source/scripts/` | `npm run check:seo` | Scans `dist/**/*.html` for missing title/meta/canonical/JSON-LD/OG tags |
| `check-wordcounts.js` | `source/scripts/` | `npm run check:wordcounts` | Verifies each page's word count falls within its type's range |
| `gen-pages-tracker.js` | `source/scripts/` | `npm run gen:tracker` | Regenerates `TRACKER.md` from content folder |
| `package-deploy.js` | `source/scripts/` | `npm run zip` | Builds `deploy.zip` source archive; its `required` list must include every script the build uses |

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
   - **Zip upload path (Hostinger-style hPanel):** upload the deploy zip via the file manager,
     extract at the web root. If the host serves a Node app instead of pure static files, point
     the app entry at a tiny static-server script shipped in the zip (e.g. `server.js`).
3. **Domain:** point DNS at the host, set the web root, enable HTTPS (e.g. Let's Encrypt).
4. **Redeploy after changes:** rebuild → re-upload → clear the host CDN/cache if used.

## Cache headers

Ship a **single** server config file for the host type — one `.htaccess` carries cache
headers **and** HTTPS/www redirects (splitting them into several config files wastes inodes on
hosts that count them). For Apache/LiteSpeed (`.htaccess`): gzip (`mod_deflate`) for
HTML/CSS/JS/JSON/SVG/woff/woff2; `mod_expires` — hashed asset bundles immutable 1 year,
images/fonts ~1 week–1 year, HTML ~1 hour; custom `ErrorDocument 404`. For Netlify use
`_headers`/`_redirects`; for Cloudflare/S3 set cache rules at the edge.

## Source zip — explicit allowlist (inode discipline)

A `zip`-style task packages files for remote builds/backup/upload. Build the archive from an
**explicit allowlist**, never "zip everything except…":

- **Include:** the built output (`dist/`), the runtime entry (e.g. `server.js`),
  `package.json`, the server config (`.htaccess`), and any build-critical config
  (`astro.config.mjs`, `site.config.js`) or script the remote build needs.
- **Exclude:** `src/` (when shipping prebuilt output), `node_modules/`, `temp/`,
  `docs/`, `.git/`, caches — every excluded dir saves thousands of inodes on the host.
- **Sync rule:** any script added to the build pipeline must be added to the packaging
  script's required list in the same change, or remote builds break silently.
- Ad-hoc/throwaway scripts live in a `temp/` dir that is tracked in git but **never**
  packaged (see the SSG skill's scaffold rules).

## SSG output note

Framework mechanics (Astro route output, `cssCodeSplit: false`, `public/` copy, post-build
passes) are owned by the **`astro-ssg`** skill — apply them there. This skill owns the
build-as-publish gate and the deploy workflow.
