# GPN-Plasmids manuscript

LaTeX skeleton for the GPN-Plasmids paper, derived from the GPN-Star manuscript
so the two share formatting, macros and bibliography behaviour.

```
GPN-Plasmids/
├── main.tex      manuscript skeleton (preamble carried over verbatim from GPN-Star)
├── songlab.bib   lab-wide bibliography, copied from GPN-Star
├── latexmkrc     latexmk config (bbl cleanup + external-document support)
└── figures/      figure PDFs go here
```

The GPN-Star sources this was built from, plus the older GPN-MSA manuscript, are
in `../reference/` for copy-paste and for checking how something was phrased.

## Building

```
latexmk -pdf main.tex     # biber runs automatically
latexmk -c                # clean aux files
```

No TeX distribution is installed on this machine — build on Overleaf, or
install TeX Live (`sudo apt install texlive-full latexmk biber`) locally. The
skeleton compiles as-is: figure slots use `\placeholderfig` boxes rather than
`\includegraphics`, so nothing breaks before the real PDFs exist.

## Conventions carried over from GPN-Star

**Method name.** `\model` expands to the method name (`\newcommand{\model}{GPN-Plasmids\xspace}`).
Use it everywhere in prose so a rename is a one-line change. The `\xspace`
handles the following space, so write `\model achieves` — no trailing `{}`.

**Cross-references.** Use the wrapper macros rather than bare `\ref`, so the
prefix words stay consistent: `\fref`, `\tref`, `\suppfref`, `\supptref`,
`\suppfrefs{a}{b}`, `\edfref`, `\edtref`. Point at Methods and Supplementary
Text by name with `\nameref{sec:methods}` and `\nameref{sec:supp_text}`.

**Two bibliographies.** Everything cited before `\maintextpartfalse` is
collected into the `maincited` category and printed as **References** after the
Discussion; anything cited only afterwards is printed at the very end as
**Supplementary References**. This is why `\maintextpartfalse` sits immediately
before the supplement and must not move.

**Float numbering.** `\beginextendeddata` and `\beginsupplement` reset the
figure/table counters and relabel floats as *Extended Data Figure N* and
*Supplementary Figure N*. Both are already placed in `main.tex`.

**Document order.** Abstract, Introduction, Results, Discussion,
Acknowledgements, Funding, References, main figures, Methods (a pointer to the
SI plus the journal statements), Extended Data Figures, then the supplement.
Main figures come *after* the references, as Nature-style submissions require.

**Captions.** Main-text captions open with a bold lead sentence, then
`\textbf{(A)}` … one sentence per panel, then a closing sentence naming the
metric and what the error bars are. Extended Data and Supplementary captions
skip the bold lead and start directly with the description.

**Wide tables.** Use `sidewaystable` (from `rotating`) wrapped in
`\resizebox{\textwidth}{!}{...}` — see the training-setup table in
`../reference/GPN-Star/main.tex` for a worked example.

**Drafting markers.** `\todo{...}` and `\fixme{...}` render in red; grep for
them before circulating. GPN-Star also kept discarded title candidates as
comments above `\title` so the group could compare — worth doing here too.

## Authors are hidden

The PDF carries no author list. `main.tex` defines

```latex
\newif\ifshowauthors
\showauthorsfalse
```

and under `\showauthorsfalse` it passes an empty `\author{}` and sets
`\hypersetup{pdfauthor={}}`, so no names appear on the title page *or* in the
PDF metadata. The full list — names, the corresponding-author footnote and the
affiliations — is still in the source inside the `\ifshowauthors` branch, so
nothing is lost. Set `\showauthorstrue` to bring it back.

Two consequences worth knowing:

- The title page keeps the ordinary title/author gap (about 2.5 em) where the
  names would have gone. Harmless, but it is why the abstract does not sit
  flush against the title.
- The "Correspondence and requests for materials" line under **Additional
  Information** is a `\todo` rather than a name, since a name there would leak
  the corresponding author regardless of the toggle. Fill it in at submission.

## Things to change before circulating

- Title and the commented alternatives
- Funding grant numbers (currently a `\todo`)
- Acknowledgements, Data/Code Availability, Author Contributions
- Correspondence line under Additional Information
- Replace every `\placeholderfig` with `\includegraphics`
- Author list and affiliations, whenever you decide to unhide them
