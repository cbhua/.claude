---
description: Generate a development plan for the next iteration
argument-hint: <task description, e.g. "Implement data preprocessing pipeline">
---

Generate a development plan based on the task description: "$1"

## Before Writing the Plan

1. Read `CLAUDE.md` to understand the project structure, conventions, and constraints.
2. Read `.claude/STATUS.md` to understand:
   - What phase the project is in
   - What was recently completed
   - What is currently in progress
   - Known issues that may affect this plan
3. Scan `.claude/reports/` (if any exist) to understand prior decisions and context.
4. Scan `.claude/plans/` to determine the next version number:
   - List existing plan files, find the highest `vN`, and use `v{N+1}`.
   - If no plans exist, start with `v1`.
5. Browse relevant source files in `src/` (and `tests/`) to understand the current codebase state — do not plan changes to code you haven't read.
6. If `.claude/refs/` contains relevant reference materials, read them.

## Plan Generation

Write the plan to `.claude/plans/v{N}.{slug}.md` where:
- `{N}` is the next version number
- `{slug}` is a short kebab-case summary of the task (e.g., `data-preprocessing`, `add-attention-module`)

The plan **must** follow this structure:

```markdown
# Plan v{N}: [Plan Title]

**Created**: YYYY-MM-DD
**Task**: [The original task description from the user]
**Branch** (suggested): [e.g., `feat/data-preprocessing` — optional, human decides]

## Objective

[1-2 sentences: what this iteration aims to achieve and why it matters for the project.]

## Context

[Brief summary of current project state relevant to this task. Reference STATUS.md findings.
 Mention any prior work (reports, plans) that this builds on.]

## Tasks

A concrete, ordered checklist. Each task should be atomic and verifiable.

- [ ] **Task 1 title**: Description of what to do. Specify target files where possible.
  - Acceptance: [How to verify this task is done correctly]
- [ ] **Task 2 title**: Description.
  - Acceptance: [verification criteria]
- [ ] ...

### Task Dependencies

[Note if any tasks must be completed before others, or if all are independent.
 If linear, say "Execute in order." If not, describe the dependency graph briefly.]

## Files Expected to Change

| File | Action | Notes |
|------|--------|-------|
| `src/...` | Create / Modify | [what and why] |
| `tests/...` | Create / Modify | [what and why] |
| `configs/...` | Create / Modify | [what and why] |

## References

- [List any files in `.claude/refs/` that the implementer should read]
- [List any prior reports in `.claude/reports/` that provide relevant context]
- [External references: paper sections, documentation links, etc.]

## Notes for Implementation

[Any design guidance, trade-offs to be aware of, or edge cases to watch for.
 Keep it brief — the implementer (Claude via /dev) should make detailed decisions autonomously and document them in the report.]

## Notes to Generate

[Optional. If this plan involves analysis, comparison, or experiments whose findings
 should be captured as a standalone technical note, specify it here.
 The /dev executor will generate the note under `.claude/notes/` upon completion.]

- **Note topic**: [What the note should summarize]
- **Note focus**: [What question it should answer or what comparison it should present]
- **Data sources**: [Which experiment outputs, metrics, or logs to draw from]

[If no note is needed for this plan, write "None — no standalone note for this iteration."]

## Out of Scope

[Explicitly list things that are NOT part of this iteration, especially if the task description is broad. This prevents scope creep during /dev execution.]
```

## Rules

- **Be specific**: Reference actual file paths, function names, and class names from the current codebase. Do not write vague tasks like "improve the model" — say exactly what to change and where.
- **Be realistic**: Each plan should represent a single coherent iteration of work. If the task is too large, split it and note what is deferred to a future plan.
- **Acceptance criteria are mandatory**: Every task must have a way to verify completion (test passes, file exists, metric computed, etc.).
- **Do not write code**: The plan is a specification, not an implementation. Do not include code snippets unless they clarify an interface contract or expected signature.
- **Do not modify any project files**: The `/plan` command only produces the plan file. It does not touch source code, STATUS.md, or anything else.
- **Surface risks**: If you see potential issues (dependency conflicts, architectural concerns, unclear requirements), note them in "Notes for Implementation" so the implementer is forewarned.

## After Writing the Plan

1. Print the full path of the generated plan file.
2. Print a short summary (2-3 lines) of what the plan covers.
3. Remind the human: `Run /dev v{N}.{slug} to execute this plan.`
