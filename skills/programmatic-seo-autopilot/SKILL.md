---
name: programmatic-seo-autopilot
description: Run a content autopilot for a programmatic local-SEO lead-gen site (any service niche) — write every missing page (core, service hubs, region, location, and location×service money pages) by hand, one at a time, honoring a generated tracker, claiming a region before writing it, and writing all of a region's locations before its region page. Use when the user references a start/tracker file or asks to generate/write content pages for a rank-and-rent or service-area site.
metadata:
  internal: true
---

# Programmatic SEO Content Autopilot

Turns you into a content autopilot for a **programmatic local-SEO lead-gen site** — any
service niche (water damage, roofing, HVAC, landscaping, moving, etc.) covering a geography
× a service list. You write every missing page directly to the content folder, one at a time,
without stopping for permission, until the tracker is all `[x]`.

This skill is the **orchestrator**. Detailed specs live in the sibling skills:
`programmatic-seo-content-writing`, `programmatic-seo-onpage-seo`,
`programmatic-seo-technical-seo`, `programmatic-seo-schema`, `programmatic-seo-local-seo`,
`programmatic-seo-image-seo`, `programmatic-seo-keyword-architecture`,
`programmatic-seo-service-catalog`, `programmatic-seo-location-model`,
`programmatic-seo-data-packs`, `programmatic-seo-design-system`, `programmatic-seo-deployment`.

## Project config (supply per site, never hardcode in the skill)

Before running, read the project's config (e.g. `site.config.js`, `docs/`, `src/data/`) for:
**brand name**, **domain**, **niche / primary service**, **service list** (N services in M
categories), **geography scope** (e.g. US states → cities ≥1K pop), **content folder**
(e.g. `src/content/`), and the **tracker/claim/build scripts**. Everything below uses generic
`{region}` / `{location}` / `{service}` — map them to the project's real units (state/city,
metro/suburb, country/city, etc.).

## Hard rule — no templates, no scripts, no copy-paste

- Write every page **individually and by hand** into its own file. No reusable skeleton, no
  fill-in-the-blank outline, no shared block.
- **Scripts may build routes, assemble data, and verify output — never write page words.**
- No sentence, paragraph, list item, table, or FAQ may be reused from any other page.
- Every page must be **≥90% unique** from every other page on the site.
- If you are about to reuse text you already wrote — **stop and rewrite**.

## Phase 1 — Gather context (every time)

1. Read all reference docs (`docs/*.md`) or load the sibling skills above.
2. Scan the content folder (`core/`, `hubs/`, regions, locations, money) to see what exists.

## Phase 1.5 — Only write unwritten pages

Read the tracker first and honor statuses. Never clobber parallel work (multiple machines
synced via git).

| Status | Meaning | Action |
|---|---|---|
| `[ ]` | missing / not started | writable |
| `[~]` | in progress (claimed, no unique content yet) | yours to write |
| `[x]` | done (unique content exists) | never touch |
| `[!]` | exists but not unique (flagged for rework) | only rewrite if user explicitly asks |

If the user asks for a page that already has content, **do not rewrite it** — warn them it is
already written (with its status) and ask before overwriting.

## Phase 1.7 — Claim a region before writing it

So other machines don't duplicate work:

1. Run the project's claim script (e.g. `node temp/claim-state.mjs <region-slug>`) — flips the
   region + its location/money rows to `[~]`.
2. Commit + push the claim so peers see it (`git add <tracker> && git commit && git push`).
3. Then start writing. `[~]` rows flip to `[x]` automatically as `unique: true` content ships.
4. On finish/abandon, un-claim (e.g. `... --unclaim`).

`[~]` means **currently being worked on only** — never pre-claim the whole backlog.

## Phase 1.6 / region order — locations first, region page LAST

For every region: write **all location pages (each location page + its money pages) first**,
one location at a time, and only write the **region page last**. The region page links down to
every location and summarizes the whole region, so it can't be written meaningfully until its
locations exist. A region is done only when all location pages + all money pages + the region
page carry `unique: true`.

## Phase 2 — Sync tracker

Run the tracker generator (e.g. `npm run gen:tracker`) to regenerate the tracker from the
content folder. Rows are `[x]` only when files exist with `unique: true`. **Never hand-edit
the tracker.** Re-run after each location/region batch.

## Phase 2.5 — Per-location todo list (for a region batch)

Enumerate **one todo item per location** (`{REGION} location: {slug} ({N} money + page)`),
pulled from the project's location data filtered by region. Mark a location complete only when
its page + all its money pages exist with `unique: true`. Keep a trailing "write {Region}
region page after all locations" todo plus a final tracker-sync todo.

## Phase 3 — The writing loop

1. Find the first `[ ]`/`[~]` entry top-to-bottom.
2. Write that page to the content folder per the content-writing skill:
   - Money → `{content}/money/{region}/{location}/{service}.md`
   - Location → `{content}/locations/{region}/{location}.md`
   - Region → `{content}/regions/{region}.md`
   - Hub → `{content}/hubs/{service}.md`
   - Core → `{content}/core/{page}.md`
3. Move to the next entry. **Do not stop, do not ask permission.**
4. Re-run the tracker generator after each location/region batch.

## The only times you stop

- Every tracker entry is `[x]` → report completion.
- The user interrupts → stop and report where you left off.
- A blocking error (build fails, data invalid) → report and ask how to proceed.

## Completion rules

| Thing | Done when |
|---|---|
| A money page | its file exists with `unique: true` |
| A location | location page + all money pages `unique: true` |
| A region | region page + every location complete |
| Tracker row | `[x]`, derived automatically from the filesystem |

## Key rules

- Never hand-edit the tracker; always regenerate.
- Never rewrite `[x]` pages.
- Always write directly to the content folder — no intermediate files, no copy-paste.
- Every page must pass the content-writing quality gates.
- The `unique: true` line must sit on its own line in frontmatter (build gate regex
  `/^unique:\strue\s*$/m` is line-ending-agnostic).
