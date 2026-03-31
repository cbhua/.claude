---
description: Sync latest experiment results into the paper
argument-hint: [chapter or table name, e.g. 05_results or main_results]
---

Synchronize the latest experiment results into the paper's LaTeX source files.

## Step 1: Understand What Needs Updating

1. Read `.claude/STATUS.md` for the latest completed work and results.
2. Read the most recent report(s) in `.claude/reports/` that contain experiment results.
3. If `$ARGUMENTS` is provided:
   - If it matches a chapter name (e.g., `05_results`), target `paper/chapters/$ARGUMENTS.tex` and any related tables.
   - If it matches a table name (e.g., `main_results`), target `paper/tables/$ARGUMENTS.tex`.
   - If it matches multiple space-separated targets, update all of them.
4. If no argument is provided, identify which paper files need updates based on the recent reports. List them and proceed.

## Step 2: Update Paper Files

When modifying LaTeX files:

- **Tables** (`paper/tables/`):
  - Update numerical values to match the latest experiment results exactly.
  - Bold the best result in each column/metric if that convention is already in use.
  - Add new rows or columns only if the plan or report calls for it.
  - Preserve the existing table formatting and style.

- **Chapters** (`paper/chapters/`):
  - Update references to specific numbers (e.g., "achieves 94.2% accuracy" → new value).
  - Add `% TODO: review this update — synced from report vN` comments next to every prose change so the human can review them.
  - Keep prose changes minimal — update facts and figures, do not rewrite paragraphs.
  - Do not touch chapters that are unrelated to the new results.

- **Figures** (`paper/figs/`):
  - If new figures were generated during the experiment, note which ones need to be copied into `paper/figs/`.
  - Do not overwrite existing figures without explicit instruction.

## Step 3: Summary

After making changes, provide:
1. A list of every file modified and what was changed.
2. A list of all `% TODO` comments added (so the human can search for them).
3. Any figures or content that needs manual attention (e.g., a figure that should be regenerated).

## Important

- This command modifies `paper/` files which are managed by Overleaf Git. Be precise and minimal.
- Always preserve the existing LaTeX formatting, macro usage, and conventions in the paper.
- Never add new packages, redefine macros, or change the document structure.
- One sentence per line — maintain this convention for clean git diffs.