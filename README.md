# My Skills

A personal **skills hub** — reusable [OpenCode](https://opencode.ai) skills that install into
any project (or globally) with the official [`skills`](https://github.com/vercel-labs/skills)
CLI. This repo is the single source of truth; it's pure Markdown (no build, no dependencies).

The skills encode a repeatable system for building **programmatic local-SEO / rank-and-rent
lead-gen sites** (any service niche). They were distilled from the reference specs in [`docs/`](docs).

## Skills

| Skill | What it does |
|---|---|
| [`programmatic-seo-autopilot`](skills/programmatic-seo-autopilot/SKILL.md) | Orchestrator: write every missing page by hand, one at a time, honoring the tracker (claim a region → locations first → region page last). |
| [`programmatic-seo-content-writing`](skills/programmatic-seo-content-writing/SKILL.md) | Content standard: ≥90% uniqueness, entity floors, expert voice, markdown/UI formatting, quality gates. |
| [`programmatic-seo-technical-seo`](skills/programmatic-seo-technical-seo/SKILL.md) | Technical & on-page SEO: URLs, titles/meta, JSON-LD matrix, sitemaps, Core Web Vitals, local SEO. |
| [`programmatic-seo-keyword-architecture`](skills/programmatic-seo-keyword-architecture/SKILL.md) | Keyword patterns per page type, tiers, and the related-services internal-link map. |
| [`programmatic-seo-service-catalog`](skills/programmatic-seo-service-catalog/SKILL.md) | Model the service/subservice list, categories, routing, hub/money specs, batch order. |
| [`programmatic-seo-location-model`](skills/programmatic-seo-location-model/SKILL.md) | Geography hierarchy, demand filter, and content-gated publishing rules. |
| [`programmatic-seo-data-packs`](skills/programmatic-seo-data-packs/SKILL.md) | Gather verified per-location local facts before writing. |
| [`programmatic-seo-design-system`](skills/programmatic-seo-design-system/SKILL.md) | Theme-able premium UI system: tokens, typography, components, accessibility. |
| [`programmatic-seo-deployment`](skills/programmatic-seo-deployment/SKILL.md) | Static build = publish (content gate), delivery checks, upload, cache headers. |

## Install

Requires [Node.js](https://nodejs.org) (for `npx`). No `npm install` needed — the CLI runs on demand.

```bash
# All skills, globally (available in every project)
npx skills add 2eerr/my-skills -g -a opencode

# Specific skills only
npx skills add 2eerr/my-skills --skill programmatic-seo-autopilot --skill programmatic-seo-content-writing -g -a opencode

# Into the current project only (writes to .agents/skills/, commit it with that project)
npx skills add 2eerr/my-skills -a opencode

# Preview what's in the repo without installing
npx skills add 2eerr/my-skills --list
```

> **Private repo:** the CLI reuses your existing git auth (Windows Credential Manager / `gh` /
> SSH), so no extra login is required.

## Update

After the maintainer edits and pushes new versions:

```bash
npx skills update -g -y     # refresh all global skills
npx skills update -p -y     # refresh project-scoped skills
npx skills list -g          # see what's installed
```

Updates take effect in **new** OpenCode sessions (no hot-reload).

## Contribute / edit

```bash
git clone https://2eerr@github.com/2eerr/my-skills.git
cd my-skills
# edit or add skills under skills/<name>/SKILL.md
git add -A && git commit -m "..." && git push
```

### Skill file rules
- One folder per skill: `skills/<name>/SKILL.md` (folder name **must** equal the frontmatter `name`).
- `name`: 1–64 chars, lowercase alphanumeric + single hyphens (`^[a-z0-9]+(-[a-z0-9]+)*$`).
- `description`: 1–1024 chars, specific — the agent uses it to decide when to load the skill.
- Recognized frontmatter only: `name`, `description`, `license`, `compatibility`, `metadata`.
- File must be exactly `SKILL.md` (all caps).
- Validate locally: `npx skills add . --list`.

## Repo layout

```
my-skills/
├── README.md
├── plan.md            # the hosting/CLI decision record
├── docs/              # reference specs the skills were distilled from (a sample niche)
└── skills/            # the installable skills (auto-discovered by the CLI)
    └── programmatic-seo-*/SKILL.md
```

## Notes
- **Windows symlinks:** the CLI symlinks skills by default; if symlinks are blocked, add `--copy`
  (then re-run `npx skills update` after pushes instead of it being instant).
- **Telemetry:** the CLI sends anonymous usage data; disable with `DISABLE_TELEMETRY=1`.
- **Other agents:** swap `-a opencode` for `claude-code`, `cursor`, etc., or `-a '*'` for all.
