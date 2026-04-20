---
description: Execute a development plan by version number and name
argument-hint: <version, e.g. v3.plan-name.md>
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
- Run necessary tests after modifying code.
- Do not modify files outside the scope of the plan unless necessary to complete a plan task.

## After Completion

- Provide a brief summary of what was done and any deviations from the plan.
- List any new issues discovered during implementation.
- Generate the report under the `.claude/reports/` folder with the same name of the plan (i.e. `$1.md`) under the following instruction:
- Run `/notify` with a summary pointing to the report path.

```markdown
# Report: [report name]

**Date**: YYYY-MM-DD
**Plan**: .claude/plans/`$1.md`

## Summary

[2-3 sentences: what was the goal and what was achieved.]

## Changes Made
- [New or modified config files, environment changes, new dependencies]

## Key Decisions

- [Any implementation decisions that were not specified in the plan]
- [Trade-offs made and their rationale]

## Results

[Experiment results, metrics, benchmarks — if applicable. Include exact numbers.]
[If no experiments were run, write "N/A — no experiments in this iteration."]

## Current Code Structure

[Brief description of the relevant code architecture after this iteration's changes.
 Focus on what changed or was added — not a full codebase walkthrough.]

## Issues & Observations

- [Bugs found, technical debt introduced, performance concerns]
- [Anything unexpected that the human should be aware of]

## Suggested Next Steps

- [ ] [Concrete, actionable items that logically follow from this work]
- [ ] ...
```

Finally, update `.claude/STATUS.md` as follows:

1. **Last Updated**: Set to today's date, note the version (e.g., "after v3 task").
2. **Current Phase**: Update if the project has moved to a new phase.
3. **Recently Completed**: Add a 1-2 line summary of this iteration's outcome. Keep only the **last 3 entries** — archive older ones by removing them.
4. **In Progress**: Update to reflect what is actively being worked on (may be empty if awaiting next plan).
5. **Next Steps**: Replace with the "Suggested Next Steps" from the report.
6. **Known Issues**: Add any new issues discovered; remove issues that were resolved in this iteration.

**Important**

- Be factual and specific in the report. Include file paths, metric values, and concrete details.
- Do not inflate results or gloss over problems — the report is a working document, not a showcase.
- The report should be useful to someone (human or Claude) who reads it weeks later with no other context.
