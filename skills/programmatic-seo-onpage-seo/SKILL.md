---
name: programmatic-seo-onpage-seo
description: "On-page SEO spec for a programmatic local-SEO lead-gen site (any service niche) — title/meta patterns, canonical and heading rules, the non-thin gate, internal-linking and link-equity rules for the hub-and-spoke, and Open Graph & Twitter card wiring. Use when writing or auditing a page's own HTML head, headings, links, and social cards."
metadata:
  internal: true
---

# Programmatic SEO — On-Page SEO

Per-page on-page rules for a programmatic local-SEO site (any niche). Apply to every generated
and hand-written page. Read the project's config for brand + domain — never hardcode. URL
architecture and the canonical trailing-slash scheme are owned by `programmatic-seo-technical-seo`;
JSON-LD by `programmatic-seo-schema`; copy quality by `programmatic-seo-content-writing`.

## 1. Titles

- Money: `{Service} in {Location}, {Region} | {Brand}` (≤60 chars, keyword front-loaded).
- Location: `{Primary service} in {Location}, {Region} | {Brand}`; Region: `… in {Region} | {Brand}`.
- **Unique per page** (script-enforced); one primary keyword; no stuffing or punctuation noise.

## 2. Meta descriptions

- **140–160 chars**, unique per page.
- Formula: primary keyword + locale + one differentiator (e.g. "same-day service", "free
  estimates", "local crews") + soft CTA.
- Written for humans to click, not a keyword dump. Generated from a layout template,
  overridable per page.

## 3. Canonical

- Absolute `https://{domain}` + the site's canonical URL scheme (see the technical skill) on
  **every** page; reuse the exact same value for `og:url` and the `WebPage.url` schema node.
- No self-referencing inconsistencies — the SEO check script cross-checks sitemap vs canonical.

## 4. Headings (H1–H4)

- Exactly **one H1**, matching the primary keyword intent; human copy, not stuffed.
- H2 = main sections, H3/H4 = sub-sections; **no heading jumps** (H1→H3 without H2).
- Secondary keywords appear naturally in H2s (e.g. `{Service} Costs in {Location}`).

## 5. Non-thin content gate

- Word counts per page type are owned by the content-writing skill (money 1,000–1,500;
  location/region 1,000–1,400; hubs 1,500–2,500). No lorem/placeholder/empty blocks.
- Write for humans first (real local facts), then optimize.

## 6. Internal linking

- Location page → region page + homepage + ≥8 related location-service pages.
- Money page → ≥6 related services in the same location + location page + region page + one
  nearby location.
- Breadcrumb on every location page: `Home › {Region} › {Location} › {Service}` (matches the
  `BreadcrumbList` schema exactly).
- Descriptive, entity-dense anchor text — never "click here". No orphan pages: every page
  reachable ≤3 clicks from home.

## 7. Link equity (rank-and-rent focus)

- Money pages receive links from: their location page, 2–3 sibling service pages in the same
  location, and the region grid.
- Region pages link down to every location; homepage links to top regions + all service hubs.
- Keep link distance to money pages ≤3 clicks; distribute evenly — no orphan clusters, no
  over-favorited single page.

## 8. Open Graph & Twitter cards

- `og:locale`, `og:type`, `og:site_name`, `og:title`, `og:description`, `og:url` (= canonical),
  `og:image` (1200×630, see the image skill) + `og:image:width/height/alt`.
- `twitter:card=summary_large_image` + `twitter:title/description/image` + `twitter:image:alt`.
- Set globally in the layout; pages override **title/description only**.

## Checklist (done when)

- [ ] Unique title ≤60 with keyword+locale; meta 140–160 with CTA.
- [ ] Canonical absolute, canonical scheme, identical in `og:url` and `WebPage.url`.
- [ ] One H1, logical H2/H3 hierarchy, secondary keywords in H2s.
- [ ] Internal links per spec (≥8 location / ≥6 money); breadcrumb visible = schema.
- [ ] OG + Twitter complete; image sized + alt'd.
- [ ] SEO check script passes for this page (head/links/social items).
