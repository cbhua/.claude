---
description: Generate a technical note summarizing findings, analysis, or framing decisions
argument-hint: <topic, e.g. "Compare framings of the contribution claim">
---

Generate a technical note based on the topic: "$1"

## Before Writing the Note

1. Read `CLAUDE.md` to understand the paper, venue, and conventions.
2. Read `.claude/STATUS.md` to understand current paper state.
3. Scan `.claude/notes/` to determine the next note number:
   - List existing note files, find the highest `NN`, and use `NN+1` (zero-padded to 2 digits).
   - If no notes exist, start with `01`.
4. Read `.claude/reports/` to gather details from prior iterations relevant to the topic.
5. Read `.claude/plans/` if needed to understand the motivation behind prior decisions.
6. Read the relevant section files, `bib/refs.bib`, and any cited material to gather concrete data points (quotes, claims, citations, numbers).
7. If `.claude/refs/` contains material relevant to the topic (reviewer comments, cited papers, venue rules), read it.

## Note Generation

Write the note to `.claude/notes/{NN}.{slug}.md` where:
- `{NN}` is the next note number, zero-padded (e.g., `01`, `02`, `13`)
- `{slug}` is a short kebab-case summary of the topic (e.g., `framing-options`, `reviewer-2-rebuttal`, `related-work-survey`)

The note **must** follow this structure:

```markdown
# [Note Title]

**Date**: YYYY-MM-DD
**Author**: Claude (via `/note`)
**Related Plans/Reports**: [list relevant `.claude/plans/` and `.claude/reports/` files, or "None"]

## TL;DR

[2-4 sentences capturing the key takeaway. A reader should be able to decide whether to read further based on this alone.]

## Background

[Why this topic matters for the paper. What question or observation prompted this note.
 Brief context on the paper's current state and what work this draws from.
 Keep it short — assume the reader knows the field.]

## Methodology

[How the findings were obtained: what was compared, what sources were read, what criteria were applied.
 Include concrete details — section references, bib keys, reviewer comment numbers, quoted excerpts.
 If this note is purely a conceptual or framing exercise (no comparison to data), replace this section
 with "## Scope" and describe what is and isn't covered.]

## Findings

[The core content of the note. Present observations and analysis with quotes, exact citations,
 and section references. Use tables for side-by-side comparisons (framings, reviewer arguments,
 related-work positionings). Be precise — quote source language verbatim where wording matters.]

## Discussion

[What do the findings mean for the paper? What surprised you? What are the limitations or caveats?
 Connect back to the paper's thesis and target venue.]

## Takeaways

[Actionable conclusions, distilled as a concise list. Each item should be a concrete statement
 that can guide a future plan, not a vague observation.
 Example: "Move the limitations paragraph before the conclusion to address Reviewer 2's concern about over-claiming"
 rather than "Address reviewer comments better".]
```

### Adapting the Structure

The template above is a default. Adjust sections based on the note's nature:

- **Framing/positioning note**: Keep Methodology and Findings with options laid out side-by-side. Add a "## Recommendations" section.
- **Related-work / literature survey note**: Replace Methodology with "## Scope". Structure Findings by paper or sub-area. Discussion focuses on positioning the current paper.
- **Reviewer-response planning note**: Add a "## Reviewer Concerns" section after Background. Findings becomes per-comment analysis with quoted excerpts and proposed responses.
- **Decision postmortem**: Add a "## Context" or "## Timeline" section. Findings becomes the decision rationale and trade-offs considered.

## Rules

- **Ground every claim in evidence**: Reference specific sections, bib keys, reviewer-comment numbers, or prior reports. Do not generalize without a source. If a claim is speculative, label it.
- **Quote precisely**: When referencing reviewer language, prior claims, or cited papers, quote verbatim. Wording matters in writing decisions.
- **Write for future readers**: The note should be self-contained and understandable weeks later without additional context. Define paper-specific terms on first use.
- **Be honest about limitations**: If the analysis is preliminary, inconclusive, or only holds under narrow conditions, say so clearly.
- **Do not modify any project files**: The `/note` command only produces the note file. It does not touch sections, bib, STATUS, or anything else.
- **Keep it focused**: One note = one topic. If the analysis branches, note it as a future `/note` topic rather than expanding scope.

## After Writing the Note

1. Print the full path of the generated note file.
2. Print a short summary (2-3 lines) of the key takeaway.
3. If the note suggests actionable next steps that warrant implementation, suggest: `Consider running /plan to create an implementation plan based on these findings.`
