---
name: seo-local-seo
description: "Local SEO playbook for a local-SEO lead-gen site (any service niche) — local on-page signals per city/region page, the LocalBusiness entity handoff to the schema skill, Google Business Profile and citation rules, NAP consistency, and local KPIs. Use when making a page compete in a specific area's results, or setting up post-launch local presence."
metadata:
  internal: true
---

# Local SEO

How each location and money page competes in its area's organic + map results on a
rank-and-rent lead-gen site (any service niche). Every money page targets a local commercial
keyword (`{Service} in {Location}, {Region}`); local SEO is how it wins. Read the project's
config for brand + NAP — never hardcode.

## 1. Local on-page signals (every location & money page)

- **Keyword + locale in every tag:** title, meta description (keyword + locale +
  differentiator), H1, one H2, one FAQ answer.
- **Local entities in copy:** county/region, climate zone, nearby locations, metro area,
  neighborhoods, landmarks, local institutions (per the data-packs + content-writing skills).
  Population/demand numbers are internal-only — never featured.
- **Localized FAQ set:** questions a local homeowner actually asks (county, weather,
  neighborhood, insurance), unique per page.
- **Local internal links:** location page → region + ≥8 service pages; money page → ≥6 related
  services in the same location + location + region + one nearby location (see on-page skill).

## 2. LocalBusiness entity (markup owned by the schema skill)

- The canonical local entity and its JSON-LD (`@type`, `@id`, `areaServed`, NAP fields,
  extension rules) live in `seo-schema` — apply it, don't restate it.
- This skill owns the **real-world side**: the NAP values fed into that entity must match the
  site config, footer, and every citation exactly.

## 3. Google Business Profile & citations (post-launch)

- **GBP:** claim one profile per real service location; category matches the trade; add
  services, hours, photos, and the site URL. NAP must equal `SITE` config values.
- **Citation consistency:** identical NAP across GBP, local directories, socials, and schema —
  mismatches hurt local rankings.
- **Reviews:** earn and respond to genuine reviews only; never fabricate (schema rule too).
  Link the review source via `sameAs` once it exists.
- **Local landing pages:** each location page already acts as one — keep them ≥90% unique so
  they rank independently.

## 4. NAP consistency

- NAP (name/address/phone) identical across site, footer, schema, GBP, and citations.
- Local/tracking phone configurable per location in the project config (placeholder until real).

## 5. Local KPIs

- Each money page ranks for its exact `{Service} in {Location}, {Region}` query.
- Location pages surface in local organic/map results for `{primary service} {Location}`.
- Monitor Search Console impressions/clicks per location; prune or boost underperformers on a
  schedule (rotate local facts to keep pages fresh).

## Checklist (done when)

- [ ] Keyword+locale present in title, meta, H1, one H2, one FAQ.
- [ ] Local entities + localized FAQ on the page (verified, from its data pack).
- [ ] Local internal links per spec; breadcrumb path correct.
- [ ] NAP byte-identical across site, footer, schema (and GBP/citations once live).
- [ ] No fabricated reviews, ratings, or invented local claims.
- [ ] Per-location ranking/CTR tracked in Search Console.
