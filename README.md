# University of Arkansas physics dissertation in LaTeX

This repository contains the LaTeX source for Kevin Wayne Lyon's Ph.D. dissertation in physics at the University of Arkansas. It is also intended to be a practical example for other University of Arkansas graduate students who want to organize a long dissertation as a maintainable LaTeX project.

The repository contains the source needed to rebuild the dissertation, including the local `uark` document class, chapter files, bibliography, figures, and plot data. A compiled `dissertation.pdf` is included for convenient reading; other generated files and historical drafts are intentionally excluded.

> [!IMPORTANT]
> This is a working dissertation project, not an official University of Arkansas template. The header in `uark.cls` says that the class was matched to Graduate School requirements in Summer 2015; however, this project produced a dissertation that was accepted by the University of Arkansas in 2026. That acceptance makes it a recent, successfully used example, but requirements can still change. Before submitting, compare the output with the current Graduate School formatting guide and obtain approval through the University's current dissertation review process.

## Building the dissertation

The project was verified on Windows with MiKTeX, pdfLaTeX, and Biber. A reasonably complete TeX distribution should install the required LaTeX packages.

From the repository root, run:

```text
pdflatex -interaction=nonstopmode -halt-on-error dissertation.tex
biber dissertation
pdflatex -interaction=nonstopmode -halt-on-error dissertation.tex
pdflatex -interaction=nonstopmode -halt-on-error dissertation.tex
```

The first pass creates the citation and cross-reference metadata. Biber builds the bibliography. The last two pdfLaTeX passes resolve citations, references, the table of contents, and the lists of figures and tables. The result is `dissertation.pdf`.

If `latexmk` and Perl are installed, the equivalent automated command is usually:

```text
latexmk -pdf dissertation.tex
```

Do not use BibTeX for the current configuration. `dissertation.tex` loads `biblatex` with `backend=biber`.

## Repository structure

```text
dissertation.tex                 Main document and chapter order
uark.cls                         University-oriented dissertation class
uark.clo                         Class layout and font-size definitions
Preamble/preamble.tex            Shared packages and visual configuration
Commands/commands.tex            Reusable mathematical commands
Bibliography/bibliography.bib    BibLaTeX reference database
Chapters/
  Front_matter/                  Title data, abstract, committee, dedication,
                                 contents, and front-matter lists
  Introduction/                  Introduction chapter
  Background/                    Background chapter and its figures/data
  MethodologyWorking/            Methodology/theory chapter source
  GlycerolWorking/               Glycerol chapter source
  Glycerol/                      Figures and data used by that chapter
  PCSWorking/                    Photon-correlation spectroscopy chapter source
  PCS/                           Figures and data used by that chapter
  Conclusion/                    Conclusion chapter
  Back_matter/                   Appendices and their figures/data
```

The main file controls the document order with `\include{...}` statements. Each scientific chapter keeps its prose in a `.tex` file and uses `\graphicspath` to locate nearby figures and CSV/TXT data. Several plots are rendered directly from data with PGFPlots, so those data files are build dependencies rather than supplementary clutter.

## Adapting the project for another dissertation

1. Update the identifying information in `Chapters/Front_matter/front_matter.tex`:
   - `\title`
   - `\author`
   - `\degreetitle`
   - `\field`
   - `\chair`
   - `\othermembers`
   - `\numberofmembers`
   - `\abstract`
   - dedication, epigraph, acknowledgements, and optional vita content
2. Replace or rename the chapter files and update the `\include{...}` lines in `dissertation.tex`.
3. Put references in `Bibliography/bibliography.bib` and cite them normally with BibLaTeX commands such as `\cite{key}`.
4. Keep chapter figures and plot data in clearly named subdirectories. Update each chapter's `\graphicspath` when moving those assets.
5. Put project-wide packages and formatting in `Preamble/preamble.tex`, and reusable macros in `Commands/commands.tex`.
6. Rebuild from a clean directory and inspect every front-matter page, margin, heading, table, figure, citation, and page number against current University requirements.

## Working safely with the class files

Most authors should first change content in `front_matter.tex`, the chapter files, and the bibliography. Changes to `uark.cls` or `uark.clo` can affect margins, pagination, headings, signature pages, and other submission-sensitive formatting throughout the document.

If the Graduate School requests a formatting change:

1. Save a known-good PDF for comparison.
2. Make one focused class-file change at a time.
3. Rebuild the entire dissertation, not just one chapter.
4. Check the first page of every major section and all front matter.

## Portability notes

- File names and LaTeX paths are case-sensitive on Linux and many online TeX systems. Keep capitalization consistent when renaming figures or directories.
- The project uses many packages, including `biblatex`, `pgfplots`, `tikz`, `siunitx`, `mhchem`, `circuitikz`, and `fourier`. A minimal TeX installation may prompt for additional packages.
- Intermediate build products such as `.aux`, `.bcf`, `.bbl`, `.log`, and `.toc` are ignored by Git and can be regenerated. The final `dissertation.pdf` is intentionally tracked.
- The dissertation's research text, figures, and data are included as a concrete example. Anyone adapting the structure should replace that content with their own work and independently confirm reuse permissions for any template or class code.

## Troubleshooting

- **Citations are missing:** run Biber after the first pdfLaTeX pass, then run pdfLaTeX twice more.
- **A figure is missing:** check both the file name's capitalization and the chapter's `\graphicspath`.
- **A plot cannot find a CSV/TXT file:** PGFPlots searches relative to the active graphics path in several chapters; verify the data file remains in the referenced chapter asset directory.
- **The table of contents or references are stale:** delete generated build files and repeat the complete four-command build sequence.
- **`latexmk` reports that Perl is missing:** either install Perl or use the explicit pdfLaTeX/Biber sequence above.
