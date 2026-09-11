---
name: seo-schema
description: "Structured data (JSON-LD) for a local-SEO lead-gen site (any niche) — base graph, per-page schema matrix, @id conventions, FAQPage-from-own-content-only, never-fabricate-reviews, LocalBusiness/NAP, and validation. Use when authoring, editing, or auditing any page's JSON-LD."
metadata:
  internal: true
---

# SEO Structured Data (JSON-LD)

The single authoritative schema spec for a local-SEO site (any service niche).
Every generated and hand-written page's structured data follows this file — no other skill or
doc restates the matrix. Read the project's config for brand, domain, and NAP — never hardcode.

## When to use it

- Adding or changing JSON-LD on any page type (home, region, location, money, hub, core, index).
- Auditing built pages for schema validity or schema↔content mismatches.
- **Not for:** titles/meta/OG (see the on-page skill), sitemaps/robots/performance (technical
  skill), GBP/citations/NAP (local skill), images (image skill), or copy rules (content-writing).

## Project config (supply per site, never hardcode)

**{Brand}**, **{domain}**, **NAP** (name/address/phone from the site config), the trade's
**`@type`** (default `LocalBusiness`; pick a more specific `HomeAndConstructionBusiness`
subtype when one fits), and the page-type set (core / hub / region / location / money).

## 1. Base graph (every page)

- Emitted via `<script type="application/ld+json">` in one `@graph` per page.
- Every page ships **`WebSite` + `Organization` + `WebPage`**; each page then adds its own
  nodes (sections below).
- **Consistent node IDs across the whole site:** `https://{domain}/#website`,
  `/#organization`, `/#localbusiness`, `/#webpage`, `/#breadcrumb`. The page's `WebPage` node
  references the other nodes by `@id` — never re-declares them inline.
- Every `BreadcrumbList` carries `@id` `…/#breadcrumb` and is referenced by that page's
  `WebPage`. URLs inside schema use the site's canonical URL scheme (same absolute trailing-
  slash form as canonical/`og:url`/sitemap — never a variant).

## 2. Schema matrix (what each page type emits)

| Page | Schema |
|---|---|
| Homepage | `LocalBusiness` + `WebSite` + `WebPage` (+ `FAQPage`) |
| Region | `WebPage` + `BreadcrumbList` (+ `Service` + `FAQPage`) |
| Location | `WebPage` + `BreadcrumbList` + `Service` (+ `FAQPage`) |
| Service hub | `Service` + `WebPage` + `BreadcrumbList` (+ `FAQPage`) |
| Money (location×service) | `Service` + `WebPage` + `BreadcrumbList` (+ `LocalBusiness` + `FAQPage`) |
| About | `AboutPage` + business entity (`Organization` via `mainEntity`) |
| Contact | `ContactPage` + business entity (`Organization` via `mainEntity`) |
| FAQ | `WebPage` + `BreadcrumbList` (+ `FAQPage` from the page's own FAQs) |
| Locations index | `WebPage` + `BreadcrumbList` + `ItemList` + `FAQPage` |
| Services index | `WebPage` + `ItemList` + `FAQPage` |

- `(+ …)` = emit only when the page actually has that content (e.g. no FAQs → no `FAQPage`).
- Article-style pages (blog) would use `Article` + `WebPage` + `BreadcrumbList` — only if the
  project has such pages.

## 3. FAQPage — content-only, never templated

- `FAQPage` is emitted **only** from the page's own `faqs:` frontmatter (≥1 Q&A) — never from
  templated, shared, or fallback FAQ sets. A page without content FAQs renders no FAQ section
  and no `FAQPage` node.
- The JSON-LD Q&A text **must match the visible FAQ text exactly** (rendered accordion/list and
  schema come from the same source).
- Each page's FAQ set is unique (per the content-writing skill) — so every `FAQPage` on the
  site is unique too.

## 4. Reviews & ratings — never fabricate

- `AggregateRating` / review markup is emitted **only when real, legitimate reviews exist**.
- No reviews yet → no rating schema at all (plain-UI testimonials are not schema). Never invent
  values, counts, or authors.

## 5. LocalBusiness entity

- `@type` = `LocalBusiness` (or the closest `HomeAndConstructionBusiness` subtype for the
  trade), `@id` `https://{domain}/#localbusiness`; emitted on the homepage and money pages.
- Carries `name`, `url`, `image`, `telephone`, `address` (`PostalAddress`), `areaServed` — all
  from the project config. **NAP must be identical** across site, footer, schema, and (once
  real) Google Business Profile / citations.
- Extend with `geo`, `openingHoursSpecification`, `sameAs` (GBP/socials), `priceRange` only
  when a real office/GBP exists — never placeholder values.

## 6. Enforcement & validation

- The SEO check script scans built HTML and fails on: missing/invalid JSON-LD on
  schema-required pages, duplicate/missing canonical-vs-schema URL mismatches.
- Validate after every build; spot-check 5 random pages per batch for schema↔visible-content
  match (FAQ text, breadcrumb order, NAP).

## Checklist (done when)

- [ ] Base graph (`WebSite`+`Organization`+`WebPage`) present on every page, referenced by `@id`.
- [ ] Page emits exactly its matrix row's nodes — no more, no less.
- [ ] All schema URLs absolute, canonical scheme, matching `og:url`/canonical/sitemap.
- [ ] `FAQPage` only from the page's own FAQs, text identical to what's visible.
- [ ] No `AggregateRating`/review markup without legitimate source data.
- [ ] `LocalBusiness` NAP matches config/footer exactly; IDs consistent site-wide.
- [ ] SEO check script passes on built output.

## Notes & gotchas

- One `@graph` per page — do not emit multiple disconnected `<script type="application/ld+json">`
  blocks that duplicate the same node.
- Breadcrumb schema order must mirror the visible breadcrumb (`Home › {Region} › {Location} ›
  {Service}`) and link to real published URLs.
- When the domain changes, every schema URL, `@id`, and the `WebSite`/`Organization` nodes must
  be regenerated from config — never hand-patched.
- `ItemList` on index pages lists only published pages (same content gate as sitemaps).
