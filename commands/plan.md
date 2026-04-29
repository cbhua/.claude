---
description: Generate a writing/revision plan for the next iteration
argument-hint: <task description, e.g. "Rewrite Section 3 method narrative">
---

Generate a writing plan based on the task description: "$1"

## Before Writing the Plan

1. Read `CLAUDE.md` to understand the paper, venue rules, and writing conventions.
2. Read `.claude/STATUS.md` to understand:
   - What phase the paper is in (outlining / drafting / polishing / reviewer response / camera-ready)
   - Deadline and remaining time
   - Per-section state (which sections are draft / polished / frozen)
   - Open questions and blockers
3. Scan `.claude/reports/` (if any exist) to understand prior decisions and revisions.
4. Scan `.claude/plans/` to determine the next version number:
   - List existing plan files, find the highest `vN`, and use `v{N+1}`.
   - If no plans exist, start with `v1`.
5. Read the relevant section file(s) under `sections/` (and `appendix/` if relevant) — do not plan revisions to prose you haven't read.
6. Skim `bib/refs.bib` if the task involves citations.
7. Skim `macros.tex` if the task involves notation.
8. If `.claude/refs/` contains material relevant to the task (venue CFP, reviewer comments, cited papers, co-author notes), read it.

## Plan Generation

Write the plan to `.claude/plans/v{N}.{slug}.md` where:
- `{N}` is the next version number
- `{slug}` is a short kebab-case summary of the task (e.g., `intro-rewrite`, `related-work-positioning`, `respond-reviewer-2`)

The plan **must** follow this structure:

```markdown
# Plan v{N}: [Plan Title]

**Created**: YYYY-MM-DD
**Task**: [The original task description from the user]
**Branch** (suggested): [e.g., `revise/intro` — optional, human decides]

## Objective

[1-2 sentences: what this iteration aims to achieve and why it matters for the paper.]

## Context

[Brief summary of current paper state relevant to this task. Reference STATUS.md findings.
 Mention any prior plans/reports this builds on, and any reviewer comments or co-author feedback driving the change.]

## Tasks

A concrete, ordered checklist. Each task should be atomic and verifiable.

- [ ] **Task 1 title**: Description of what to do. Specify target files (e.g., `sections/01-intro.tex`).
  - Acceptance: [How to verify — e.g., "section opens with the new framing", "claim X has a supporting citation in refs.bib", "no `% TODO` left in this paragraph"]
- [ ] **Task 2 title**: Description.
  - Acceptance: [verification criteria]
- [ ] ...

### Task Dependencies

[Note if any tasks must be completed before others, or if all are independent.
 If linear, say "Execute in order." If not, describe the dependency graph briefly.]

## Files Expected to Change

| File | Action | Notes |
|------|--------|-------|
| `sections/03-method.tex` | Modify | [what and why] |
| `bib/refs.bib` | Add entries | [which references and why] |
| `figures/overview.pdf` | Replace | [what changes in the figure] |
| `tables/results.tex` | Modify | [which numbers update] |
| `macros.tex` | Add macro | [new notation, why needed] |

## References

- [Files in `.claude/refs/` the implementer should read — e.g., reviewer comments, the cited paper PDF, venue guidelines]
- [Prior reports in `.claude/reports/` providing context]
- [External references: bib keys, paper sections, links]

## Notes for Implementation

[Design guidance, framing tensions, voice considerations, edge cases to watch for.
 Keep it brief — the implementer (Claude via /dev) should make detailed decisions autonomously and document them in the report.]

## Out of Scope

[Explicitly list things that are NOT part of this iteration, especially if the task description is broad.
 This prevents scope creep during /dev execution.]
```

## Rules

- **Be specific**: Reference actual section files, label names, bib keys, and figure files. Do not write vague tasks like "improve the intro" — say exactly which paragraph or framing to change and where.
- **Be realistic**: Each plan should represent a single coherent iteration. If the task is too large (e.g., "rewrite the whole paper"), split it and note what is deferred to a future plan.
- **Acceptance criteria are mandatory**: Every task must have a way to verify completion (paragraph reads as X, citation key exists in `refs.bib`, word count under N, claim hedged appropriately, no `% TODO` left).
- **Do not write prose**: The plan is a specification, not the writing itself. Do not draft sentences for inclusion — the goal is to brief the implementer.
- **Do not modify any project files**: The `/plan` command only produces the plan file. It does not touch sections, bib, STATUS, or anything else.
- **Surface risks**: If you see potential issues (claims without sources, page-limit pressure, framing tensions, reviewer disagreements, missing data), note them in "Notes for Implementation" so the implementer is forewarned.

## After Writing the Plan

1. Print the full path of the generated plan file.
2. Print a short summary (2-3 lines) of what the plan covers.
3. Remind the human: `Run /dev v{N}.{slug} to execute this plan.`
