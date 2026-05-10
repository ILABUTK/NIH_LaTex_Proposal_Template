# NIH LaTeX Proposal Template

A reusable scaffold for NIH grant applications using a single-source LaTeX
workflow. All sections compile from one master file (`proposal_package.tex`);
individual submission PDFs are extracted via ghostscript.

---

## Quick Start

1. Copy `template_nih/` to a new directory for your project.
2. Edit the variables at the top of `tex/Makefile` (project name, page ranges).
3. Fill in each `*_body.tex` file — search for `\placeholder{...}` to find every
   slot that needs content.
4. Run `make all` from inside `tex/`.

```bash
cp -r template_nih/ my_new_grant/
cd my_new_grant/tex/
make all         # compile package + extract all section PDFs
make watch       # continuous rebuild on save (requires fswatch + latexmk -pvc)
```

---

## File Structure

```
tex/
  Makefile               Build system — set page ranges here after first compile
  nih_preamble.tex       Shared NIH formatting (do not change without good reason)
  proposal_package.tex   Single compilable source — inputs all body files
  summary_body.tex       Project Summary/Abstract  (30-line NIH limit)
  narrative_body.tex     Project Narrative         (2–3 sentences)
  aims_body.tex          Specific Aims             (1-page limit)
  mpi_body.tex           Multiple PD/PI Plan       (omit if single-PI)
  strategy_body.tex      Research Strategy         (6-page hard limit for RO3)
  refs.bib               BibTeX bibliography
```

---

## NIH Page Limits (hard limits — do not exceed)

| Attachment | Limit | Notes |
|---|---|---|
| `project_summary.pdf` | 30 lines | Reviewers see this first |
| `project_narrative.pdf` | 2–3 sentences | Plain-language significance |
| `specific_aims.pdf` | **1 page** | Most important document |
| `Multiple_PD_Plan.pdf` | Fits on ~1 page | Only required for MPI submissions |
| `research_strategy.pdf` | **6 pages** | References do NOT count toward limit |
| `references.pdf` | No limit | Extracted from the end of the package |

---

## Build Targets

| Command | Action |
|---|---|
| `make all` | Compile `proposal_package.pdf` then extract all individual PDFs |
| `make package` | Compile `proposal_package.pdf` only |
| `make split` | Extract individual PDFs from existing `proposal_package.pdf` |
| `make summary` | Extract `project_summary.pdf` only |
| `make narrative` | Extract `project_narrative.pdf` only |
| `make aims` | Extract `specific_aims.pdf` only |
| `make mpiplan` | Extract `Multiple_PD_Plan.pdf` only |
| `make strategy` | Extract `research_strategy.pdf` only |
| `make references` | Extract `references.pdf` only |
| `make clean` | Remove build artifacts (keep PDFs) |
| `make cleanall` | Remove build artifacts AND all PDFs |
| `make watch` | Auto-rebuild on file changes |

---

## Page Range Workflow

The Makefile uses hardcoded page ranges for ghostscript extraction. After adding
content and changing section lengths, you must update these ranges.

**Step 1:** Run `make package` (or `make all`) — note total page count in output.

**Step 2:** Open `proposal_package.pdf` and find the actual page numbers for each
section boundary.

**Step 3:** Edit the range variables near the top of `Makefile`:

```makefile
P_SUMMARY_S  := 1    # Project Summary starts
P_SUMMARY_E  := 1    # Project Summary ends
P_NARRATIVE_S:= 2
P_NARRATIVE_E:= 2
P_AIMS_S     := 3
P_AIMS_E     := 3
P_MPI_S      := 4    # omit mpiplan target if single-PI project
P_MPI_E      := 4
P_STRATEGY_S := 5
P_STRATEGY_E := 10   # adjust when strategy changes length
P_REFS_S     := 11   # references: no upper bound (extracts to end of doc)
```

**Step 4:** Run `make split` to regenerate all individual PDFs.

---

## Single-PI vs MPI Projects

If this is a **single-PI** submission:

1. Delete `mpi_body.tex`.
2. Remove the `\input{mpi_body}` and `\clearpage` lines from `proposal_package.tex`.
3. Remove the `mpiplan` / `mpi` / `$(MPI_PDF)` targets from `Makefile`.
4. Shift page ranges accordingly (`P_AIMS_E` ends the aims; strategy starts next page).

---

## Customization Checklist

- [ ] Replace every `\placeholder{...}` in the body files with real content
- [ ] Update `refs.bib` with your actual references
- [ ] Set correct page ranges in `Makefile` after first compile
- [ ] Add project-specific macros to `nih_preamble.tex` (optional)
- [ ] Update Gantt chart dates in `strategy_body.tex` C.7 (use `YYYY-MM` format)
- [ ] Verify font: NIH requires Arial 11pt — `nih_preamble.tex` sets this via XeLaTeX
- [ ] Remove `\usepackage{draftwatermark}` if added during drafting (before submission)

---

## Requirements

- TeX Live 2022+ with XeLaTeX
- `latexmk`
- Ghostscript (`gs`)
- Arial font installed on system (required for XeLaTeX/fontspec)
- Packages: `fontspec`, `pgfgantt`, `tikz`, `natbib`, `booktabs`, `geometry`,
  `microtype`, `soul`, `enumitem`, `hyperref`, `setspace`, `titlesec`,
  `tabularx`, `longtable`, `graphicx`, `xcolor`, `ulem`

Install on macOS: `brew install --cask mactex` (includes all of the above).

---

## BibTeX Rules

- **Never** put `%` comments inside `@article{}` / `@book{}` / `@inproceedings{}`
  blocks — BibTeX treats `%` as part of a field name and throws a parse error.
- All comments go **outside** entry blocks as standalone `% ...` lines.
- Use `@inproceedings` for conference papers, `@book` for books,
  `@article` for journal papers and preprints (`journal = {arXiv}`).
