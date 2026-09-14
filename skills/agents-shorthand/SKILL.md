---
name: agents-shorthand
description: "Audit and set up shorthand trigger commands (e.g. git!, push!, docs!) in a repo's AGENTS.md. Checks whether a shorthand/trigger section exists, appends a tailored trigger table derived from the repo's own conventions when missing, and audits an existing table for staleness against the actual workflow. Use when the user asks to add shorthand commands, set up a new repo's AGENTS.md, or types an `x!`-style phrase that is not defined anywhere."
metadata:
  internal: true
---

# AGENTS.md Shorthand Commands (trigger phrases)

Many repos define single-token shorthand commands — `git!`, `push!`, `docs!` — that the
agent executes when the user types one **alone** as an entire message. This skill makes
sure a repo's `AGENTS.md` actually carries such a section, and that what it carries still
matches how the repo really works.

## What this skill does

Two jobs:

1. **Set up** — if the repo's `AGENTS.md` has no shorthand/trigger section, derive one from
   the repo's own conventions and append it (after showing the user the proposed block).
2. **Audit** — if a section exists, verify each trigger still matches the repo's actual
   workflow and fix any drift.

## When to use it

- The user asks to "add shorthand commands" / "set up triggers" for a repo.
- A new repo is being onboarded and its `AGENTS.md` is being written.
- The user types an `x!`-style phrase that matches no defined trigger — the phrase may be
  intended as a shorthand that was never defined.
- Periodic hygiene: after big workflow changes (branch model, deploy flow, doc layout).
- **Not for:** executing a defined trigger (follow the repo's AGENTS.md table directly),
  or general AGENTS.md authoring beyond this section.

## Project config (read per repo, never hardcode)

Derive every trigger from the repo's real setup before proposing anything:

| Variable | Where to read it |
|---|---|
| `{default}` branch + branch model (single protected branch vs `main`+`{dev}`) | git history / AGENTS.md / remote HEAD |
| `{remote}` name | `git remote -v` |
| push policy (which branches are ever pushed) | AGENTS.md conventions |
| docs layout (`docs/`, wiki, none) | repo tree |
| build/quality gates (`npm run build`, `check:seo`, …) | package.json scripts / CI config |
| commit + branch-naming conventions | AGENTS.md |

## Rules

1. **Triggers are always-loaded config, not skills.** A trigger typed alone has no topic
   for a skill description to match, so it must live in `AGENTS.md` (auto-loaded every
   session). Never move a trigger definition into a skill — the agent may never load it
   and the phrase silently dies.
2. **Derive, never transplant.** Do not copy another repo's trigger table verbatim. A table
   written for a different branch model (`main`+`dev` vs single `main`) or different docs
   layout would contradict this repo's own rules. Build the table from the config above.
3. **Show before appending.** Present the proposed block to the user, get a yes, then append.
4. **One section, one place.** All triggers live in a single section of `AGENTS.md`
   (e.g. "Shorthand commands"). No scattered definitions elsewhere.
5. **Trigger = typed alone.** Each trigger fires only when the user's entire message is the
   token (e.g. `push!`) — never mid-sentence, never automatically otherwise.
6. **Keep instructions with implementation in sync.** If a trigger implies both an action
   and a doc update (e.g. "implement and record in docs"), encode that in the row.
7. **Staleness is drift.** An audit that finds a trigger describing a workflow the repo no
   longer uses (old branch model, removed script) is a defect — fix or remove it.

## Procedure — SET UP (no section exists)

1. Read the project config above (branch model, remote, push policy, docs layout, gates).
2. Pick triggers that match the repo's real workflow. The common set, generalized:

   | Trigger | Generic meaning (adapt per repo) |
   |---|---|
   | commit trigger (`git!`) | inspect working tree → commit on a feature branch with a descriptive imperative message (never on a protected branch) |
   | publish trigger (`push!`) | commit → integrate branches (rebase + fast-forward, no merge commits) → push only the pushable branch(es) → prune merged local branches |
   | docs-sync trigger (`docs!`) | refresh all docs so they match the current code/content; re-run any validation gates |
   | record-only trigger (`i!`) | instruction only — add the request as a rule/instruction to the relevant `.md` file, do NOT act on it |
   | implement+record trigger (`ii!`) | implement the request AND document it in the relevant `.md` files |
   | prune trigger (`clean!`) | like publish but without the push: commit → integrate → delete only merged local branches |

3. Write the section: a short intro (triggers fire when typed alone), the table
   (Trigger | Do this), and the notes (precedence rules, e.g. record-only beats implement+record).
4. Show the user the block, get approval, append it to `AGENTS.md`.
5. Commit on a feature branch per the repo's git conventions.

## Procedure — AUDIT (section exists)

1. For each row, answer: does the described action still match the repo's actual workflow?
2. Check the classic drift points:
   - branch model changed (added/removed an integration branch) — triggers referencing it
     are stale;
   - push policy changed (which branches are pushed);
   - referenced scripts/commands no longer exist (`package.json` scripts, generators);
   - doc paths referenced by record/implement triggers have moved;
   - two rows overlap or contradict each other.
3. Propose fixes row by row; apply the approved edits to the same section.
4. Report: triggers verified, triggers fixed, triggers removed (with why).

## Procedure — UNDEFINED TRIGGER FIRED

The user typed an `x!`-style phrase that matches no defined trigger:

1. Check `AGENTS.md` (and other always-loaded instruction files) for a shorthand section.
2. If none exists → offer to run SET UP.
3. If a section exists but lacks this token → ask the user what it should do, then add the
   row to the existing table. Do not guess and do not execute an undefined phrase.

## Commands

```bash
git remote -v                       # {remote}, push policy context
git branch -vv                      # branch model: does {dev} exist?
ls                                  # docs layout (docs/, *.md at root)
cat AGENTS.md                       # existing shorthand section (audit input)
```

## Checklist (done when)

- [ ] `AGENTS.md` has exactly one shorthand-commands section.
- [ ] Every trigger is derived from the repo's real workflow, not copied from elsewhere.
- [ ] Each row states what happens, including any "never do X" guard (e.g. never commit to the protected branch).
- [ ] The section states triggers fire only when typed alone as the whole message.
- [ ] Record-only trigger is documented as taking precedence over implement+record.
- [ ] Audit mode: every existing row was verified against the current workflow; stale rows fixed or removed.

## Notes & gotchas

- **Do not turn triggers into skills.** Skills load on demand; triggers must be always-loaded.
  A skill can carry the *detailed procedure* a trigger invokes, but the trigger definition
  itself stays in `AGENTS.md` (optionally with a pointer to that skill).
- Keep trigger tokens short, unique, and terminal (e.g. `word!`) so they can't collide with
  normal sentences.
- If the repo has no strong workflow yet, propose a minimal set (commit + docs-sync) rather
  than the full table.
- Placeholders like `{default}`, `{remote}`, `{dev}` above are per-repo values — resolve them
  from the project config, never hardcode.
