---
description: Generate a technical note summarizing findings, analysis, or insights
argument-hint: <topic, e.g. "总结各 heuristic 算法在不同数据集上的表现">
---

Generate a technical note based on the topic: "$1"

## Before Writing the Note

1. Read `CLAUDE.md` to understand the project structure and conventions.
2. Read `.claude/STATUS.md` to understand current project state.
3. Scan `.claude/notes/` to determine the next note number:
   - List existing note files, find the highest `NN`, and use `NN+1` (zero-padded to 2 digits).
   - If no notes exist, start with `01`.
4. Read `.claude/reports/` to gather implementation details, experiment results, and metrics that are relevant to the topic.
5. Read `.claude/plans/` if needed to understand the motivation and design context behind experiments.
6. Browse relevant source files, configs, and logs to collect concrete data points (metrics, hyperparameters, outputs, etc.).
7. If `.claude/refs/` contains reference materials relevant to the topic, read them.

## Note Generation

Write the note to `.claude/notes/{NN}.{slug}.md` where:
- `{NN}` is the next note number, zero-padded (e.g., `01`, `02`, `13`)
- `{slug}` is a short kebab-case summary of the topic (e.g., `heuristic-comparison`, `attention-ablation`)

The note **must** follow this structure:

```markdown
# [Note Title]

**Date**: YYYY-MM-DD
**Author**: Claude (via `/note`)
**Related Plans/Reports**: [list relevant `.claude/plans/` and `.claude/reports/` files, or "None"]

## TL;DR

[2-4 sentences capturing the key takeaway. A reader should be able to decide whether to read further based on this alone.]

## Background

[Why this topic matters. What question or observation prompted this note.
 Brief context on the project state and what experiments/work this draws from.
 Keep it short — assume the reader has general domain knowledge.]

## Methodology

[How the findings were obtained: what experiments were run, what data was used,
 what configurations were compared. Include concrete details — model names,
 hyperparameters, dataset sizes, evaluation metrics.
 If this note is purely a conceptual summary (no experiments), replace this section
 with "## Scope" and describe what is and isn't covered.]

## Findings

[The core content of the note. Present results, observations, and analysis.
 Use tables for quantitative comparisons. Use subsections if covering multiple aspects.
 Be precise — include exact numbers, file paths to relevant outputs, and
 specific conditions under which results hold.]

## Discussion

[Interpretation of the findings. What do the results mean for the project?
 What surprised you? What are the limitations or caveats?
 Connect back to the broader research objective.]

## Takeaways

[Actionable conclusions, distilled as a concise list. Each item should be
 a concrete statement that can guide future decisions, not a vague observation.
 Example: "AdamW with lr=3e-4 consistently outperforms SGD on this task by 2-4% accuracy"
 rather than "Optimizer choice matters."]
```

### Adapting the Structure

The template above is a default. Adjust sections based on the note's nature:

- **Experiment comparison note**: Keep Methodology and Findings with detailed tables. Add a "## Recommendations" section if appropriate.
- **Conceptual/survey note**: Replace Methodology with "## Scope". Expand Discussion. Findings can be organized by sub-topic.
- **Debugging/postmortem note**: Add a "## Root Cause" section after Background. Findings becomes "## Investigation" with a timeline or step-by-step.
- **Literature/method review note**: Structure Findings by method/paper. Add a comparison table. Discussion focuses on applicability to this project.

## Rules

- **Ground every claim in evidence**: Reference specific reports, experiment outputs, or code. Do not make unsupported generalizations. If a claim is speculative, label it as such.
- **Include exact numbers**: When referencing metrics, always include the actual values, the conditions (dataset, split, config), and the source (which report or log file).
- **Write for future readers**: The note should be self-contained and understandable weeks later without additional context. Define project-specific terms on first use.
- **Be honest about limitations**: If results are preliminary, inconclusive, or only hold under narrow conditions, say so clearly.
- **Do not modify any project files**: The `/note` command only produces the note file. It does not touch source code, STATUS.md, or anything else.
- **Keep it focused**: One note = one topic. If the analysis branches into a separate concern, note it as a potential future `/note` topic rather than expanding scope.

## After Writing the Note

1. Print the full path of the generated note file.
2. Print a short summary (2-3 lines) of the key takeaway.
3. If the note suggests actionable next steps that warrant implementation, suggest: `Consider running /plan to create an implementation plan based on these findings.`
