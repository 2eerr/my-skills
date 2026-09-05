# Plan: Skills Hub on GitHub + official `npx skills` CLI

## Decision

- **CLI**: Use ONLY the official `npx skills` CLI (vercel-labs/skills, the tool behind skills.sh). No custom scripts, no junctions, no install.ps1.
- **Hosting**: Skills live in a GitHub repo (single source of truth), versioned with git.
- **Target agent**: OpenCode (agent id `opencode`). The CLI also supports Claude Code, Cursor, etc. if needed later.

---

## Goal

Maintain custom skills in one GitHub repo, install them into any project (or globally) with one command, and refresh everywhere with `npx skills update`.

---

## How it works

```
GITHUB: github.com/<you>/skills-hub          <- SOURCE OF TRUTH
        │
        │  edit + git commit + git push
        ▼
local clone: D:\LDrive\34\my skills\
├── plan.md
└── skills\                                  <- CLI auto-discovers this folder
    ├── my-skill-one\
    │   └── SKILL.md
    ├── my-skill-two\
    │   └── SKILL.md
    └── ...
        │
        │  npx skills add <you>/skills-hub  (installs/symlinks)
        ▼
OpenCode locations:
  global  ->  C:\Users\Hammad\.config\opencode\skills\   (all projects)
  project ->  <project>\.agents\skills\                  (one project, committed w/ repo)
```

- `npx skills add` records the source repo; `npx skills update` re-pulls the latest version from GitHub.
- Default install = **symlink** to one canonical copy (single source of truth). `--copy` fallback if symlinks unavailable.
- Private repos work: the CLI reuses your git credentials / `gh` auth / SSH.

---

## Command cheat-sheet (what you get)

| Task | Command |
|---|---|
| Install ALL your skills globally (all projects) | `npx skills add <you>/skills-hub -g -a opencode` |
| Install specific skills globally | `npx skills add <you>/skills-hub --skill my-skill-one --skill my-skill-two -g -a opencode` |
| Install into current project only | `npx skills add <you>/skills-hub -a opencode` |
| Install from full URL | `npx skills add https://github.com/<you>/skills-hub --skill my-skill-one` |
| Update everything | `npx skills update -g -y` (global) / `npx skills update -p -y` (project) |
| Update one skill | `npx skills update my-skill-one -y` |
| List installed | `npx skills list` / `npx skills ls -g` |
| Remove | `npx skills remove <name>` / `npx skills remove --all -g` |
| Search community skills | `npx skills find <query>` |
| New skill template | `npx skills init <name>` |
| Preview skills in a repo | `npx skills add <you>/skills-hub --list` |

---

## Day-to-day workflow

| Action | What you do |
|---|---|
| Edit a skill | Edit `skills\<name>\SKILL.md` in the clone → `git add -A; git commit; git push` |
| Propagate to this machine | `npx skills update -g -y` (or `npx skills update <name> -y`) |
| Propagate to another machine | `npx skills add <you>/skills-hub -g -a opencode` once, then `npx skills update -g -y` afterwards |
| Add a new skill | `npx skills init my-new-skill` → move into `skills\` → push → `npx skills add ... --skill my-new-skill -g -a opencode` |
| Use in a specific project only | run `npx skills add <you>/skills-hub --skill <name> -a opencode` inside that project (installs to `.agents\skills\`, commit it with the project) |

Note: updates take effect in **new opencode sessions** (no hot-reload).

---

## Implementation Steps (to execute ONLY when user says "implement")

1. **Repo structure**: create `skills\` folder in `D:\LDrive\34\my skills\` + one starter skill (`skills\example-skill\SKILL.md` with valid frontmatter).
2. **Git**: `git init`, `.gitignore`, initial commit.
3. **GitHub**: create repo `<you>/skills-hub` (private recommended) and push — via `gh repo create skills-hub --private --source . --push` if `gh` is available, else manual remote + push (will ask you for the repo URL).
4. **Install**: run `npx skills add <you>/skills-hub -g -a opencode` (global → all projects).
5. **Verify**: open opencode in any project → skill appears in `<available_skills>`; run `npx skills list -g`; edit skill → push → `npx skills update -g -y` → confirm change.
6. **Docs**: short `README.md` in the repo with the commands above.

## Deliverables when "implement" is said

- [ ] `skills/` folder + starter skill
- [ ] Git repo initialized + pushed to GitHub
- [ ] Global install via `npx skills add` (symlink or `--copy` fallback)
- [ ] Verification + README

---

## Skill file rules (OpenCode + CLI compatible)

- One folder per skill; folder name == `name` in frontmatter.
- `name`: 1–64 chars, lowercase alphanumeric + single hyphens, regex `^[a-z0-9]+(-[a-z0-9]+)*$`.
- `description`: 1–1024 chars, specific (agent uses it to decide when to load the skill).
- Recognized frontmatter only: `name`, `description`, `license`, `compatibility`, `metadata`.
- File must be exactly `SKILL.md` (all caps).
- CLI discovery: skills found under `skills/` (flat or up to 2 category levels deep), repo root, or common agent dirs — our `skills/<name>/SKILL.md` layout is the standard one.

---

## Constraints & notes

- **Windows symlinks**: may require Developer Mode; if `npx skills add` can't symlink, use `--copy` (then updates require `npx skills update` instead of being instant).
- **GitHub auth** once per machine: `gh auth login`, or SSH key, or git credential helper.
- **Private repo**: fine — CLI uses existing auth. Public repo = shareable with everyone + listed potential on skills.sh.
- **Telemetry**: CLI sends anonymous usage data; disable with `DISABLE_TELEMETRY=1`.
- **Global vs project scope**: global (`-g`) = all projects instantly; project = per-project, committed with that project's repo (good for teammates).
- **Naming collisions**: skill names must be unique across global + project locations.

---

## Status

- [x] Plan written
- [x] Decision locked: GitHub repo + official `npx skills` CLI only (no custom scripts)
- [ ] Implementation (waiting for user's explicit "implement" command)
