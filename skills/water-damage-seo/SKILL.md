---
name: water-damage-seo
description: Apply the Water Damage Pros US technical & on-page SEO spec — trailing-slash URL architecture, title/meta patterns, canonicals, heading rules, the JSON-LD schema matrix (WebSite/Organization/WebPage/LocalBusiness/Service/FAQPage/BreadcrumbList/ItemList), Open Graph & Twitter cards, the sitemap-index with per-state sitemaps, robots, Core Web Vitals, accessibility, local SEO & NAP, check-seo.js enforcement, and the pre-publish checklist. Use when building or auditing any page's SEO.
---

# Water Damage SEO Playbook

The enforcement spec that `scripts/check-seo.js` and the templates follow. Apply to every
generated and hand-written page.

## 1. URL structure (canonical architecture)

- Lowercase, hyphenated slugs; no underscores, no query params, no trailing `index`.
- **Trailing-slash scheme everywhere** (except bare root `/`): sitemap URLs, canonical,
  `og:url`, JSON-LD URLs, and every internal `href`. Never mix slash/no-slash.
- Patterns: `/`, `/locations/`, `/services/`, `/services/{service}/`, `/{state}/`,
  `/{state}/{city}/`, `/{state}/{city}/{service}/` (no `/water-damage` prefix — single niche),
  plus `/about/ /contact/ /faq/ /privacy/ /terms/`.
- One URL per page — no www/non-www or slash variants (301 the other).
- Use the `paths` helpers (`src/lib/data.js`) — never hardcode a bare no-slash path.

## 2–3. Titles & meta descriptions

- Money: `{Service} in {City}, {State} | Water Damage Pros US` (≤60 chars, keyword front-loaded).
- City: `Water Damage Restoration in {City}, {State} | ...`; State: `... in {State} | ...`.
- Unique per page (script-enforced). Meta 140–160 chars: keyword + locale + one differentiator
  + soft CTA; written for clicks, not a keyword dump.

## 4–5. Canonical & headings

- Absolute canonical `https://{domain}` + trailing-slash path on every page; reuse the same
  value for `og:url` and `WebPage.url`.
- Exactly one H1 matching primary keyword intent; H2 = sections, H3/H4 = sub-sections; no
  heading jumps (H1→H3).

## 6. Non-thin content

Money 1,000–1,500 words; city/state 1,000–1,400; no lorem/placeholder/empty blocks. Write
for humans first (real local facts), then optimize.

## 8. Internal linking

- City page → state page + homepage + ≥8 related city-service pages.
- Money page → ≥6 related services in same city + city page + state page + one nearby city.
- Breadcrumb on every location page: `Home › {State} › {City} › {Service}`.
- Descriptive anchor text; no orphan pages (reachable ≤3 clicks from home).

## 9. Structured data (JSON-LD)

Base graph ships **WebSite + Organization + WebPage** on every page; each page adds nodes.
Every `BreadcrumbList` has an `@id` (`…/#breadcrumb`) referenced by the page's `WebPage`.

| Page | Schema |
|---|---|
| Homepage | LocalBusiness + WebSite + WebPage (+ FAQPage) |
| State | WebPage + BreadcrumbList (+ Service + FAQPage) |
| City | WebPage + BreadcrumbList + Service (+ FAQPage) |
| Service hub | Service + WebPage + BreadcrumbList (+ FAQPage) |
| Money | Service + WebPage + BreadcrumbList (+ LocalBusiness + FAQPage) |
| About / Contact | AboutPage / ContactPage + Organization (mainEntity) |
| FAQ | WebPage + BreadcrumbList (+ FAQPage) |
| Locations | WebPage + BreadcrumbList + ItemList + FAQPage |
| Services index | WebPage + ItemList + FAQPage |

- Local business `@type` = **`LocalBusiness`**, id `https://waterdamageprosus.com/#localbusiness`.
- **FAQPage only from a page's own `faqs:` frontmatter** (≥1 Q&A) and must match visible text
  exactly — never from templated/fallback FAQs.
- **Never fabricate** `AggregateRating`/reviews. Consistent IDs: `#website`, `#organization`,
  `#localbusiness`, `#webpage`, `#breadcrumb`. Validate post-build with `check-seo.js`.

## 10. Open Graph & Twitter

`og:locale=en_US`, `og:type`, `og:site_name/title/description/url/image` (1200×630) +
`og:image:width/height/alt`; `twitter:card=summary_large_image` + title/description/image +
`twitter:image:alt`. Set globally; pages override title/description only.

## 11. Technical SEO

- **Sitemap:** `sitemap-index.xml` (post-build `scripts/generate-sitemaps.js`) + `sitemap-core.xml`
  + 51 per-state files (`01`–`51`, alphabetical). **Only published pages** appear; empty state
  sitemaps exist on disk but are **excluded from the index**. `<lastmod>` = content file mtime.
  Each child ≤50,000 URLs / ≤50MB. `npm run build` runs it automatically.
- **Robots:** allow all, reference sitemap. `<meta name="robots">` =
  `index, follow, max-snippet:-1, max-image-preview:large, max-video-preview:-1`.
  `referrer` = `strict-origin-when-cross-origin`.
- **Performance:** LCP<2.5s, INP<200ms, CLS<0.1. Self-hosted woff2 fonts preloaded with
  `fetchpriority="high"`; CSS link gets `fetchpriority` via `scripts/add-fetchpriority.js`;
  `size-adjust`/`ascent-override`/`descent-override` for zero CLS; images declare width/height.
- **Accessibility:** semantic landmarks, one h1, `lang="en"`, `aria-label` on icon-only,
  skip-to-content, visible `:focus-visible`, contrast ≥4.5:1.
- **Indexability:** clean 200s, real 404 status, no duplicate titles/descriptions, 301 domain
  variants to canonical, HTTPS + CDN caching.
- **Analytics:** Google tag `G-CDTP0G75BP` in `SITE.analytics`, injected only in production.

## 12–15. Link equity, local SEO, NAP

- Money pages get links from city page + 2–3 sibling services + state grid; keep link
  distance to money pages ≤3 clicks; no orphan clusters.
- Local on-page signals: keyword+locale in title/meta/H1/one H2/one FAQ; local entities;
  localized FAQ; local internal links; consistent NAP.
- Post-launch: one GBP per real location, citation-consistent NAP, genuine reviews only.
- NAP consistent across site, footer, schema, GBP.

## 16–17. Enforcement & pre-publish checklist

`node scripts/check-seo.js` fails on: missing/duplicate title or meta, missing/relative
canonical, missing og/twitter, missing/invalid JSON-LD, orphan pages, under word count,
empty blocks.

Pre-publish (every page): unique title ≤60 · meta 140–160 · absolute canonical matching
sitemap · one H1 + logical hierarchy · valid JSON-LD matching content · OG+Twitter present ·
internal links per spec (≥8 city / ≥6 service) · images sized+alt+lazy · word count in range ·
local entities present · `check-seo.js` passes · Lighthouse ≥90 perf / ≥95 SEO.
