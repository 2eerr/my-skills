---
name: seo-technical-seo
description: "Technical SEO for a local-SEO lead-gen site (any niche) — URL architecture (trailing-slash scheme), sitemaps, robots, Core Web Vitals, indexability, analytics, and the SEO check script. Use when building routes, auditing site-level technical SEO, or debugging sitemaps/robots/canonicals. Titles/meta: on-page skill; JSON-LD: schema skill; NAP: local skill."
metadata:
  internal: true
---

# Technical SEO

Site-level technical rules the build and the SEO check script enforce. Siblings own the rest:
`seo-onpage-seo` (titles, metas, canonicals, headings, links, OG),
`seo-schema` (JSON-LD), `seo-local-seo` (GBP/NAP),
`seo-image-seo` (images), `seo-design-system` (component a11y).

## 1. URL structure (canonical architecture — single source of truth)

- Lowercase, hyphenated slugs; no underscores, no query params, no trailing `index`.
- **Trailing-slash scheme everywhere** (except bare root `/`): sitemap URLs, canonicals,
  `og:url`, JSON-LD URLs, and every internal `href`. Never mix slash/no-slash. Other skills
  reference this scheme — it is defined only here.
- Patterns: `/`, `/locations/`, `/services/`, `/services/{service}/`, `/{region}/`,
  `/{region}/{location}/`, `/{region}/{location}/{service}/` (no niche prefix on a single-niche
  site), plus `/about/ /contact/ /faq/ /privacy/ /terms/`.
- One URL per page — no www/non-www or slash variants (301 the other).
- Use the project's URL/path helpers — never hardcode a bare no-slash path.

## 2. Sitemaps

- A `sitemap-index.xml` (generated post-build) + a core sitemap + one per-region sitemap
  (numbered, alphabetical by region slug).
- **Only published pages appear** (the content gate decides what exists in the build output);
  empty region sitemaps stay on disk but are **excluded from the index** (avoids empty-sitemap
  warnings). A region's sitemap auto-populates when its content ships.
- `<lastmod>` = the content file's actual mtime (auto-synced; never hand-edited). Omit
  `changefreq`/`priority` — Google ignores them.
- Each child ≤50,000 URLs / ≤50MB (sitemaps.org protocol); URLs sorted for deterministic output.
- The build runs the generator automatically (see `astro-ssg` for the sitemap-only regen task).
- Total sitemap URL count must equal the rendered page count exactly (script-compared).

## 3. Robots & meta-robots

- `robots.txt`: allow all, reference `sitemap-index.xml` at the site root.
- `<meta name="robots">` on every page: `index, follow, max-snippet:-1,
  max-image-preview:large, max-video-preview:-1`. Never `noindex` a live page.
- `referrer` meta = `strict-origin-when-cross-origin`.

## 4. Performance (Core Web Vitals)

- Targets: LCP < 2.5s, INP < 200ms, CLS < 0.1 (aim CLS ≈ 0).
- Font loading/preloading and zero-CLS fallback metrics follow the **design-system** skill's
  typography rules; the Astro wiring + post-build `fetchpriority` pass live in **`astro-ssg`**.
- Images declare width/height and lazy-load per the **image-SEO** skill. Minify HTML/CSS/JS;
  inline critical CSS; no render-blocking third-party scripts above the fold.

## 5. Indexability & crawling

- Clean 200s everywhere; the 404 page returns a real 404 status (no soft-404s).
- No duplicate titles/descriptions site-wide (script-enforced).
- 301 all domain variants (www, staging) to the canonical domain; canonicals, sitemap, and OG
  all reference it from the site config. HTTPS + CDN caching (immutable hashed assets, ~1h HTML).
- Favicon + apple-touch-icon + `application-name` referenced in head.

## 6. Accessibility (site-level)

- Semantic landmarks, one `h1`, `lang`, skip-to-content, visible `:focus-visible`, contrast
  ≥4.5:1. Component-level a11y and the WCAG checklist are owned by the design-system skill.

## 7. Analytics & verification

- Inject the configured measurement ID **only in production builds** (never on localhost);
  config-driven, so it can be changed/disabled in one place. Search Console verification tag
  from config.

## 8. Enforcement (the SEO check script)

Runs on built HTML before every delivery gate; fails on: missing/duplicate title or meta,
missing/relative canonical, missing og/twitter, missing/invalid JSON-LD, orphan pages (no
internal link in), pages under word count, empty blocks.

**Script:** `source/scripts/check-seo.js` — scans `dist/**/*.html` for missing title, meta
description, canonical, JSON-LD, and OG tags. Outputs per-tag missing counts and total
page count.

**Sitemap script:** `source/scripts/generate-sitemaps.js` — walks `dist/`, produces
`sitemap-index.xml` + per-region child sitemaps. Uses source file `mtime` for
`<lastmod>`. Omits `changefreq`/`priority`.

## Checklist (done when)

- [ ] Every route follows the URL patterns + trailing-slash scheme; no variant URLs.
- [ ] Sitemap index lists only non-empty children; URL count = rendered page count.
- [ ] robots.txt + meta-robots per spec.
- [ ] CWV targets met (Lighthouse ≥90 perf / ≥95 SEO on samples).
- [ ] Real 404s, no duplicate titles/descriptions, domain variants 301'd.
- [ ] Analytics production-only.
- [ ] SEO check script: 0 issues.
