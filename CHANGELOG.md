# Changelog

All notable changes to this skills hub are documented here.
Format follows [Keep a Changelog](https://keepachangelog.com/). The repo is versioned by git
(no release tags yet — everything is `[Unreleased]` until the first tag).

## [Unreleased]

### Added
- **SEO skill split** — `programmatic-seo-technical-seo` was "technical & on-page"; its content
  is now four focused skills: `programmatic-seo-onpage-seo` (titles/meta/canonical/headings/
  internal links/link equity/OG), `programmatic-seo-technical-seo` (URL architecture — sole
  owner of the trailing-slash scheme — sitemaps, robots, CWV, indexability, analytics, check
  script), `programmatic-seo-local-seo` (local signals, GBP/citations, NAP, local KPIs), and
  `programmatic-seo-image-seo` (self-hosting, SVG/WebP, width/height+alt+lazy, OG card,
  licensing/attribution — consolidated from the SEO + design docs; design-system now points to
  it). Autopilot sibling list, schema/design-system cross-refs, README, and AGENTS updated.
- **`programmatic-seo-schema` skill** — all JSON-LD instructions in one place: base graph,
  per-page-type schema matrix, `@id` conventions, FAQPage-from-own-content-only,
  never-fabricate-reviews, LocalBusiness entity + NAP consistency, validation.
  `programmatic-seo-technical-seo` §9 now points to it (no duplicated matrix); the autopilot
  sibling list and README updated.
- **`linear-git-workflow` skill** — the repo's git discipline generalized into a reusable,
  agent-agnostic skill: never commit to the default branch, `<type>/<kebab-slug>` feature
  branches, descriptive imperative commits, integrate via rebase + `git merge --ff-only`
  (linear history, no merge commits), then push; includes the commit/publish procedures,
  commands, checklist, and force-push gotchas.
- **Skills (9):** `programmatic-seo-autopilot`, `programmatic-seo-content-writing`,
  `programmatic-seo-technical-seo`, `programmatic-seo-keyword-architecture`,
  `programmatic-seo-service-catalog`, `programmatic-seo-location-model`,
  `programmatic-seo-data-packs`, `programmatic-seo-design-system`,
  `programmatic-seo-deployment` — a niche-agnostic system for building programmatic
  local-SEO / rank-and-rent lead-gen sites.
- **`README.md`** — overview, skills table, install/update, the full `skills` CLI command
  reference, environment variables, and npm-package attribution.
- **`AGENTS.md`** — AI working instructions: golden rules, skill-file spec, commands,
  add/update/remove workflows, conventions, and trigger phrases (`git!`, `push!`, `docs!`, `i!`).
- **`templates/SKILL.template.md`** — scaffold for new skills.
- **`CHANGELOG.md`** — this file.
- **Git conventions** — feature-branch workflow, Conventional-Commits branch naming
  (`<type>/<kebab-slug>`), and a strictly linear history (rebase + `--ff-only`, no merge commits).

### Changed
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
- Generalized the original water-damage-specific skills into reusable `programmatic-seo-*`
  skills — brand, domain, service list, and analytics ID replaced with `{placeholders}` plus a
  "read the project config" step.

### Notes
- `docs/` holds the original water-damage reference specs the skills were distilled from — kept
  on maintainer machines only (gitignored, never pushed).
