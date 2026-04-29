---
description: Execute a writing/revision plan by version number and name
argument-hint: <version, e.g. v3.intro-rewrite>
---

Execute the plan specified in `.claude/plans/$1.md`.

## Before Starting

1. **Run `git pull`** at the project root to sync any edits the human made on Overleaf since the last session. If the pull produces a merge conflict, stop and surface it — do not attempt to auto-resolve unless trivially obvious.
2. Read `CLAUDE.md` for paper-wide conventions and guardrails.
3. Read `.claude/STATUS.md` for current state, deadline, and known blockers.
4. Read the plan file `.claude/plans/$1.md` thoroughly before changing any prose.
5. If the plan references files in `.claude/refs/` (reviewer comments, cited papers, venue rules), read the relevant ones.
6. If the plan references previous reports, read them from `.claude/reports/`.
7. Read the target section file(s) in full before editing — do not edit prose you haven't read.

## During Execution

- Follow the plan's task list in order unless a dependency requires reordering.
- If you encounter ambiguity or an under-specified requirement in the plan, make a reasonable decision, proceed, and **document the decision clearly** in the report — do not stop to ask.
- Maintain the **one-sentence-per-line** convention in all `.tex` edits.
- Use existing macros from `macros.tex`. Add new macros there (not inline) and note them in the report.
- If a claim needs a citation you can't verify, leave `% TODO: cite ...` in the source and flag it in the report — do not invent a reference.
- **Do not run `git commit`** during `/dev`. Leave changes in the working tree; the human commits manually after reviewing.
- **Do not run local LaTeX compilation.** Overleaf is the canonical build; the human will check the rendered PDF there.
- Do not modify files outside the scope of the plan unless necessary to complete a plan task.
- Do not modify venue style/class files (`*.sty`, `*.cls`).

## After Completion

- Provide a brief summary of what was done and any deviations from the plan.
- List any new issues discovered during the work.
- Generate the report under `.claude/reports/` with the same name as the plan (i.e. `$1.md`) following the structure below. **The report is generated after every `/dev`.** Standalone notes under `.claude/notes/` are produced only when the human runs `/note` — do not generate one here.
- Run `/notify` with a summary pointing to the report path.

```markdown
# Report: [report name]

**Date**: YYYY-MM-DD
**Plan**: .claude/plans/`$1.md`

## Summary

[2-3 sentences: what was the goal and what was achieved.]

## Changes Made

- [Sections modified, with a brief description of what changed]
- [Bib entries added or edited, with keys]
- [Figures/tables added or replaced]
- [Macro additions in macros.tex]
- [Any preamble/STATUS updates]

## Key Decisions

- [Writing/framing decisions not specified in the plan]
- [Trade-offs made and the rationale — e.g., "trimmed paragraph X to keep page count under 9"]
- [Citations added or deferred and why]

## Current Manuscript State

[Per-section update relative to the plan. Which sections moved from draft → polished, which are now frozen.
 Total page count if known and any pressure against the page limit.]

## Open TODOs Left in the Manuscript

- [ ] [Any `% TODO: cite ...` left in source, with file path and approximate location]
- [ ] [Any `% TODO: ...` for content gaps]

## Issues & Observations

- [Concerns about claims, framing tensions, missing data, reviewer points still unaddressed]
- [Anything unexpected the human should know before reviewing the PDF]

## Suggested Next Steps

- [ ] [Concrete, actionable items that logically follow from this iteration]
- [ ] ...
```

Finally, update `.claude/STATUS.md` as follows:

1. **Last Updated**: today's date, note the version (e.g., "after v3.intro-rewrite").
2. **Current Phase**: update if the paper has moved phases (drafting → polishing, etc.).
3. **Section Status table**: update the state and word/page count for any section touched this iteration.
4. **Recently Completed**: add a 1-2 line summary of this iteration's outcome. Keep only the **last 3 entries** — archive older ones by removing them.
5. **In Progress**: update to reflect what is actively being worked on (may be empty if awaiting next plan).
6. **Next Steps**: replace with the "Suggested Next Steps" from the report.
7. **Open Questions / TODOs**: add new blockers; remove any resolved this iteration.

**Important**

- Be factual and specific in the report. Include file paths, exact bib keys, word counts, and page counts.
- Do not inflate progress or gloss over problems — the report is a working document, not a showcase.
- The report should be useful to someone (human or Claude) who reads it weeks later with no other context.
