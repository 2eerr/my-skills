---
name: seo-service-catalog
description: Model the service/subservice catalog for a local-SEO lead-gen site (any niche) — grouping services into categories, canonical slugs, routing, where each service appears (overview, hub, money, location, mega menu), hub & money page specs, publishing rules, and a fixed batch writing order. Use when defining the service list, building service hubs, or selecting services for money pages.
metadata:
  internal: true
---

# SEO Service Catalog

Define the site's **fixed set of subservices** (any niche) grouped into categories. Each gets
a national hub `/services/{slug}/` plus one money page per location. `/services/` (overview)
is the extra index page.

## Structure (niche-agnostic)

- Pick a bounded service list (e.g. 20–50 subservices) that covers the niche's real,
  searchable jobs. Group them into ~5–7 categories (e.g. Core, Surface/Material,
  Damage/Repair, Prevention/Protection, Specialty, Source-Specific).
- Each service = one canonical slug (lowercase, hyphenated, never varied) used as the URL
  segment and the money-page filename.

## Service record (e.g. `src/data/services.json`)

`{ slug, title, category, description, icon, related[~6] }` — slug is the canonical URL
segment; `related` drives cross-linking (see the keyword-architecture skill).

## Where each service appears (5 places)

| Layer | Route | Content source |
|---|---|---|
| Overview | `/services/` | `{content}/core/services.md` |
| Hub | `/services/{slug}/` | `{content}/hubs/{slug}.md` |
| Money | `/{region}/{location}/{slug}/` | `{content}/money/{region}/{location}/{slug}.md` |
| Location | `/{region}/{location}/` | links all services for that location |
| Mega menu | global | data-driven from the services file |

## Page specs

- **Hub** (`/services/{slug}/`): national authority page, 1,500–2,500 words, ≥15 entities,
  ≥3 unique FAQs, schema `Service + WebPage + BreadcrumbList (+ FAQPage)`. Always builds,
  ships only with `unique: true`.
- **Money** (`/{region}/{location}/{slug}/`): conversion page, 1,000–1,500 words, ≥15 entities,
  3–9 unique FAQs, ≥6 related-service links, schema `Service + LocalBusiness + FAQPage +
  BreadcrumbList`.

## Publishing rules

- Overview always ships (core page).
- Hub ships when `hubs/{slug}.md` has `unique: true` — per service, no cross-dependency.
- Money page ships when its file has `unique: true` — per page.
- Header Services mega menu is hidden until ≥1 hub has `unique: true`.

## Fixed batch writing order

When generating content for a new location, write all services in one **fixed order** (define
it once in the project — typically highest-volume/core services first, specialty last) so
batches are deterministic and comparable across locations.

## Validation

Validate the services data file structure · build · run the SEO check · run the word-count
check (hubs 1,500–2,500).
