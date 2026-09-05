# Changelog

All notable changes to this skills hub are documented here.
Format follows [Keep a Changelog](https://keepachangelog.com/). The repo is versioned by git
(no release tags yet — everything is `[Unreleased]` until the first tag).

## [Unreleased]

### Added
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
- Generalized the original water-damage-specific skills into reusable `programmatic-seo-*`
  skills — brand, domain, service list, and analytics ID replaced with `{placeholders}` plus a
  "read the project config" step.

### Notes
- `docs/` holds the original water-damage reference specs the skills were distilled from (kept
  as a sample niche for traceability).
