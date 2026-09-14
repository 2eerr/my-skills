# Changelog

All notable changes to this skills hub are documented here.
Format follows [Keep a Changelog](https://keepachangelog.com/). The repo is versioned by git
(no release tags yet — everything is `[Unreleased]` until the first tag).

## [Unreleased]

### Added
- **`agents-md` skill** — create, complete, and audit a repo's `AGENTS.md` (the always-loaded
  AI operating file): full file anatomy (what-it-is, reading protocol/routing table, commands,
  conventions, git rules, docs-sync rule, quality gates, `x!` shorthand triggers,
  knowledge-base scope rules), a per-repo config table to **derive every section from the
  repo's real workflow** (never transplant from another repo), CREATE / COMPLETE / AUDIT
  procedures, and the design rules — always-loaded means lean, triggers live in `AGENTS.md`
  not in skills, one owner per rule, docs are a knowledge base to flag conflicts against
  (never blindly execute).
- **docs/AGENTS.md conversion** (target-project operating file): concrete build-performance
  mechanisms — lazy frontmatter reads, stat-validated content-index cache, scope-first
  completeness, memoized/Map lookups, shared markdown processor, worker_threads passes,
  Tailwind `@source` scoping, Node LTS (→ `astro-ssg` §7); named-rest-param + `temp/` +
  package.json/copy rules (→ `astro-ssg`); multi-PC sync + never-auto-destructive git +
  optional `dev` staging variant (→ `linear-git-workflow`); mobile check, a11y pass,
  deploy-zip sync rule (→ `seo-deployment`); docs-are-knowledge-base + 5 follow-up
  suggestions (→ `seo-autopilot`); no-invented-hexes (→ `seo-design-system`); one-doc-per-
  purpose (→ `seo-site-blueprint`).
- **Docs↔skills audit fixes:** added the missing coverage — E-E-A-T & off-page rules (→
  `seo-local-seo`), data-validation + never-hand-edit-JSON regeneration rules (→
  `seo-location-model`), responsive breakpoints + micro-interaction/reduced-motion rules (→
  `seo-design-system`), the per-page author-prompt skeleton and corrected core-page word-count
  ranges (→ `seo-content-writing`). De-duplicated cross-skill bleed so each rule has one
  owner: keyword caps → keyword-architecture; page paths + strict content rule →
  content-writing (autopilot enforces via pointers); word/entity/schema specs + publish gating
  → content-writing/schema/location-model (service-catalog points); font + motion mechanics →
  design-system (technical/astro point); sitemap regen task → astro-ssg; OG/canonical rule
  statements → on-page/technical (astro-ssg keeps only Base.astro wiring).
- **`seo-site-blueprint` skill** — the phased build methodology (from the reference plan doc):
  four strategy pillars, scope discipline + page-inventory math, Phases 0–8 each with a review
  gate (data → scaffold → design → core → pilot region → batched rollout → SEO layer →
  QA/launch → post-launch), and the per-region batch checklist.
- **`astro-ssg` §7 "Build performance at scale"** — light templates, single CSS bundle,
  SVG-first imagery, per-region sub-builds, sitemap-only regeneration, post-build passes on
  every build variant (build-time guidance from the reference docs; runtime CWV stays in
  `seo-technical-seo`).
- **`astro-ssg` skill** — all Astro-only instructions consolidated from the reference docs:
  scaffold/config, `getStaticPaths()` data-driven routes, `Base.astro` head wiring, content-file
  rendering + gates, component-first rule, `dist/` output quirks, post-build passes.
  `seo-deployment`'s SSG note now points to it; autopilot sibling list updated.
- **SEO skill split** — `seo-technical-seo` was "technical & on-page"; its content
  is now four focused skills: `seo-onpage-seo` (titles/meta/canonical/headings/
  internal links/link equity/OG), `seo-technical-seo` (URL architecture — sole
  owner of the trailing-slash scheme — sitemaps, robots, CWV, indexability, analytics, check
  script), `seo-local-seo` (local signals, GBP/citations, NAP, local KPIs), and
  `seo-image-seo` (self-hosting, SVG/WebP, width/height+alt+lazy, OG card,
  licensing/attribution — consolidated from the SEO + design docs; design-system now points to
  it). Autopilot sibling list, schema/design-system cross-refs, README, and AGENTS updated.
- **`seo-schema` skill** — all JSON-LD instructions in one place: base graph,
  per-page-type schema matrix, `@id` conventions, FAQPage-from-own-content-only,
  never-fabricate-reviews, LocalBusiness entity + NAP consistency, validation.
  `seo-technical-seo` §9 now points to it (no duplicated matrix); the autopilot
  sibling list and README updated.
- **`tracker` skill** — the TRACKER.md file format and generator-script contract:
  exact section layout (summary table, legend, core/hubs/regions/money pages with
  nested per-region sub-headings), status symbol derivation rules, per-line format
  specs, frontmatter `unique: true` check contract, `[~]`/`[!]` preservation across
  regenerations, and the content-folder structure the script reads.
- **`source/` directory** — consolidated all project reference files into `source/`
  (gitignored): `source/docs/` (domain manuals + content pipeline), `source/scripts/`
  (build, validation, and generation scripts), `source/temp/` (ad-hoc throwaway
  helpers). Updated all skill path references from `scripts/`→`source/scripts/`,
  `temp/`→`source/temp/`, `docs/`→`source/docs/` across 7 skills.
- **Script references across skills** — added concrete `source/scripts/` and `source/temp/` path
  references to 7 skills: `tracker` (gen-pages-tracker, claim-state), `seo-autopilot`
  (claim-state, gen-pages-tracker, check-seo, check-wordcounts), `seo-deployment`
  (full build pipeline: prebuild-check, build, add-fetchpriority, generate-sitemaps,
  validate-data, check-seo, check-wordcounts, gen-pages-tracker, package-deploy),
  `seo-technical-seo` (check-seo, generate-sitemaps), `seo-location-model`
  (validate-data, gen-locations), `astro-ssg` (full build pipeline), `seo-content-writing`
  (check-wordcounts, check-seo, gen-pages-tracker).
- **`ii!` shorthand command** — implement + record: make the code/content changes AND
  update the relevant `.md` files so the new behavior is documented. Opposite of `i!`
  (instruction only, no code changes).
- **`linear-git-workflow` skill** — the repo's git discipline generalized into a reusable,
  agent-agnostic skill: never commit to the default branch, `<type>/<kebab-slug>` feature
  branches, descriptive imperative commits, integrate via rebase + `git merge --ff-only`
  (linear history, no merge commits), then push; includes the commit/publish procedures,
  commands, checklist, and force-push gotchas.
- **Skills (9):** `seo-autopilot`, `seo-content-writing`,
  `seo-technical-seo`, `seo-keyword-architecture`,
  `seo-service-catalog`, `seo-location-model`,
  `seo-data-packs`, `seo-design-system`,
  `seo-deployment` — a niche-agnostic system for building
  local-SEO / rank-and-rent lead-gen sites.
- **`README.md`** — overview, skills table, install/update, the full `skills` CLI command
  reference, environment variables, and npm-package attribution.
- **`AGENTS.md`** — AI working instructions: golden rules, skill-file spec, commands,
  add/update/remove workflows, conventions, and trigger phrases (`git!`, `push!`, `docs!`, `i!`).
- **`templates/SKILL.template.md`** — scaffold for new skills.
- **`CHANGELOG.md`** — this file.
- **Git conventions** — feature-branch workflow, Conventional-Commits branch naming
  (`<type>/<kebab-slug>`), and a strictly linear history (rebase + `--ff-only`, no merge commits).
- **Evals (6 skills):** `seo-autopilot`, `seo-content-writing`, `seo-technical-seo`,
  `seo-schema`, `seo-deployment`, `tracker` — each with 3 realistic test prompts in
  `evals/evals.json` for benchmarking and quality verification.
- **source/ → skill relationship** documented in `AGENTS.md` (Golden rules subsection) —
  `source/` files are niche-specific reference material; skills extract the general pattern
  with `{placeholders}`, never carry niche-specific bindings.

### Changed
- **Windows install docs** — README + AGENTS.md now document the PowerShell/cmd way to set
  `INSTALL_INTERNAL_SKILLS` (the `VAR=1 cmd` prefix is bash-only; PowerShell needs
  `$env:INSTALL_INTERNAL_SKILLS = "1"` first). A plain `npx skills add 2eerr/my-skills` without
  the variable reports "No skills found" because every skill is `internal: true`. `seo-technical-seo`, `seo-schema`, `seo-deployment`,
  `astro-ssg`, `tracker`, `linear-git-workflow` — shortened to ≤300 chars while keeping
  trigger specificity (skill-creator industry standard).
- **`seo-keyword-architecture`** — added "When to use it" section with 4 trigger contexts
  (keyword selection, internal-link map, density audit, cross-service keywords).
- **`seo-onpage-seo` §3** — clarified that the canonical trailing-slash scheme is owned
  by `seo-technical-seo`; this section covers only the tag itself.
- **Removed `skills/skill-creator/`** — external dependency from `anthropics/skills`,
  not a custom skill; lives in `.agents/skills/skill-creator/` (CLI install location).
- **`source/` description** updated in `AGENTS.md` Layout + `README.md` Layout — now
  reads "niche-specific reference files from a real implementation (water damage site)".
- **`source/docs/TRACKER.md`** — removed duplicate legend from Money Pages section
  (top-of-file legend already covers status symbols).
- Renamed all SEO skills from `programmatic-seo-*` to `seo-*` (folders + frontmatter) and
  removed the "Programmatic SEO" wording from every title, description, and doc — the system is
  now branded simply `seo-*` alongside `linear-git-workflow`.
- `docs/` is now **local-only**: added to `.gitignore`, untracked from git, purged from git
  history (filter-repo + force-push), and all README/AGENTS references reworded — the files stay
  on the maintainer's disk only.
- Deleted `plan.md` — its unique content moved to `AGENTS.md` (new "Decisions (locked)" section:
  official `skills` CLI only, GitHub as source of truth, public repo + internal skills; CLI
  discovery rules) and `README.md` (new-machine propagate note, layout + validate fixes).
- Generalized docs from OpenCode-only to **multi-agent** use (primary: OpenCode + Freebuff):
  README + `AGENTS.md` install commands now use `-a '*'` / explicit agent lists, a Freebuff
  workaround is documented (universal `.agents/skills/` folder + `AGENTS.md` pointer), and a new
  golden rule keeps skills agent-agnostic. README install commands now carry
  `INSTALL_INTERNAL_SKILLS=1` (required since the skills went internal).
- Marked all 9 skills `metadata.internal: true` — hidden from `skills` CLI discovery and
  normal installs (install with `INSTALL_INTERNAL_SKILLS=1`), so they no longer register on
  skills.sh via install telemetry; GitHub stays public. `AGENTS.md` + the skill template now
  require the flag on every skill, and `DISABLE_TELEMETRY=1` is a documented maintainer setting.
- Generalized the original water-damage-specific skills into reusable `seo-*`
  skills — brand, domain, service list, and analytics ID replaced with `{placeholders}` plus a
  "read the project config" step.

### Notes
- `docs/` holds the original water-damage reference specs the skills were distilled from — kept
  on maintainer machines only (gitignored, never pushed).
