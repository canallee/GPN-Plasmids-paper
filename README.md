# GPN-Plasmids paper

Manuscript sources for GPN-Plasmids.

```
GPN-Plasmids/     manuscript (main.tex, songlab.bib, figures/)
reference/        GPN-Star and GPN-MSA sources — local only, not tracked
```

Start at [`GPN-Plasmids/README.md`](GPN-Plasmids/README.md) for the LaTeX
conventions: the `\model` macro, the cross-reference wrappers, the two-bibliography
setup, and the author-hiding switch.

## Building

```
cd GPN-Plasmids
latexmk -pdf main.tex
```

Needs TeX Live with `latexmk` and `biber`, or build on Overleaf.

## A note on `reference/`

The formatting of this manuscript is derived from the GPN-Star paper. Those
sources live in `reference/` locally but are **not tracked in git**: this
repository is public and the GPN-Star manuscript is unpublished. They are also
around 57 MB of figure binaries, which does not belong in git history.

If you want them versioned, put them in a **private** repository, or use
`git lfs` for the figures — but not here while this repo is public.
