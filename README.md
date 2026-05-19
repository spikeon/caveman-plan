# caveman-plan

Big project? Epic first. Plans split. Hunt one file. Boss say go. Then next hunt.

Agent skill for **large-project planning and execution**: Epic checklist → multiple hunt plans → one plan per hunt (hard stops) → inventory pass against original Goal. Talks to Boss in [caveman](https://github.com/JuliusBrussee/caveman) voice; plan tasks use [superpowers-plan](https://github.com/obra/superpowers) step shape (files, change, verify).

## What it does

Four phases. Boss checkpoints between each. Planning artifacts live in `.plans/` (gitignored — local only).

| Phase | Output | Boss checkpoint |
|-------|--------|-----------------|
| **Epic** | `.plans/epic_<slug>.md` — Goal (verbatim prompt) + Steps (S/M/L hints) | Changes before planning? |
| **Plans** | `.plans/epic_<slug>/plan_*.md` — **one plan file per Epic step minimum** + `plans-manifest.md` | Time to go hunting? |
| **Hunts** | Kill tasks in **one** `plan_*.md`, then **end turn** | Follow Ups? → Time for next hunt? |
| **Inventory** | Compare kills to Epic Goal + Steps | Hunting complete (only when Epic satisfied) |

**Hunt rules:** Mark tasks `- [x]` only when sure. Doubt → `## Follow Ups` in same plan, not checked off. **Never** chain multiple plans in one turn. **Never** collapse a multi-step Epic into one plan file.

**Layout:**

```
.plans/
├── epic_auth-refactor.md
└── epic_auth-refactor/
    ├── plans-manifest.md      # step → plan mapping + hunt status
    ├── plan_jwt-middleware.md
    ├── plan_migration.md
    └── plan_rollout-flag.md
```

## How to invoke

```
/caveman-plan              # start Epic from current prompt
/caveman-plan hunt         # resume current hunt (one plan only)
/caveman-plan inventory    # Phase 4 only
```

**Claude + `/goal` enabled:** set goal at start, Boss says **`NEXT HUNT!`** between plans:

```
/goal Epic file created. All plan files created. Every checkbox in epic and every plan checked off.
```

Natural triggers: "epic plan", "go hunting", **`NEXT HUNT!`**, large multi-file / multi-session work.

**Hunt turn limit:** one plan = max **3 agent turns**. Still open on turn 3 → stop, report blockers, wait for Boss (no silent grind).

## Install

Requires **`caveman`** and **`superpowers-plan`** skills installed (see [Credits](#credits)).

**Cursor / Claude Code (`~/.agents/skills`):**

```bash
git clone https://github.com/spikeon/caveman-plan.git ~/.agents/skills/caveman-plan
```

Or symlink a local clone:

```bash
ln -sfn /path/to/caveman-plan ~/.agents/skills/caveman-plan
```

**Cursor project skill (optional):**

```bash
mkdir -p .cursor/skills
ln -sfn /path/to/caveman-plan .cursor/skills/caveman-plan
```

Attach the skill or mention `/caveman-plan` in chat.

## Example flow

**Boss:** "Refactor auth to JWT. No forced logout on deploy."

**Epic** (caveman to Boss): writes `epic_auth-refactor.md` with Goal + 4 Steps → *"Boss: change Epic before planning?"*

**Plans:** 4+ `plan_*.md` files + manifest → *"Boss: time go hunting?"*

**Hunt 1:** only `plan_jwt-middleware.md` → end turn → *"Follow Ups? Next hunt?"*

**Hunt 2+:** Boss says yes → next plan only.

**Inventory:** re-read Goal, check every Epic step → *"Hunting complete."*

## See also

- [`SKILL.md`](./SKILL.md) — full agent instructions
- [`reference.md`](./reference.md) — templates, manifest examples, anti-patterns

## Credits

This skill **builds on** (install separately):

| Project | Author | Used for |
|---------|--------|----------|
| [**caveman**](https://github.com/JuliusBrussee/caveman) | [Julius Brussee](https://github.com/JuliusBrussee) | Terse caveman voice when talking to Boss during the workflow |
| [**superpowers**](https://github.com/obra/superpowers) (`superpowers-plan`) | [Jesse Vincent (obra)](https://github.com/obra) | Per-task plan format: small steps, files to touch, verify commands |

**caveman-plan** combines those patterns into Epic → Plans → Hunts → Inventory with mandatory Boss checkpoints. It is not affiliated with or maintained by the upstream projects.

## License

MIT — see [LICENSE](./LICENSE).
