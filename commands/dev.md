---
description: Execute a development plan by version number
argument-hint: <version, e.g. v3>
---

Execute the development plan specified in `.claude/plans/$1.md`.

## Before Starting

1. Read `.claude/STATUS.md` for current project state and known issues.
2. Read the plan file `.claude/plans/$1.md` thoroughly before writing any code.
3. If the plan references files in `.claude/refs/`, read the relevant ones.
4. If the plan references previous reports, read them from `.claude/reports/`.

## During Execution

- Follow the plan's task list in order unless a dependency requires reordering.
- If you encounter ambiguity or an under-specified requirement in the plan, make a reasonable decision, proceed, and **document the decision clearly** — do not stop to ask.
- Commit meaningful intermediate progress using the project's commit prefix convention:
  - `[project]` for code changes
  - `[paper]` for paper changes
  - `[config]` for configuration changes
  - `[fix]` for bug fixes
- Run tests after modifying code in `project/src/`. If tests fail, fix them before moving on.
- Do not modify files outside the scope of the plan unless necessary to complete a plan task.

## After Completion

- Provide a brief summary of what was done and any deviations from the plan.
- List any new issues discovered during implementation.
- Do **not** generate the report — the human will run `/report $1` separately.