---
name: seo-content-writing
description: Author unique, entity-rich, expert-voice SEO content pages for a local-SEO lead-gen site (any service niche) — money, location, region, service-hub, and core pages. Enforces the no-template/no-copy-paste rule, ≥90% uniqueness, entity floors, per-type word-count ranges, the markdown/UI formatting standard, AI-free & plagiarism-free rules, per-page specs, and the pre-ship quality gates. Use when writing or reviewing page copy for a rank-and-rent or service-area site.
metadata:
  internal: true
---

# SEO Content Writing Standard

The single authoritative writing spec for a local-SEO site (any service niche).
A page is **not done** until it passes §Quality Gates and its tracker row re-derives to `[x]`.
Read the project's config for brand, domain, niche, and service list — never hardcode them.

## Strict content rule (mandatory)

1. No templated content — each page written from scratch for its own location + service.
2. No scripts to write content — scripts only build routes, assemble data, verify.
3. No copy-paste / duplicated content anywhere on the site.
4. **≥90% unique** vs every sibling page (same service other locations, other services this
   location, region/location/hub pages). Synonym-swapping or reshuffling does **not** count.
5. Direct write only — into the page's own file in the content folder.
6. Unique, SEO-optimized, entity-rich: own title/meta, real local entities, expert voice,
   localized FAQ set.

## Page families & where each is written

| Family | Path pattern |
|---|---|
| Core | `{content}/core/{slug}.md` |
| Service hubs | `{content}/hubs/{slug}.md` |
| Region | `{content}/regions/{slug}.md` |
| Location | `{content}/locations/{region}/{slug}.md` |
| Money (location×service) | `{content}/money/{region}/{location}/{service}.md` |

## Per-page specs (word count · entities · FAQs)

- **Money page** — 1,000–1,500 words · ≥15 entities · 3–9 FAQs (count varies per page).
  Outline: intro → signs → why {Location} needs it → how we work → long-tail subservices →
  cost table → prevention → service area/neighborhoods → FAQ → CTA → related links.
  Title/H1: `{Service} in {Location}, {Region} | {Brand}`.
- **Location page** — 1,000–1,400 words · ≥8 entities · ≥3 FAQs. Facts strip (county/region,
  climate, geography) at top, services grid below. **Population is internal-only — never
  featured.** Title: `{Primary service} in {Location}, {Region} | {Brand}`.
- **Region page** — 1,000–1,400 words · ≥5 entities · ≥3 FAQs. Locations grid at top, services
  below, regional/seasonal patterns. Title: `{Primary service} in {Region} | {Brand}`.
- **Service hub** — 1,500–2,500 words · ≥15 entities · ≥3 FAQs. What it is → signs → how →
  cost table → DIY vs pro → prevention → FAQ → links to top regions/locations.
- **Core pages** — home 1,000–1,800 (complementary prose, must not duplicate template
  components); about ≥500; contact ≥200; faq ≥500; services overview ≥300; privacy/terms legal.

## Long-tail subservices (every money page)

Weave **3–5** hyper-specific subservices related to the main service into body + FAQ (e.g. for
a roofing site: "skylight leak repair", "flat roof replacement"; for mold: "attic mold
remediation", "crawlspace mold cleanup"). Vary by local housing stock. 1–2 mentions each,
never stuffed.

## Neighborhoods (location + money pages)

Include **3–6 real, verifiable neighborhoods/districts** per page (vary the count), each with
1–2 sentences: name, what the area is known for, why it's relevant to the service. Weave into
prose or an H2 + description block — never a bare bullet list. Location page and its money
pages overlap but are not identical.

## Entity rules

Name entities precisely; make relationships explicit; include local entities (county/region,
climate zone, nearby towns, neighborhoods, landmarks, local institutions). Entity floors:
≥15 money, ≥8 location, ≥5 region. Build the entity graph around the niche's real-world
concepts (materials, damage/issue types, methods, equipment, regulations).

## Expert voice

Read like a licensed local contractor/practitioner with years in that area. First-person trade
observations (1–2 per money page), practical/honest, correct trade vocabulary explained in
plain English, name the real local problems the area causes for this service, confidence
without hype. **Forbidden:** invented licenses/stats/reviews, generic platitudes,
regionally-wrong claims.

## Readability

8th-grade level (~60–70 Flesch). Short sentences, plain English, **1–3 sentence paragraphs**,
one idea per sentence, every sentence adds a fact.

## AI-free & plagiarism-free

Pass an AI detector (<10% on batch sample). Vary sentence length/structure. No formulaic
openers/closers ("In today's…", "Whether you're dealing with…", "In conclusion"), no
bullet-spam without context, no em-dash overuse, no "Moreover/Furthermore". Concrete numbers +
local references. FAQ answers unique per page. Spot-check plagiarism per batch.

## Formatting / UI standard (hard gate)

Content **is** the UI — rendered through the design system. Use **real markdown semantics**
(`##`/`###`, `-` lists, `1.` lists, `|` tables, `>` blockquote). Never fake structure with bold
paragraphs.

- Money page floor: ≥5 H2, ≥2 bullet lists (bold lead-ins), ≥1 numbered list, ≥1 table,
  ≥1 blockquote, 3–9 FAQs, natural CTA.
- Location: ≥4 H2, ≥1 bullet list, ≥1 list/table, ≥3 FAQs, CTA.
- Region: ≥4 H2, ≥2 bullet lists, ≥1 seasonal/regional table, ≥3 FAQs, CTA.
- Hub: ≥6 H2, ≥2 bullet lists, ≥1 numbered list, ≥1 table, ≥3 FAQs, CTA.

Forbidden: raw pastes, walls of text, bullet lists >8 items, tables without heading+intro,
H2 with <2 sentences, paragraphs standing in for lists.

## Keyword frequency (per page)

- Money keyword `{Service} in {Location}, {Region}` → H1 + first paragraph, **4–5× max**.
- Primary keyword → H1, first paragraph, ≥1 H2, **4–5× max**.
- Secondary/long-tail → H2/H3 + body, **1–2× each**, 4–6 distinct phrases.
- Density natural (0.5–1.5%). Exact-match money keyword in H1 + intro + one H2 + one FAQ.

## File format

Frontmatter between `---`: `title` (required), `description` (required, 140–160 chars),
`faqs` (array of `{q, a}`, 3–9, unique per page), `unique: true` (publishes the page; must be
on its own line). FAQs live in frontmatter, not body. Body is structured markdown per the
per-page outline.

## Quality gates (before a page ships)

- [ ] ≥90% unique vs every sibling (measured + recorded)
- [ ] No reused sentence anywhere on the site
- [ ] Word count within its type's range (word-count check script)
- [ ] Entity floor met (15/8/5)
- [ ] Main keyword in H1, intro, ≥1 H2, 1 FAQ; ≥3 secondary keywords
- [ ] 3–5 long-tail subservices; local entities present; 3–5 local-expert specifics;
      1–2 real local problems named
- [ ] 8th-grade readability; expert voice; AI-detector + plagiarism checks pass
- [ ] FAQ unique + matches FAQPage schema; ≥1 table on money/hub pages
- [ ] UI/UX floor met; ≥6 internal links (money) / ≥8 (location)
- [ ] SEO check script + non-thin review pass; row re-derives to `[x]` after the tracker
      generator runs
