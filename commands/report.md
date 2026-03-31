---
description: Generate a development report and update project status
argument-hint: <version, e.g. v3>
---

Generate a completion report for iteration `$1` and update the project status file.

## Step 1: Gather Context

1. Read the plan: `.claude/plans/$1.md`
2. Review what was actually changed: run `git diff` or `git log` to see recent commits.
3. Read `.claude/STATUS.md` for the prior state.

## Step 2: Write Report

Create `.claude/reports/$1.md` with the following structure:

```markdown
# Report: [version] — [Brief Descriptive Title]

**Date**: YYYY-MM-DD
**Plan**: .claude/plans/[version].md

## Summary

[2-3 sentences: what was the goal and what was achieved.]

## Changes Made

### Project (Engineering Code)
- [List files created or modified in `project/`, grouped logically]

### Paper (if applicable)
- [List files created or modified in `paper/`]

### Configuration
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

## Step 3: Update STATUS.md

Update `.claude/STATUS.md` as follows:

1. **Last Updated**: Set to today's date, note the version (e.g., "after v3 task").
2. **Current Phase**: Update if the project has moved to a new phase.
3. **Recently Completed**: Add a 1-2 line summary of this iteration's outcome. Keep only the **last 3 entries** — archive older ones by removing them.
4. **In Progress**: Update to reflect what is actively being worked on (may be empty if awaiting next plan).
5. **Next Steps**: Replace with the "Suggested Next Steps" from the report.
6. **Known Issues**: Add any new issues discovered; remove issues that were resolved in this iteration.

## Important

- Be factual and specific in the report. Include file paths, metric values, and concrete details.
- Do not inflate results or gloss over problems — the report is a working document, not a showcase.
- The report should be useful to someone (human or Claude) who reads it weeks later with no other context.