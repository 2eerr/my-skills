---
name: seo-keyword-architecture
description: Keyword architecture for a local-SEO lead-gen site (any niche) — money-keyword patterns per page type, the three keyword tiers (primary/secondary/long-tail) with max-uses-per-page, how to build per-service keyword sets from research tools, cross-service and informational/FAQ keyword opportunities, and the related-services internal-linking map. Use when choosing target keywords or internal links for a page.
metadata:
  internal: true
---

# SEO Keyword Architecture

Master keyword inventory by page type. The slug is the canonical URL segment; the title is the
human page name. Populate the per-service sets from the project's keyword research
(`docs/KEYWORDS.md` or a data file) — the patterns below are niche-agnostic.

## Money-keyword pattern per page type

| Page type | Route | Primary keyword pattern |
|---|---|---|
| Core | `/`, `/about/`, `/contact/`, `/faq/`, `/privacy/`, `/terms/`, `/services/`, `/locations/`, `/404/` | brand / trust |
| Service (overview + hubs) | `/services/`, `/services/{slug}/` | `{service}` |
| Region | `/{region}/` | `{primary service} {Region}` |
| Location | `/{region}/{location}/` | `{primary service} {Location}, {Region}` |
| Money (location×service) | `/{region}/{location}/{service}/` | `{Service} in {Location}, {Region}` |

Total pages = core + services + regions + locations + (locations × services).

## Keyword tiers (per money page)

| Tier | What | Where | Max uses |
|---|---|---|---|
| **Primary** | main commercial keyword + city | H1, first paragraph, title, meta | 4–5 |
| **Secondary** | supporting service terms + local modifiers | H2/H3, body | 1–2 each, 4–6 distinct |
| **Long-tail** | specific scenario/informational questions | FAQ, body | 1 each, 3–5 distinct |

## Building per-service keyword sets

For each service, gather primary/secondary/long-tail keywords with volume + CPC from research
tools (Semrush, Ahrefs, Google Keyword Planner/Autocomplete, niche lead-gen tools). Store them
in the project's keyword data. Prioritize the highest-volume primaries for the homepage,
overview, and the largest locations. Services with no volume data yet fall back to the generic
`{service}` + `{service} {location}` pattern.

## Cross-service opportunities (homepage / overview / location pages)

High-intent "near me" and cost/timeline/insurance/prevention queries that span services belong
on the homepage, `/services/`, location pages, and the `/faq/` page. Rotate informational
questions (cost, how-long, how-it-works, insurance-coverage, prevention, emergency) into FAQ
sections.

## Related-services internal-linking map

Give every service a `related` array of ~6 services; money pages link to those in the same
location, and hubs link to their related services. Build the map by pairing services that
co-occur in the same job or share a category (e.g. a water-damage site pairs
mold-remediation ↔ mold-inspection ↔ sanitization; a roofing site pairs roof-leak-repair ↔
storm-damage ↔ emergency-tarping). Keep the full map in the project's keyword/service data.

## Rules

- Every micro-niche keyword appears naturally in its service hub; the matching money page
  includes 2–3 of them in H2/H3 + body.
- Never stuff — if a phrase exceeds its max, rephrase. Read the page aloud; cut repeats.
