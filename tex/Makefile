# ============================================================
# NIH R03 LaTeX Build System
#
# Single-source workflow: proposal_package.pdf is compiled first,
# then individual section PDFs are extracted via ghostscript.
#
# SETUP — edit these variables for your project:
#   PACKAGE   Name of the master .tex file (without extension)
#
# PAGE RANGES — update after first compile if section lengths change.
#   Run: make package
#   Open proposal_package.pdf and find each section's page boundaries.
#   Update the P_* variables below, then run: make split
#
# Usage:
#   make              — compile package + extract all section PDFs
#   make package      — compile proposal_package.pdf only
#   make split        — extract individual PDFs from package
#   make summary      — extract project_summary.pdf
#   make narrative    — extract project_narrative.pdf
#   make aims         — extract specific_aims.pdf
#   make mpiplan      — extract Multiple_PD_Plan.pdf
#   make strategy     — extract research_strategy.pdf
#   make references   — extract references.pdf
#   make clean        — remove build artifacts (keep PDFs)
#   make cleanall     — remove build artifacts AND PDFs
#   make watch        — auto-rebuild on changes
# ============================================================

LATEXMK   := latexmk
FLAGS     := -xelatex -interaction=nonstopmode -halt-on-error
GS        := gs
GS_FLAGS  := -dNOPAUSE -dBATCH -sDEVICE=pdfwrite

PACKAGE   := proposal_package
SOURCES   := $(wildcard *.tex) refs.bib

# ---- Page ranges (update after first compile if sections change length) ----
P_SUMMARY_S    := 1
P_SUMMARY_E    := 1
P_NARRATIVE_S  := 2
P_NARRATIVE_E  := 2
P_AIMS_S       := 3
P_AIMS_E       := 3
P_MPI_S        := 4
P_MPI_E        := 4
P_STRATEGY_S   := 5
P_STRATEGY_E   := 6   # adjust when strategy changes length
P_REFS_S       := 7   # references: no upper bound (extracts to end of doc)

# ---- Output PDF names ----
SUMMARY_PDF   := project_summary.pdf
NARRATIVE_PDF := project_narrative.pdf
AIMS_PDF      := specific_aims.pdf
MPI_PDF       := Multiple_PD_Plan.pdf
STRATEGY_PDF  := research_strategy.pdf
REFS_PDF      := references.pdf

.PHONY: all package split summary narrative aims strategy mpiplan mpi references clean cleanall watch

all: package split

package: $(PACKAGE).pdf

$(PACKAGE).pdf: $(PACKAGE).tex $(SOURCES)
	$(LATEXMK) $(FLAGS) $<

split: $(SUMMARY_PDF) $(NARRATIVE_PDF) $(AIMS_PDF) $(MPI_PDF) $(STRATEGY_PDF) $(REFS_PDF)

$(SUMMARY_PDF): $(PACKAGE).pdf
	$(GS) $(GS_FLAGS) -dFirstPage=$(P_SUMMARY_S) -dLastPage=$(P_SUMMARY_E) -sOutputFile=$@ $<

$(NARRATIVE_PDF): $(PACKAGE).pdf
	$(GS) $(GS_FLAGS) -dFirstPage=$(P_NARRATIVE_S) -dLastPage=$(P_NARRATIVE_E) -sOutputFile=$@ $<

$(AIMS_PDF): $(PACKAGE).pdf
	$(GS) $(GS_FLAGS) -dFirstPage=$(P_AIMS_S) -dLastPage=$(P_AIMS_E) -sOutputFile=$@ $<

$(MPI_PDF): $(PACKAGE).pdf
	$(GS) $(GS_FLAGS) -dFirstPage=$(P_MPI_S) -dLastPage=$(P_MPI_E) -sOutputFile=$@ $<

$(STRATEGY_PDF): $(PACKAGE).pdf
	$(GS) $(GS_FLAGS) -dFirstPage=$(P_STRATEGY_S) -dLastPage=$(P_STRATEGY_E) -sOutputFile=$@ $<

$(REFS_PDF): $(PACKAGE).pdf
	$(GS) $(GS_FLAGS) -dFirstPage=$(P_REFS_S) -sOutputFile=$@ $<

summary:    $(SUMMARY_PDF)
narrative:  $(NARRATIVE_PDF)
aims:       $(AIMS_PDF)
strategy:   $(STRATEGY_PDF)
mpiplan:    $(MPI_PDF)
mpi:        $(MPI_PDF)
references: $(REFS_PDF)

clean:
	$(LATEXMK) -c $(PACKAGE).tex
	@rm -f *.bbl *.run.xml

cleanall: clean
	@rm -f $(PACKAGE).pdf $(SUMMARY_PDF) $(NARRATIVE_PDF) $(AIMS_PDF) $(MPI_PDF) $(STRATEGY_PDF) $(REFS_PDF)

watch:
	$(LATEXMK) $(FLAGS) -pvc $(PACKAGE).tex
