---
name: water-damage-content-autopilot
description: Run the "Water Damage Pros US" content autopilot — write every missing site page (core, service hubs, state, city, and city×service money pages) by hand, one at a time, honoring the tracker, claiming a state before writing it, and writing all of a state's cities before its state page. Use when the user references the start file or asks to generate/write content pages for the water damage restoration site.
---

# Water Damage Content Autopilot

Turns you into a content autopilot for a national rank-and-rent water damage restoration
site (brand **Water Damage Pros US**, domain `waterdamageprosus.com`). You write every
missing page directly to `src/content/`, one at a time, without stopping for permission,
until the tracker is all `[x]`.

This skill is the **orchestrator**. Detailed specs live in the sibling skills:
`water-damage-content-writing`, `water-damage-seo`, `water-damage-keywords`,
`water-damage-services`, `water-damage-locations`, `water-damage-data-packs`,
`water-damage-design-system`, `water-damage-deployment`.

## Hard rule — no templates, no scripts, no copy-paste

- Write every page **individually and by hand** into its own `.md` file. No reusable
  skeleton, no fill-in-the-blank outline, no shared block.
- **Scripts may build routes, assemble data, and verify output — never write page words.**
- No sentence, paragraph, list item, table, or FAQ may be reused from any other page.
- Every page must be **≥90% unique** from every other page on the site.
- If you are about to reuse text you already wrote — **stop and rewrite**.

## Phase 1 — Gather context (every time)

1. Read all reference docs (`docs/*.md`) or load the sibling skills above.
2. Scan `src/content/` subfolders (`core/`, `hubs/`, `states/`, `cities/`, `money/`) to see
   what already exists.

## Phase 1.5 — Only write unwritten pages

Read `docs/TRACKER.md` first and honor statuses. Never clobber parallel work.

| Status | Meaning | Action |
|---|---|---|
| `[ ]` | missing / not started | writable |
| `[~]` | in progress (claimed, no unique content yet) | yours to write |
| `[x]` | done (unique content exists) | never touch |
| `[!]` | exists but not unique (flagged for rework) | only rewrite if user explicitly asks |

If the user asks for a page that already has content, **do not rewrite it** — warn them it
is already written (with its status) and ask before overwriting.

## Phase 1.7 — Claim a state before writing it

So other machines (synced via GitHub) don't duplicate work:

1. `node temp/claim-state.mjs <state-slug>` — flips the state + its city/money rows to `[~]`.
2. `git add docs/TRACKER.md && git commit -m "chore: claim state <state> as in-progress" && git push origin dev`
3. Then start writing. `[~]` rows flip to `[x]` automatically as `unique: true` content ships.
4. On finish/abandon: `node temp/claim-state.mjs <state-slug> --unclaim`.

`[~]` means **currently being worked on only** — never pre-claim the whole backlog.

## Phase 1.6 / state order — cities first, state page LAST

For every state: write **all city pages (each city page + its 39 money pages) first**, one
city at a time, and only write the **state page last**. The state page links down to every
city and summarizes the whole state, so it cannot be written meaningfully until its cities
exist. A state is done only when all city pages + all money pages + the state page carry
`unique: true`.

## Phase 2 — Sync tracker

`npm run gen:tracker` regenerates `docs/TRACKER.md` from `src/content/`. Rows are `[x]` only
when files exist with `unique: true`. **Never hand-edit the tracker.** Re-run after each
city/state batch.

## Phase 2.5 — Per-city todo list (for a state batch)

Enumerate **one todo item per city** (`{ST} city: {city-slug} (39 money + city page)`),
pulled from `src/data/cities.json` filtered by state. Mark a city complete only when its
city page + all 39 money pages exist with `unique: true`. Keep a trailing "write {State}
state page after all cities" todo plus a final tracker-sync todo.

## Phase 3 — The writing loop

1. Find the first `[ ]`/`[~]` entry top-to-bottom.
2. Write that page to `src/content/` per the content-writing skill:
   - Money → `src/content/money/{state}/{city}/{service}.md`
   - City → `src/content/cities/{state}/{city}.md`
   - State → `src/content/states/{state}.md`
   - Hub → `src/content/hubs/{service}.md`
   - Core → `src/content/core/{page}.md`
3. Move to the next entry. **Do not stop, do not ask permission.**
4. Re-run `npm run gen:tracker` after each city/state batch.

## The only times you stop

- Every tracker entry is `[x]` → report completion.
- The user interrupts → stop and report where you left off.
- A blocking error (build fails, data invalid) → report and ask how to proceed.

## Completion rules

| Thing | Done when |
|---|---|
| A money page | its `.md` exists with `unique: true` |
| A city | city page + all 39 money pages `unique: true` |
| A state | state page + every city complete |
| Tracker row | `[x]`, derived automatically from the filesystem |

## Key rules

- Never hand-edit `TRACKER.md`; always regenerate.
- Never rewrite `[x]` pages.
- Always write directly to `src/content/` — no intermediate files, no copy-paste.
- Every page must pass the content-writing quality gates.
- Updates take effect in new sessions; the `unique: true` line must sit on its own line in
  frontmatter (the build gate regex is `/^unique:\strue\s*$/m`).
