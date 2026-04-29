# [Paper Title]: [One-Line Description]

## Paper Overview

[2-3 sentences: thesis, target venue, what this paper claims that prior work doesn't.]

## Venue & Format

- **Target venue**: [e.g., NeurIPS 2026, main track]
- **Submission deadline**: YYYY-MM-DD
- **Page limit**: [e.g., 9 pages excluding refs]
- **Style file**: [e.g., neurips_2026.sty — provided by venue, do not modify]
- **Author guidelines**: [link or path under .claude/refs/]

## Repository Layout

The Overleaf project is git-cloned locally; `.claude/` lives at the project root alongside the manuscript files. Expected layout:

```
overleaf-project/
├── main.tex                    # entry point: \input each section, doc setup
├── preamble.tex                # packages and formatting (KEEP MINIMAL)
├── macros.tex                  # \newcommand definitions for math/notation
├── sections/                   # one file per section (names are illustrative — adapt per paper)
│   ├── 00-abstract.tex
│   ├── 01-intro.tex
│   ├── 02-related.tex
│   ├── 03-method.tex
│   ├── 04-experiments.tex
│   ├── 05-discussion.tex
│   └── 06-conclusion.tex
├── appendix/                   # filenames also illustrative
│   ├── a-proofs.tex
│   └── b-extra-results.tex
├── bib/
│   └── refs.bib
├── figures/                    # PDFs, PNGs, TikZ source
├── tables/                     # standalone .tex files, \input from sections
└── .claude/
    ├── CLAUDE.md
    ├── STATUS.md
    ├── plans/
    ├── reports/
    ├── notes/
    ├── refs/
    └── commands/
```

Sections are split into one file each so a `/dev` iteration can rewrite a single section without diffing the whole document. The exact filenames vary per paper — read `main.tex` to see what is actually `\input`'d, and check `.claude/STATUS.md` for the current section-file list.

## Writing Conventions

### LaTeX style
- **One sentence per line.** Hard rule. Makes diffs reviewable and lets `/dev` rewrite sentences without touching neighbors.
- **Labels are namespaced**: `\label{sec:method}`, `\label{fig:overview}`, `\label{tab:results}`, `\label{eq:loss}`.
- **References use cleveref**: `\Cref{...}` at sentence start (capitalized), `\cref{...}` mid-sentence. Avoid manual `Section~\ref{...}` once cleveref is in the preamble.
- **Math notation lives in `macros.tex`.** Never inline `\mathbf{x}` in section files — define `\xvec` once and use it everywhere.
- **Citations**: `\citep{key}` for parenthetical, `\citet{key}` for textual (natbib). Prefer not starting a sentence with a citation.
- **Non-breaking ties** between text and reference: `Section~\ref{...}`, `Table~\ref{...}` (cleveref handles this automatically when used).

### BibTeX
- One entry per paper in `bib/refs.bib`.
- Key format: `firstauthor_keyword_year` (e.g., `vaswani_attention_2017`). Stay consistent across the file.
- Include `doi` or `url` when available.
- **Never invent a citation.** If you don't have a verifiable source for a claim, leave `% TODO: cite ...` and flag it in the report.

### Figures and tables
- Source artwork in `figures/`, standalone tables in `tables/`.
- `\input{tables/results.tex}` from the section rather than inlining table source in the prose file.
- File name matches label suffix: `figures/overview.pdf` ↔ `\label{fig:overview}`.
- Captions are full sentences and self-contained — a reader skimming should grasp the figure without reading the body.

### Tone and voice
- Active voice where possible. First person plural ("we") unless the venue forbids it.
- Hedge claims that exceed the evidence ("suggests", "consistent with") rather than overclaiming.
- Define every acronym on first use, including in the abstract.
- Match the prose style of the target venue when in doubt — see prior accepted papers in `.claude/refs/`.

## Development Workflow

### How This Paper Iterates

1. Plans go to `.claude/plans/vN.{slug}.md` via `/plan`.
2. `/dev vN.{slug}` reads the plan, makes the writing/revision changes, regenerates tables if needed, commits incrementally.
3. The cycle ends with a report at `.claude/reports/vN.{slug}.md` and a `STATUS.md` update.
4. Human reviews the rendered PDF on Overleaf and starts the next cycle.

### Session Startup Checklist

**At the beginning of every session, read these files in order**:
1. This file (`CLAUDE.md`) — paper structure, conventions, guardrails
2. `.claude/STATUS.md` — current draft state per section, deadline, blockers
3. The relevant plan in `.claude/plans/` if executing `/dev`

### Slash Commands

| Command | Usage | Description |
|---------|-------|-------------|
| `/plan <task>` | `/plan Tighten Section 3 method` | Generate a plan in `.claude/plans/` |
| `/dev <version>` | `/dev v3.method-rewrite` | Execute the plan, write report, update STATUS |
| `/note <topic>` | `/note Compare framings of the contribution claim` | Generate a standalone analytical note |
| `/notify [note]` | `/notify intro frozen` | Send a Telegram summary of the just-completed task |

## Compilation

Overleaf is the canonical build — the human reviews the rendered PDF there. **Do not run local LaTeX compilation** (`latexmk`, `pdflatex`, etc.); it is not part of this workflow.

The Overleaf project is git-synced. **Do not run `git push`** unless the human explicitly says so — pushing publishes to Overleaf. Pulling (`git pull`) is fine and is in fact required at the start of every `/dev` (see the command).

## Key Files Quick Reference

When you need to understand or modify a specific aspect:

| I need to... | Look at |
|--------------|---------|
| Understand current draft state | `.claude/STATUS.md` |
| Know what to do this iteration | `.claude/plans/vN.{slug}.md` |
| See what was revised before | `.claude/reports/` |
| Read prior analytical notes | `.claude/notes/` |
| Read the manuscript | `main.tex` and `sections/` |
| Find a citation | `bib/refs.bib` |
| Look up notation | `macros.tex` |
| Read venue rules / reviews / cited papers | `.claude/refs/` (see `refs/README.md`) |

## Reference Materials in `.claude/refs/`

Useful items to keep here (read-only — never modify):
- Venue call for papers and formatting instructions
- Author kit / official template (mirror of what Overleaf uses)
- Reviewer comments and meta-review (when revising)
- Prior papers central to the related-work positioning
- Co-author notes / verbal feedback transcripts
- Experimental result tables/CSVs the paper draws on

## Notifications

This session may run on a remote server across multiple tmux windows. Use `/notify`
to ping Telegram when a long task ends — I can't watch all sessions at once.

**Auto-run `/notify` when finishing**:
- Any `/dev vN.{slug}` execution (after the report is written)
- Multi-step tasks (>~5 real tool calls), e.g., a full section rewrite or related-work synthesis
- Long sweeps across many sections or many bib entries

**Don't notify for**: quick lookups, single typo fixes, clarifying
questions, or mid-task progress. When unsure, skip it.

The summary should be terse: what was done, the result, and any follow-up I
should know. Always includes project + tmux context automatically.

---

## Constraints and Guardrails

- **Never delete or overwrite** files in `.claude/refs/`. They are read-only references.
- **Never run `git push`** without explicit human instruction (pushes publish to Overleaf).
- **Never modify the venue style/class files** (`*.sty`, `*.cls`) — these are dictated by the conference/journal.
- **Never invent citations.** If a claim needs a source you can't verify, leave `% TODO: cite ...` and surface it in the report.
- **Use existing macros.** When writing a section, use `\newcommand`s already in `macros.tex`. If new notation is genuinely needed, add it to `macros.tex` and note it in the report — do not redefine notation locally in section files.
- **One sentence per line** in all `.tex` files. Match this convention even when adding new content.
- **Preserve existing prose voice** when revising — match the surrounding paragraphs rather than introducing a new style mid-section.
- When in doubt about a writing/framing decision during `/dev`, **document the decision and rationale** rather than asking — the human will review in the report.
