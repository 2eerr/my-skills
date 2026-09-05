---
name: water-damage-services
description: The 39 water-damage subservices catalog for Water Damage Pros US — the six categories, canonical slugs, routing, where each service appears (overview, hub, money, city, mega menu), hub & money page specs, publishing rules, and the fixed 39-service batch writing order. Use when working with the service list, service hubs, or money-page service selection.
---

# Water Damage Services Catalog

**Exactly 39 subservices** across 6 categories. Each gets a national hub `/services/{slug}/`
plus one money page per city. `/services/` (overview) is the 43rd service page.

## Categories & services (canonical slugs)

**Core Services (6):** water-damage-restoration, flood-damage-cleanup,
emergency-water-extraction, water-damage-repair, water-damage-inspection, water-mitigation

**Surface & Material (8):** drywall-restoration, hardwood-floor-drying, carpet-drying,
subfloor-replacement, insulation-replacement, ceiling-repair, content-restoration,
document-drying

**Damage & Restoration (10):** mold-remediation, mold-inspection, sanitization,
sewage-cleanup, storm-damage-restoration, wind-damage-restoration, ice-dam-removal,
roof-leak-repair, emergency-tarping, fire-suppression-water-damage

**Prevention & Protection (11):** basement-waterproofing, crawlspace-waterproofing,
sump-pump-installation, moisture-mapping, air-quality-testing, odor-removal,
electronic-restoration, appliance-water-connection, burst-pipe-repair, plumbing-leak-repair,
foundation-waterproofing

**Specialty (2):** commercial-water-damage, residential-water-damage

**Source-Specific Repair (2):** pipe-thawing, window-leak-repair

## Service record (`src/data/services.json`)

`{ slug, title, category, description, icon, related[6] }` — slug is the canonical URL
segment (never vary it); `related` drives cross-linking (see the keywords skill).

## Where each service appears (5 places)

| Layer | Route | Content source |
|---|---|---|
| Overview | `/services/` | `src/content/core/services.md` |
| Hub | `/services/{slug}/` | `src/content/hubs/{slug}.md` |
| Money | `/{state}/{city}/{slug}/` | `src/content/money/{state}/{city}/{slug}.md` |
| City | `/{state}/{city}/` | links all 39 for that city |
| Mega menu | global | data-driven from `services.json` |

## Page specs

- **Hub** (`/services/{slug}/`): national authority page, 1,500–2,500 words, ≥15 entities,
  ≥3 unique FAQs, schema `Service + WebPage + BreadcrumbList (+ FAQPage)`. Always builds,
  ships only with `unique: true`.
- **Money** (`/{state}/{city}/{slug}/`): conversion page, 1,000–1,500 words, ≥15 entities,
  3–9 unique FAQs, ≥6 related-service links, schema `Service + LocalBusiness + FAQPage +
  BreadcrumbList`.

## Publishing rules

- Overview always ships (core page).
- Hub ships when `hubs/{slug}.md` has `unique: true` — per service, no cross-dependency.
- Money page ships when its file has `unique: true` — per page.
- Header Services mega menu is hidden until ≥1 hub has `unique: true`.

## Fixed batch writing order (all 39, per city)

1 water-damage-restoration · 2 flood-damage-cleanup · 3 emergency-water-extraction ·
4 water-damage-repair · 5 water-damage-inspection · 6 drywall-restoration ·
7 hardwood-floor-drying · 8 carpet-drying · 9 subfloor-replacement · 10 insulation-replacement ·
11 ceiling-repair · 12 content-restoration · 13 document-drying · 14 mold-remediation ·
15 mold-inspection · 16 sanitization · 17 sewage-cleanup · 18 storm-damage-restoration ·
19 wind-damage-restoration · 20 ice-dam-removal · 21 roof-leak-repair · 22 emergency-tarping ·
23 basement-waterproofing · 24 crawlspace-waterproofing · 25 sump-pump-installation ·
26 moisture-mapping · 27 air-quality-testing · 28 odor-removal · 29 electronic-restoration ·
30 appliance-water-connection · 31 burst-pipe-repair · 32 plumbing-leak-repair ·
33 commercial-water-damage · 34 residential-water-damage · 35 water-mitigation ·
36 foundation-waterproofing · 37 pipe-thawing · 38 fire-suppression-water-damage ·
39 window-leak-repair

## Validation

`npm run validate` (services.json) · `npm run build` · `npm run check:seo` ·
`npm run check:wordcounts` (hubs 1,500–2,500).
