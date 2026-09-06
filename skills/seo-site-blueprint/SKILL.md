---
name: seo-site-blueprint
description: "Phased build methodology for creating a local-SEO / rank-and-rent lead-gen site from zero (any service niche) — scope and data foundations, scaffold, design system, core pages, a pilot region that becomes the reference pattern, batched region-by-region rollout with deterministic page-count gates, the global SEO layer, QA/launch, post-launch iteration, and strict scope discipline (what NOT to build). Use when starting, planning, or gating a new lead-gen site build, deciding page scope, or reviewing build progress."
metadata:
  internal: true
---

# SEO Site Blueprint (phased build methodology)

The master plan for building a local-SEO lead-gen site end-to-end: **{N} services ×
{M} qualifying locations** rendered data-driven, shipped progressively behind a content gate.
The content-writing loop is owned by `seo-autopilot`; this skill owns the **phases, gates, and
scope decisions**.

## Project config (decide in Phase 0, never hardcode later)

**{Brand}**, **{domain}**, **niche / primary service**, **service list** (N subservices in
~5–7 categories), **geography scope** (region type + demand filter, e.g. cities ≥1,000 pop),
**content folder**, **data files**, and the **build/validate/check scripts**.

## Core strategy (the four pillars)

1. **Data-driven generation** — all locations/services live in JSON; the SSG renders every page
   from templates at build time (see `astro-ssg`). Never hand-write hundreds of page files.
2. **Phased & batched execution** — never generate everything at once; each phase has a review
   gate before the next starts.
3. **Hub-and-spoke internal linking** — home → region → location → money page, plus service
   hubs, so every page has rankable depth (see `seo-onpage-seo`).
4. **Content-gated publishing** — only pages with verified-unique content build and ship;
   rollout is progressive (see `seo-location-model` + `seo-deployment`).

## Scope discipline (decide once, enforce always)

- **Page inventory math:** total pages = core + services (overview + hubs) + regions +
  locations + (locations × services). Compute it in Phase 0; every later page count must match.
- **The scope questions** before building any page: Is it in the niche? Is it a core/hub/region/
  location/money page? Does the location pass the demand filter? Is the service in the fixed
  set? Does the total still match? If a page is added → update the plan, tracker, and data
  together.
- **Explicitly do NOT build** (at launch): blog/news/resources, out-of-niche expansions,
  neighborhoods as standalone pages (entities inside location pages only), sub-threshold
  locations, reviews/team/careers/aggregator pages, duplicate URL variants.

## Phases & gates

| # | Phase | Build | Gate (do not proceed until) |
|---|---|---|---|
| 0 | **Data foundations** | Lock brand + service list; build region/location datasets from an authoritative source; write a data-validation script (references, ranges, dupes, required fields). | Dataset clean, counts documented. |
| 1 | **Scaffold** | SSG project + Tailwind/tokens + config file (brand/NAP/analytics placeholders) + base layout with head wiring. | Dev server renders the homepage shell with the design system applied. |
| 2 | **Design system** | Tokens, core components, accessibility + responsive pass. | A `/design-system` showcase matches the design spec. |
| 3 | **Core pages** | Home, services overview, about/contact/faq/legal, 404 — real copy, no lorem. | Lighthouse ≥90 perf / ≥95 SEO on the homepage. |
| 4 | **Pilot region** | Build all location templates end-to-end for ONE region (region + location + money pages) with full SEO; generate only that region. | 5 random pages pass non-thin + schema + linking review. **This is the reference pattern for every later region.** |
| 5 | **Batched rollout** | Batch 1: essential pages (no locations). Batch 2: pilot. Then **one region per batch**: add data → validate → build → SEO check → spot-check → mark done. Content itself ships via `seo-autopilot`. | Per batch: page count grows by exactly the expected amount, 0 SEO-check issues, 5 random pages reviewed, sitemap matches output, no broken links introduced. |
| 6 | **SEO layer (global)** | Sitemap-index + per-region sitemaps, JSON-LD matrix everywhere, OG/Twitter, internal-link rules script-enforced, performance passes, analytics + Search Console verification. | Sitemap URL count = rendered page count; zero orphan pages; Lighthouse perf ≥95. |
| 7 | **QA & launch** | Crawl internal links (fix 404s/orphans), HTML/accessibility validation, per-batch content sample review, deploy setup, DNS + HTTPS + CDN/cache headers. | Clean crawl, no broken links, no duplicate titles/descriptions. |
| 8 | **Post-launch iteration** | Monitor Search Console + submit sitemaps; rotate local facts quarterly to keep pages fresh; track leads per page; prune or boost underperformers; topical-authority expansions (e.g. a blog) only as a deliberate later scope change. | Indexing grows; per-page lead data drives the prune/boost cycle. |

## Per-region batch checklist (Phase 5, every batch)

- [ ] Order: all location pages (+ money pages) first, region page last (autopilot rule).
- [ ] Demand filter applied correctly; data validation 0 errors.
- [ ] Page count grew by exactly: 1 region + N locations + N×services money pages.
- [ ] Unique title/meta per page; JSON-LD valid; internal links per spec.
- [ ] 5 random pages manually reviewed (content, entities, schema, links).
- [ ] Sitemap matches rendered output; tracker re-derived.

## Commands (pattern — wire to the project's real scripts)

```bash
npm run validate     # data sanity (Phase 0 gate, every batch)
npm run build        # render + post-build passes
npm run check:seo    # built-HTML enforcement (every batch gate)
# + word-count check, tracker generator, sitemap generator per the sibling skills
```

## Notes & gotchas

- **Don't skip the pilot** (Phase 4): templates locked without a reviewed reference region
  cause rework across thousands of pages.
- Template rollout and content uniqueness are **separate tracks** — pages can exist as routes
  long before any of them publish.
- Keep shared components heavy-use light: template cost multiplies by page count (see
  `astro-ssg` build-performance rules).
- Any scope change (new page type, new service) must update plan + tracker + data in one
  commit, or counts silently drift.
- **One doc per purpose.** Each project doc covers one topic completely (all SEO in one file,
  all content in another); before creating a new doc, check whether it belongs in an existing
  one — merge back anything that starts to scatter.
