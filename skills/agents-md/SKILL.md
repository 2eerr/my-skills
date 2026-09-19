---
name: agents-md
description: "Create, complete, and audit a repo's AGENTS.md — the always-loaded AI operating file. Full anatomy (overview, reading protocol, commands, conventions, git rules, docs-sync, quality gates, shorthand triggers), deriving every section from the repo's real workflow instead of transplanting templates, and auditing an existing file for staleness against the current code. Use when onboarding a repo, writing or expanding an AGENTS.md, or after workflow changes that may have staled it."
metadata:
  internal: true
---

# AGENTS.md — the repo's AI operating file

`AGENTS.md` is the **always-loaded** instruction file that AI coding agents read at the
start of every session. Because it is loaded every time (unlike skills, which load
on-demand), it is the right home for anything the agent must know **before** any topic
comes up: the project map, hard rules, commands, and trigger phrases.

The craft: an AGENTS.md must describe the repo **as it actually is** — derived from the
real workflow, never transplanted from another repo or a generic template.

## When to use it

- Setting up an `AGENTS.md` for a repo that has none.
- Completing or expanding an existing one (a section is missing or thin).
- Auditing for staleness after the workflow changed (branch model, scripts, doc layout).
- The user references "agents instructions", "AI rules file", or similar.
- **Not for:** executing triggers defined in the file (follow them directly), or the
  trigger-table mechanics specifically (see `agents-shorthand` for that — this skill
  covers the table's place in the file; `agents-shorthand` owns its derivation).

## Project config (read per repo, derive — never transplant)

Everything in the file must come from the repo's real setup:

| What | Where to read it |
|---|---|
| Language/runtime, framework, package manager | manifest (`package.json`, `pyproject.toml`, …), lockfile |
| Build / dev / test / lint commands | manifest `scripts`, CI config, Makefile |
| Branch model (single protected default vs default+integration) | `git branch -vv`, remote HEAD |
| Remote name + push policy (which branches ever get pushed) | `git remote -v` + AGENTS.md/CI |
| Branch-naming + commit conventions | git history (`git log --oneline`) |
| Docs layout (`docs/`, root `*.md`, wiki) | repo tree |
| Quality gates (typecheck, lint, tests, custom check scripts) | scripts + CI |
| Deploy target/flow | CI, docs, hosting config |

## File anatomy (what a complete AGENTS.md contains)

Include a section only when the repo actually has that concern. Order matters — most
important for orientation first:

1. **What this repo is** — 2–4 lines: product/purpose, stack, the single most important
   architectural fact. The agent reads this before every task.
2. **Reading protocol / routing table** — "if you are going to touch X, read Y first."
   Map task domains to docs/files, so the agent loads the right context before acting.
   Include the action rule: read → act → update the doc if behavior changed.
3. **Documentation map** — a table listing every doc file, what it covers, and when to
   read it. This is the agent's quick reference for navigating the project's knowledge base.
4. **Commands** — the real commands: install, dev, build, test, lint/check, deploy.
   One table, with gotchas inline (slow cold start, required env, ordering).
5. **Directory structure** — what lives where: src/, scripts/, docs/, public/, temp/.
   Include the .gitignore rules if they affect workflow.
6. **Conventions** — code style, file naming, component rules, "no X in Y" prohibitions.
   Only rules that are actually enforced or matter here; not generic style advice.
7. **Git rules** — branch model, branch naming, commit-message format, what never happens
   without the user asking (commit/push/merge/reset), conflict policy.
8. **GitHub sync / multi-PC rules** — pull at session start, push at session end, conflict
   resolution policy, pinned remote URL for credential manager.
9. **Docs-sync rule** — code and docs must not drift: change code → update the matching
   doc in the same change. Name which files are synced to what.
10. **Quality gates / delivery gate** — what must pass before work is "done" (build,
    tests, checks, review steps).
11. **Shorthand trigger commands** — single-token `x!` phrases the user types **alone**.
    Own only the table here; derive rows per repo-specific workflow.
12. **Context-gathering / scope rules** — how the agent should treat the file: read
    relevant docs before acting, docs are a knowledge base (flag conflicts, don't blindly
    execute), what "gather context" vs "take action" means if the repo defines it.
13. **Post-task follow-up** — after finishing any task, propose 5 contextually relevant
    next steps drawn from the project's current state. Suggestions only, never performed
    unprompted.
14. **SEO checklist** (if applicable) — per-page requirements: title, meta, canonical,
    OG/Twitter, JSON-LD, internal links, images.
15. **Data rules** (if applicable) — validation requirements, field definitions, sources.
16. **Build & verify** (if applicable) — step-by-step build process, batch quality bar,
    verification workflow, and deployment gate.
17. **Build performance** (if applicable) — mechanisms that keep builds fast, with
    "don't revert" reasons for each.

## Hard rules for writing the file

1. **Derive, never transplant.** Every section comes from the repo's real workflow. A
   section copied from a different repo can contradict this repo's rules (branch model,
   push policy, layout) — that is worse than a missing section.
2. **Placeholders over hardcoding.** Within any reusable fragment, use `{placeholders}`
   (`{Brand}`, `{default}`, `{remote}`) resolved from the project config — never bake in
   another project's names, paths, or IDs.
3. **Always-loaded means always-relevant.** Keep it lean; the file is re-read every
   session. Detailed procedures belong in skills/docs, referenced by pointer.
4. **One owner per rule.** A rule lives in exactly one section (or one doc it points to).
   Never state the same rule twice with different wording — drift starts there.
5. **Imperative, concrete, checkable.** "Run `npm run check:seo` before every build" —
   not "make sure things are good."
6. **Triggers stay here, not in skills.** A trigger typed alone has no topic for a skill
   description to match, so it must live in this always-loaded file.
7. **The file is a knowledge base, not an automation script.** Say so: when a doc
   conflicts with reality or the user's ask, the agent flags it — never mechanically
   executes an outdated instruction.
8. **Match the repo's existing tone/structure** if an AGENTS.md already exists — extend
   it, don't restyle it.
9. **English only.** The agent's responses and thinking must always be in English only —
   no other language in replies, summaries, commit messages, or any written output.
10. **One .md file per purpose.** Never scatter related instructions across many docs.
    Every doc covers one topic completely. Before creating a new .md, check whether the
    content already belongs in an existing doc.

## Procedure — CREATE (no AGENTS.md)

1. Read the project config above until you can state the repo's workflow in one paragraph.
2. Draft the anatomy sections that apply, each derived from that workflow.
3. Show the draft to the user (section list + any decisions taken), get approval.
4. Write `AGENTS.md`, commit on a feature branch per the repo's git rules.

## Procedure — COMPLETE (exists, gaps)

1. Inventory the existing file against the anatomy: list what's present, missing, thin.
2. Derive the missing sections from the project config (same as CREATE).
3. Show the user the proposed additions; on approval, **append in the file's existing
   style** — do not restyle existing sections.

## Procedure — AUDIT (staleness check)

1. For each section, verify claims against the repo as it is now:
   - Commands still exist and work (check manifest scripts; run nothing destructive).
   - Paths/dirs in the routing table still exist; docs listed still exist.
   - Branch model, remote, push policy match git reality.
   - Quality gates reference scripts that still exist.
   - Trigger rows still describe workflows the repo actually uses.
2. Check the structural rules: no duplicated rules across sections; nothing hardcoded
   that should be a placeholder; no section so long it belongs in a skill/doc.
3. Propose fixes per section; apply approved edits.
4. Report: sections verified, sections fixed, sections removed (with why).

## Context gathering pattern

When the user asks to "read" or "gather context" from the AGENTS.md (or similar),
the agent should:

1. Read the AGENTS.md file itself.
2. Read every `.md` file referenced in the documentation map.
3. **Gather context only** — do not take action, make changes, or execute commands
   unless explicitly asked.

The docs are a **knowledge base, not an automation script**. If a doc says "do X"
and the user asks for something different, or the code actually does something
different, the agent should flag the discrepancy and ask the user — never
mechanically execute an outdated or incorrect doc.

## Custom commands pattern

Define shorthand trigger commands as single-word `x!` phrases typed **alone** (as the
entire message). Each is never run automatically at any other time.

Example commands (derive from the repo's real workflow):

| Command | What it does |
|---|---|
| `git!` | Make a descriptive git commit (review working tree, stage, commit, remind to push) |
| `push!` | Full workflow: commit + merge branches + push to origin |
| `clean!` | Git cleanup: commit + merge all topic branches into dev + delete merged |
| `docs!` | Sync docs with code (update all .md files to match current state) |
| `ii!` | Implement requested changes AND record them in docs |
| `i!` | Add instruction only — no code changes |
| `sync!` | Two-way sync of main/dev with origin (pull + push) |

Rules for custom commands:
- Never run automatically unless the user types the exact command
- After `git!`, remind user to push so other PCs can sync
- `push!` is explicit — pushes without asking
- `docs!` scope is limited to docs/ plus README.md — only when they need updating

## Delivery gate pattern

Before any phase or batch is "done," verify:

1. Build succeeds and page count matches expectations.
2. Lint/check scripts pass with zero errors.
3. Random sample (5 pages) reviewed for non-thin content + valid links.
4. UI/UX presentation pass — pages render with real markdown structure.
5. Accessibility pass — no prohibited ARIA, contrast ≥4.5:1, keyboard-operable.
6. Deploy zip in sync — any new build script is also in the packaging script's required list.

## Routing table pattern

If the repo has a complex doc surface, add a routing table near the top. Format:

| If you are going to… | Read these first |
|---|---|
| Deploy live | `deployment.md` |
| Modify core data flow | `architecture.md` |
| Change data quality | `data-quality.md` |
| Work on a skill | `AGENTS.md` + that skill |
| Anything else | `AGENTS.md` first, then the relevant skill |

Derive rows from the repo's actual docs and task domains. The rule: read the right
doc before acting; if a doc conflicts with reality, flag it.

## .gitignore section pattern

If the repo has a `.gitignore`, reference it in the file anatomy. State what it
covers (e.g. `node_modules/`, `.astro/`, `dist/`, `.env`) and whether the agent
should create or update `.gitignore` entries when adding new build artifacts or
temp directories.

## Hard git prohibitions pattern

Spell out what the agent must never do without explicit user instruction:

- **Never auto-commit** — always ask before committing
- **Never auto-push** — always ask before pushing
- **Never auto-merge** — always ask before merging branches
- **Never auto-rebase/reset** — always ask before destructive git operations
- **Never force push** — ask for confirmation
- **Never delete untracked files** without asking
- **Never add untracked files** without asking

State the conflict resolution policy (e.g. "always take local changes unless told otherwise").

## Build & verify pattern

If the repo has a multi-step build process, document the exact sequence:

1. **Build steps** — ordered commands (e.g. `node scripts/build-pages.js` →
   `node scripts/build-all.js`). State which steps are required and which are optional.
2. **Verify steps** — commands to validate the build succeeded (e.g. page count check,
   link check, HTML validation). State the expected outcomes.
3. **Batch quality bar** — after each batch, verify: content renders with real markdown,
   not placeholder; no broken links; no regressions from earlier batches; UI/UX check.
4. **Batch commit pattern** — name the convention (e.g. `content(batch-NN): …`).
5. **Deployment gate** — final validation before going live: page count, sitemap,
   build zip, link check, HTML validation, a11y spot-check.

## Deployment gate pattern (detailed)

If the repo deploys to production, specify:

1. **Validation before push** — what commands/tests must pass.
2. **Build packaging** — how to create the deploy artifact (zip, upload script).
3. **Push procedure** — step-by-step: build → package → verify → push → confirm URL.
4. **Post-push** — what to check on the live site (URL, page count, sitemap).
5. **Rollback** — how to revert if something breaks (git revert, push old build).

## Performance & scale pattern (detailed)

If the build generates many pages, document the performance mechanisms:

| Mechanism | Why it exists |
|---|---|
| Lazy reads (`lazy: true` in `read()`) | Load file metadata without reading content into memory upfront |
| `worker_threads` for multi-file writes | Parallel I/O across CPU cores — do not collapse back to sequential |
| No caching of `process.cwd()` etc. | Values may change; always read fresh |
| Bounded context in reads | `offset`/`limit` to avoid loading entire files into prompt |
| Stream instead of `fs.readFileSync` | For large files (>1 MB), never load entire content at once |

Never revert these mechanisms. If a performance pattern blocks a task, find a
workaround that preserves the constraint.

## Commands

```bash
cat AGENTS.md                       # the file under review
ls                                  # layout: docs/, src/, scripts/, …
cat package.json                    # or pyproject.toml / Makefile — real commands
git branch -vv && git remote -v     # branch model + push policy
git log --oneline -15               # commit-message + branch-naming conventions
```

## Checklist (done when)

- [ ] Every anatomy section that applies exists (and no dead sections for concerns the repo doesn't have).
- [ ] Each section was derived from the repo's real workflow, verifiable against manifest/git/docs.
- [ ] No rule is stated in two places with different wording.
- [ ] Commands section lists only commands that exist in the repo.
- [ ] Trigger table (if present) fires only when typed alone; record-only trigger documented as taking precedence.
- [ ] File states the knowledge-base rule (flag conflicts; never blindly execute).
- [ ] Routing table (if complex doc surface) maps task domains to the right docs.
- [ ] .gitignore section references the actual `.gitignore` and its contents.
- [ ] Hard git prohibitions are explicit (never auto-commit/push/merge/rebase/force-push).
- [ ] Build & verify section documents exact build sequence, verify steps, and deployment gate.
- [ ] Performance mechanisms are documented with "don't revert" reasons.
- [ ] Audit mode: every section verified against the current repo; stale content fixed or removed with reasons.

## Notes & gotchas

- **AGENTS.md vs skills split:** the always-loaded file holds orientation + hard rules +
  triggers; on-demand skills hold detailed procedures. If a section grows past ~a screen,
  move the procedure into a skill and leave a pointer.
- **Don't duplicate the README.** README is for humans (setup, usage); AGENTS.md is for
  the agent (rules, workflows, gates). Overlap should be a pointer, not a copy.
- Nested agents: some tools read `AGENTS.md` from subdirectories for scoped rules — if
  the repo uses that, say so in the root file.
- Keep line length and formatting conventions consistent with the repo's existing docs.
- If the repo has no strong workflow yet, write a minimal file (overview + commands + git
  rules) and grow it as conventions solidify.
