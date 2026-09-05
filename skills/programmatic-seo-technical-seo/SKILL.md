---
name: programmatic-seo-technical-seo
description: Apply a technical & on-page SEO spec for a programmatic local-SEO lead-gen site (any niche) — trailing-slash URL architecture, title/meta patterns, canonicals, heading rules, the JSON-LD schema matrix (WebSite/Organization/WebPage/LocalBusiness/Service/FAQPage/BreadcrumbList/ItemList), Open Graph & Twitter cards, a sitemap-index with per-region sitemaps, robots, Core Web Vitals, accessibility, local SEO & NAP, an SEO check script, and a pre-publish checklist. Use when building or auditing any page's SEO.
metadata:
  internal: true
---

# Programmatic SEO — Technical & On-Page SEO

The enforcement spec that the SEO check script and templates follow. Apply to every generated
and hand-written page. Use the project's configured brand + domain (never hardcode).

## 1. URL structure (canonical architecture)

- Lowercase, hyphenated slugs; no underscores, no query params, no trailing `index`.
- **Trailing-slash scheme everywhere** (except bare root `/`): sitemap URLs, canonical,
  `og:url`, JSON-LD URLs, and every internal `href`. Never mix slash/no-slash.
- Patterns: `/`, `/locations/`, `/services/`, `/services/{service}/`, `/{region}/`,
  `/{region}/{location}/`, `/{region}/{location}/{service}/` (no niche prefix on a single-niche
  site), plus `/about/ /contact/ /faq/ /privacy/ /terms/`.
- One URL per page — no www/non-www or slash variants (301 the other).
- Use the project's URL/path helpers — never hardcode a bare no-slash path.

## 2–3. Titles & meta descriptions

- Money: `{Service} in {Location}, {Region} | {Brand}` (≤60 chars, keyword front-loaded).
- Location: `{Primary service} in {Location}, {Region} | ...`; Region: `... in {Region} | ...`.
- Unique per page (script-enforced). Meta 140–160 chars: keyword + locale + one differentiator
  + soft CTA; written for clicks, not a keyword dump.

## 4–5. Canonical & headings

- Absolute canonical `https://{domain}` + trailing-slash path on every page; reuse the same
  value for `og:url` and `WebPage.url`.
- Exactly one H1 matching primary keyword intent; H2 = sections, H3/H4 = sub-sections; no
  heading jumps (H1→H3).

## 6. Non-thin content

Money 1,000–1,500 words; location/region 1,000–1,400; no lorem/placeholder/empty blocks.
Write for humans first (real local facts), then optimize.

## 8. Internal linking

- Location page → region page + homepage + ≥8 related location-service pages.
- Money page → ≥6 related services in same location + location page + region page + one nearby
  location.
- Breadcrumb on every location page: `Home › {Region} › {Location} › {Service}`.
- Descriptive anchor text; no orphan pages (reachable ≤3 clicks from home).

## 9. Structured data (JSON-LD)

Base graph ships **WebSite + Organization + WebPage** on every page; each page adds nodes.
Every `BreadcrumbList` has an `@id` (`…/#breadcrumb`) referenced by the page's `WebPage`.

| Page | Schema |
|---|---|
| Homepage | LocalBusiness + WebSite + WebPage (+ FAQPage) |
| Region | WebPage + BreadcrumbList (+ Service + FAQPage) |
| Location | WebPage + BreadcrumbList + Service (+ FAQPage) |
| Service hub | Service + WebPage + BreadcrumbList (+ FAQPage) |
| Money | Service + WebPage + BreadcrumbList (+ LocalBusiness + FAQPage) |
| About / Contact | AboutPage / ContactPage + Organization (mainEntity) |
| FAQ | WebPage + BreadcrumbList (+ FAQPage) |
| Locations index | WebPage + BreadcrumbList + ItemList + FAQPage |
| Services index | WebPage + ItemList + FAQPage |

- Local business `@type` = **`LocalBusiness`** (or a more specific `HomeAndConstructionBusiness`
  subtype when one fits the trade), id `https://{domain}/#localbusiness`.
- **FAQPage only from a page's own `faqs:` frontmatter** (≥1 Q&A) and must match visible text
  exactly — never from templated/fallback FAQs.
- **Never fabricate** `AggregateRating`/reviews. Consistent IDs: `#website`, `#organization`,
  `#localbusiness`, `#webpage`, `#breadcrumb`. Validate post-build with the SEO check script.

## 10. Open Graph & Twitter

`og:locale`, `og:type`, `og:site_name/title/description/url/image` (1200×630) +
`og:image:width/height/alt`; `twitter:card=summary_large_image` + title/description/image +
`twitter:image:alt`. Set globally; pages override title/description only.

## 11. Technical SEO

- **Sitemap:** a `sitemap-index.xml` (generated post-build) + a core sitemap + one per-region
  sitemap (numbered, alphabetical). **Only published pages** appear; empty region sitemaps
  exist on disk but are **excluded from the index**. `<lastmod>` = content file mtime. Each
  child ≤50,000 URLs / ≤50MB. The build runs the generator automatically.
- **Robots:** allow all, reference the sitemap. `<meta name="robots">` =
  `index, follow, max-snippet:-1, max-image-preview:large, max-video-preview:-1`.
  `referrer` = `strict-origin-when-cross-origin`.
- **Performance:** LCP<2.5s, INP<200ms, CLS<0.1. Self-hosted woff2 fonts preloaded with
  `fetchpriority="high"`; give the bundled CSS link `fetchpriority="high"`; use
  `size-adjust`/`ascent-override`/`descent-override` for zero CLS; images declare width/height.
- **Accessibility:** semantic landmarks, one h1, `lang`, `aria-label` on icon-only,
  skip-to-content, visible `:focus-visible`, contrast ≥4.5:1.
- **Indexability:** clean 200s, real 404 status, no duplicate titles/descriptions, 301 domain
  variants to canonical, HTTPS + CDN caching.
- **Analytics:** inject the configured measurement ID only in production builds.

## 12–15. Link equity, local SEO, NAP

- Money pages get links from location page + 2–3 sibling services + region grid; keep link
  distance to money pages ≤3 clicks; no orphan clusters.
- Local on-page signals: keyword+locale in title/meta/H1/one H2/one FAQ; local entities;
  localized FAQ; local internal links; consistent NAP.
- Post-launch: one Google Business Profile per real location, citation-consistent NAP, genuine
  reviews only.
- NAP consistent across site, footer, schema, GBP.

## 16–17. Enforcement & pre-publish checklist

The SEO check script scans built HTML and fails on: missing/duplicate title or meta,
missing/relative canonical, missing og/twitter, missing/invalid JSON-LD, orphan pages, under
word count, empty blocks.

Pre-publish (every page): unique title ≤60 · meta 140–160 · absolute canonical matching
sitemap · one H1 + logical hierarchy · valid JSON-LD matching content · OG+Twitter present ·
internal links per spec (≥8 location / ≥6 service) · images sized+alt+lazy · word count in
range · local entities present · SEO check passes · Lighthouse ≥90 perf / ≥95 SEO.
