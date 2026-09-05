---
name: water-damage-keywords
description: Keyword architecture for the Water Damage Pros US site — money-keyword patterns per page type, the three keyword tiers (primary/secondary/long-tail) with max-uses-per-page, per-service keyword sets for all 39 subservices, cross-service and informational/FAQ keyword opportunities, and the related-services internal-linking map. Use when choosing target keywords or internal links for a page.
---

# Water Damage Keyword Architecture

Master keyword inventory by page type. The slug is the canonical URL segment; the title is
the human page name.

## Money-keyword pattern per page type

| Page type | Route | Count | Primary keyword pattern |
|---|---|---|---|
| Core | `/`, `/about/`, `/contact/`, `/faq/`, `/privacy/`, `/terms/`, `/services/`, `/locations/`, `/404/` | 9 | brand / trust |
| Service (overview + hubs) | `/services/`, `/services/{slug}/` | 43 | `{service}` |
| State | `/{state}/` | 51 | `water damage restoration {State}` |
| City | `/{state}/{city}/` | 11,447 | `water damage restoration {City}, {State}` |
| Money (city×service) | `/{state}/{city}/{service}/` | 446,433 | `{Service} in {City}, {State}` |

Total = 457,980 pages (11,447 cities × 39 services = 446,433 money pages).

## Keyword tiers (per money page)

| Tier | What | Where | Max uses |
|---|---|---|---|
| **Primary** | main commercial keyword + city | H1, first paragraph, title, meta | 4–5 |
| **Secondary** | supporting service terms + local modifiers | H2/H3, body | 1–2 each, 4–6 distinct |
| **Long-tail** | specific scenario/informational questions | FAQ, body | 1 each, 3–5 distinct |

## Per-service keyword sets (39 services)

Each of the 39 subservices has a primary/secondary/long-tail set with national monthly volume
+ CPC. Highest-volume primaries to prioritize: `water damage restoration` (49.5K),
`mold removal` (33.1K), `mold remediation` (22.2K), `water damage repair` (22.2K),
`basement waterproofing` (22.2K), `water mitigation services` (8.1K), `mold inspection` (8.1K),
`sump pump installation` (8.1K), `foundation waterproofing` (6.6K), `roof leak repair` (6.6K).

The full per-service tables (with volumes/CPC) live in `docs/KEYWORDS.md` §2A — read that file
for the exact keyword list of the service you're writing. Services with no volume data yet
(appliance-water-connection, commercial/residential-water-damage) use the generic pattern.

## Cross-service opportunities (homepage / overview / city pages)

`water damage restoration near me` (18.1K), `water damage repair near me` (9.9K),
`mold remediation near me` (8.1K), `flood cleanup near me` (3.6K), `water extraction near me`
(1.9K), `emergency water damage near me` (1.3K). Informational/FAQ clusters (cost, timeline,
insurance, prevention, emergency) feed the `/faq/` page and FAQ sections.

## Related-services internal-linking map

Every service has a `related` array of 6 services; money pages link to those 6 in the same
city, and hubs link to their related services. Canonical pairs (source → 6 related):

- water-damage-restoration → emergency-water-extraction, flood-damage-cleanup, moisture-mapping, mold-remediation, water-damage-repair, water-mitigation
- flood-damage-cleanup → water-damage-restoration, emergency-water-extraction, sewage-cleanup, sanitization, moisture-mapping, storm-damage-restoration
- emergency-water-extraction → water-damage-restoration, moisture-mapping, air-quality-testing, water-damage-repair, sanitization, flood-damage-cleanup
- water-damage-repair → water-damage-restoration, drywall-restoration, subfloor-replacement, hardwood-floor-drying, moisture-mapping, insulation-replacement
- water-damage-inspection → water-damage-restoration, mold-inspection, moisture-mapping, water-damage-repair, emergency-water-extraction, sanitization
- mold-remediation → mold-inspection, water-damage-restoration, sanitization, odor-removal, moisture-mapping, air-quality-testing
- mold-inspection → mold-remediation, water-damage-inspection, air-quality-testing, moisture-mapping, water-damage-restoration, emergency-water-extraction
- sewage-cleanup → sanitization, flood-damage-cleanup, water-damage-restoration, odor-removal, emergency-water-extraction, moisture-mapping
- storm-damage-restoration → wind-damage-restoration, flood-damage-cleanup, roof-leak-repair, emergency-tarping, water-damage-restoration, moisture-mapping
- basement-waterproofing → crawlspace-waterproofing, sump-pump-installation, water-damage-restoration, foundation-waterproofing, plumbing-leak-repair, moisture-mapping
- burst-pipe-repair → appliance-water-connection, plumbing-leak-repair, water-damage-restoration, emergency-water-extraction, moisture-mapping, basement-waterproofing

The complete 39-row map is in `docs/KEYWORDS.md` (Related Services) and `docs/SERVICES.md` §8.

## Rules

- Every micro-niche keyword appears naturally in its service hub; the matching money page
  includes 2–3 of them in H2/H3 + body.
- Never stuff — if a phrase exceeds its max, rephrase. Read the page aloud; cut repeats.
