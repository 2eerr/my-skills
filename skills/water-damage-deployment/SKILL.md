---
name: water-damage-deployment
description: "Build-and-publish workflow for Water Damage Pros US — the content gate (unique: true) that makes `npm run build` equal publishing, the delivery-gate validation scripts (validate-data, check-seo, check-wordcounts), sitemap generation, the Hostinger static upload to waterdamageprosus.com, LiteSpeed cache headers, and the source-zip packaging. Use when shipping newly-written content or deploying the site."
---

# Water Damage Deployment

Static Astro build: `npm run build` renders everything into `dist/`, and that folder **is**
the entire site. Deployed as a static file upload to **Hostinger** at
`https://waterdamageprosus.com`.

## Build = publish (content gate)

`dist/` only ever contains pages whose content is **verified unique** (`unique: true` in
`src/content/**` frontmatter). Whole states ship only when fully complete; cities ship
per-city; hubs per-service; in-progress pages simply aren't built. There is no separate
publish switch — **`npm run build` IS publishing**; uploading `dist/` deploys exactly what has
unique content.

- Gate source of truth: `src/lib/page-content.js` → `hasPageContent()`.
- Regex `/^unique:\strue\s*$/m` is **line-ending-agnostic** (CRLF + LF). Do not change the
  trailing `\s*` to `\s` or a literal space — that broke on Linux LF checkouts. Test both if
  you edit it.
- `SITE.domain` in `site.config.js` = `https://waterdamageprosus.com`; canonicals, sitemap,
  robots, and OG all reference it. If the domain changes, update `SITE.domain` (+ the
  `public/robots.txt` sitemap line) and rebuild before deploying.

## Delivery gate (run before every upload)

```bash
npm install        # once
npm run build      # renders dist/ + regenerates sitemaps (post-build)
npm run validate   # data sanity — 0 errors
npm run check:seo  # 0 issues expected (page count = unique-content pages)
```

(`check-wordcounts.js` enforces per-type word ranges; `gen-pages-tracker.js` re-derives the
tracker.) Sitemaps are generated post-build by `scripts/generate-sitemaps.js`
(`npm run gen:sitemaps` regenerates from existing `dist/` without a full build).

## Deploy to Hostinger

1. **Build** locally and pass the delivery gate above (never upload a stale `dist/`).
2. **Upload `dist/` contents** (not the folder itself) into `public_html/` via hPanel
   File Manager or FTP/SFTP — delete old contents first (back up if needed).
   - SSH alternative: `rsync -avz dist/ user@your-host:public_html/`
3. **Domain:** point `waterdamageprosus.com` at Hostinger nameservers, document root =
   `public_html/`, enable free Let's Encrypt HTTPS.
4. **Redeploy after changes:** rebuild → re-upload → clear Hostinger CDN/cache if used.

## Cache headers (LiteSpeed)

`public/.htaccess` (copied into `dist/` at build) sets, for Hostinger's LiteSpeed:
- gzip compression (`mod_deflate`) for HTML/CSS/JS/JSON/SVG/woff/woff2;
- `mod_expires`: hashed `/_astro/*` immutable 1 year, images/fonts 1 week, HTML 1 hour;
- `ErrorDocument 404 /404/`.

The old Netlify `public/_headers` convention is **not** read by LiteSpeed — its rules are
folded into `.htaccess`.

## Source zip

`npm run zip` → `deploy.zip` packages source files (`scripts/package-deploy.js`) for remote
builds/backup.

## Astro output note

Astro emits `index.html` per route, so web servers serve both `/alaska` and `/alaska/` with
no redirect rules. Keep `cssCodeSplit: false` in `astro.config.mjs`.

## Netlify (ARCHIVED — do not follow)

`docs/DEPLOYMENT.md` keeps a Netlify archive (`netlify.toml`, `_redirects`, `_headers`, drag-&-drop
/ git deploy) **for reference only** in case hosting switches back. The current workflow is
Hostinger static upload — do not implement the Netlify section.
