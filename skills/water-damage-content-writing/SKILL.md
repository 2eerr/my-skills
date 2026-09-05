---
name: water-damage-content-writing
description: Author unique, entity-rich, expert-voice SEO content pages for the Water Damage Pros US site — money, city, state, service-hub, and core pages. Enforces the no-template/no-copy-paste rule, ≥90% uniqueness, entity floors, per-type word-count ranges, the markdown/UI formatting standard, AI-free & plagiarism-free rules, per-page specs, and the pre-ship quality gates. Use when writing or reviewing any page copy for the site.
---

# Water Damage Content Writing Standard

The single authoritative writing spec. A page is **not done** until it passes §Quality Gates
and its tracker row re-derives to `[x]`.

## Strict content rule (mandatory)

1. No templated content — each page written from scratch for its own city + service.
2. No scripts to write content — scripts only build routes, assemble data, verify.
3. No copy-paste / duplicated content anywhere on the site.
4. **≥90% unique** vs every sibling page (same service other cities, other services this
   city, city/state/hub pages). Synonym-swapping or reshuffling does **not** count.
5. Direct write only — into the page's own `.md` file in `src/content/`.
6. Unique, SEO-optimized, entity-rich: own title/meta, real local entities, expert voice,
   localized FAQ set.

## Page families & where each is written

| Family | Count | Path |
|---|---|---|
| Core | 9 | `src/content/core/{slug}.md` |
| Service hubs | 39 | `src/content/hubs/{slug}.md` |
| State | 51 | `src/content/states/{slug}.md` |
| City | 11,447 | `src/content/cities/{state}/{city}.md` |
| Money (city×service) | 446,433 | `src/content/money/{state}/{city}/{service}.md` |

## Per-page specs (word count · entities · FAQs)

- **Money page** — 1,000–1,500 words · ≥15 entities · 3–9 FAQs (count varies per page).
  Outline: intro → signs → why {City} needs it → how we work → long-tail subservices →
  cost table → prevention → service area/neighborhoods → FAQ → CTA → related links.
  Title/H1: `{Service} in {City}, {State} | Water Damage Pros US`.
- **City page** — 1,000–1,400 words · ≥8 entities · ≥3 FAQs. Facts strip (county, climate,
  geography) at top, services grid below. **Population is internal-only — never featured.**
  Title: `Water Damage Restoration in {City}, {State} | Water Damage Pros US`.
- **State page** — 1,000–1,400 words · ≥5 entities · ≥3 FAQs. Cities grid at top, services
  below, regional/seasonal patterns. Title: `Water Damage Restoration in {State} | ...`.
- **Service hub** — 1,500–2,500 words · ≥15 entities · ≥3 FAQs. What it is → signs → how →
  cost table → DIY vs pro → prevention → FAQ → links to top states/cities.
- **Core pages** — home 1,000–1,800 (complementary prose, must not duplicate template
  components); about ≥500; contact ≥200; faq ≥500; services overview ≥300; privacy/terms
  legal.

## Long-tail subservices (every money page)

Weave **3–5** hyper-specific subservices related to the main service into body + FAQ
(e.g. mold-remediation → attic mold remediation, bathroom mold removal, crawlspace mold
cleanup). Vary by city housing stock. 1–2 mentions each, never stuffed.

## Neighborhoods (city + money pages)

Include **3–6 real, verifiable neighborhoods** per page (vary the count), each with 1–2
sentences: name, what the area is known for, why it's relevant to water damage. Weave into
prose or an H2 + description block — never a bare bullet list. City page and its money pages
overlap but are not identical.

## Entity rules

Name entities precisely; make relationships explicit; include local entities (county, region,
climate zone, nearby cities, neighborhoods, landmarks, local institutions). Entity floors:
≥15 money, ≥8 city, ≥5 state.

## Expert voice

Read like a licensed local water damage restoration contractor with years in that city.
First-person trade observations (1–2 per money page), practical/honest, correct trade
vocabulary explained in plain English, name the real local problems the area causes,
confidence without hype. **Forbidden:** invented licenses/stats/reviews, generic platitudes,
regionally-wrong claims.

## Readability

8th-grade level (~60–70 Flesch). Short sentences, plain English, **1–3 sentence paragraphs**,
one idea per sentence, every sentence adds a fact.

## AI-free & plagiarism-free

Pass an AI detector (<10% on batch sample). Vary sentence length/structure. No formulaic
openers/closers ("In today's…", "Whether you're dealing with…", "In conclusion"), no
bullet-spam without context, no em-dash overuse, no "Moreover/Furthermore". Concrete numbers
+ local references. FAQ answers unique per page. Spot-check plagiarism per batch.

## Formatting / UI standard (hard gate)

Content **is** the UI — rendered via `<PageContent />` in `prose-brand`. Use **real markdown
semantics** (`##`/`###`, `-` lists, `1.` lists, `|` tables, `>` blockquote). Never fake
structure with bold paragraphs.

- Money page floor: ≥5 H2, ≥2 bullet lists (bold lead-ins), ≥1 numbered list, ≥1 table,
  ≥1 blockquote, 3–9 FAQs, natural CTA.
- City: ≥4 H2, ≥1 bullet list, ≥1 list/table, ≥3 FAQs, CTA.
- State: ≥4 H2, ≥2 bullet lists, ≥1 seasonal/regional table, ≥3 FAQs, CTA.
- Hub: ≥6 H2, ≥2 bullet lists, ≥1 numbered list, ≥1 table, ≥3 FAQs, CTA.

Forbidden: raw pastes, walls of text, bullet lists >8 items, tables without heading+intro,
H2 with <2 sentences, paragraphs standing in for lists.

## Keyword frequency (per page)

- Money keyword `{Service} in {City}, {State}` → H1 + first paragraph, **4–5× max**.
- Primary keyword → H1, first paragraph, ≥1 H2, **4–5× max**.
- Secondary/long-tail → H2/H3 + body, **1–2× each**, 4–6 distinct phrases.
- Density natural (0.5–1.5%). Exact-match money keyword in H1 + intro + one H2 + one FAQ.

## File format

Frontmatter between `---`: `title` (required), `description` (required, 140–160 chars),
`faqs` (array of `{q, a}`, 3–9, unique per page), `unique: true` (publishes the page; must be
on its own line). FAQs live in frontmatter, not body. Body is structured markdown per §9.

## Quality gates (before a page ships)

- [ ] ≥90% unique vs every sibling (measured + recorded)
- [ ] No reused sentence anywhere on the site
- [ ] Word count within its type's range (`node scripts/check-wordcounts.js`)
- [ ] Entity floor met (15/8/5)
- [ ] Main keyword in H1, intro, ≥1 H2, 1 FAQ; ≥3 secondary keywords
- [ ] 3–5 long-tail subservices; local entities present; 3–5 local-expert specifics;
      1–2 real local problems named
- [ ] 8th-grade readability; expert voice; AI-detector + plagiarism checks pass
- [ ] FAQ unique + matches FAQPage schema; ≥1 table on money/hub pages
- [ ] UI/UX floor met (§10.4); ≥6 internal links (money) / ≥8 (city)
- [ ] `check-seo.js` + non-thin review pass; row re-derives to `[x]` after
      `node scripts/gen-pages-tracker.js`
