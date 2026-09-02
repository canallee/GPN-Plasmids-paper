# Conversion notes — artifacts → LaTeX

What each section of `main.tex` came from, what was deliberately left out, and
what still needs your hand. Produced by the conversion described in
`../CONVERSION_PROMPT.md`.

**Snapshot: 2026-08-28.** The artifacts are still being edited, so this is a
point-in-time transcription. Each `sections/*.tex` file opens with a header
comment naming its source and that date, so a single section can be re-synced
without touching the others.

## Where each section came from

| LaTeX file | Source | Snapshot |
|---|---|---|
| `sections/01_introduction.tex` | `CONVERSION_PROMPT.md` §8 (author-provided) | 2026-08-28 |
| Abstract (in `main.tex`) | `CONVERSION_PROMPT.md` §7 (author-provided) | 2026-08-28 |
| `sections/02_pretraining.tex` | `plasmids_GPN/manuscript/artifact/page.html`, "Main text" band | mtime 2026-08-28 13:27 |
| `sections/03_origin.tex` | `BEND_ori_intervals/manuscript/artifact_origin/page.html`, "Main text" band | mtime 2026-08-28 15:33 |
| `sections/04_genomewide.tex` | — (unwritten; placeholder only) | — |
| `sections/05_interpretation.tex` | `plasmids_GPN/manuscript/artifact_interpretation/page.html`, "Main text" band | mtime 2026-08-28 15:47 |
| `sections/supp_methods_02_pretraining.tex` | `pretraining/02_`, `03_` (non-factorized), `04_`, `05_*.md` | 2026-08-28 |
| `sections/supp_methods_03_origin.tex` | origin artifact, `section#methods` | 2026-08-28 |
| `sections/supp_methods_05_interpretation.tex` | interpretation artifact, `section#methods` | 2026-08-28 |
| `sections/main_floats.tex` | Figure/table bands of all three artifacts | 2026-08-28 |
| `sections/supp_tables.tex` | `pretraining/06_*.md` + both artifacts' supplementary tables | 2026-08-28 |
| `sections/supp_figures.tex` | Supplementary bands of all three artifacts | 2026-08-28 |

## Decisions you may want to revisit

**1. §2 Results came from the artifact, not from `01_results_sampling_design.md`.**
Those two diverge. The markdown file is a longer, section-headed narrative
(~1,400 words, six `##` headings, blockquoted figure callouts); the artifact's
"Main text" band is seven flowing paragraphs sized as a Results subsection.
The conversion prompt says the artifacts are the source of truth and the
artifacts' own build spec says their main text is written so that promoting it
into the manuscript is copy-paste, so the artifact version was used. The
markdown Methods files (`02`–`06`) *were* used — those match the artifact and
self-declare as canonical. Swap in the longer narrative if you meant that one.

*Superseded 2026-09-01, author-directed.* Those seven paragraphs are no longer
presented as one subsection. They were **regrouped into three Results
subsections** — `IMG/PR processing` (`sec:imgpr`), `Diversity-aware sampling
for pretraining` (`sec:sampling`) and `Pretraining and model architecture`
(`sec:pretraining`) — laid out 2 / 3 / 3 paragraphs. This is a structural
edit, not a re-transcription: the file still tracks the same artifact band and
`02_pretraining.tex` remains one file, so the provenance row above still holds.
Two sentences changed paragraph (old P1 s2 now opens §2.3; old P2 s1 and s3 now
open §2.2) and three sentences were added (the Fig. 1c callout and the closer
in §2.1, and the second half of the §2.2 opener). No transcribed clause was
deleted. Note that the split ends up closer in shape to
`01_results_sampling_design.md`, so if you ever swap that narrative in, compare
against the three-subsection layout rather than the flat one. Two further
author-directed changes rode along: IMG/PR is now expanded on first use in the
body, and the epoch size was reconciled from "approximately 170,000" to the
measured mean of 166,192.

*Superseded 2026-09-01, author-directed — §3, same session.* `03_origin.tex`
was given the same treatment. Its seven transcribed paragraphs are **regrouped
into four Results subsections** — `Origin scoring in intergenic regions`
(`sec:origin`, the original label, kept), `Origins of transfer`
(`sec:origin-orit`), `Origins of replication` (`sec:origin-oriv`) and
`Model scale, ensembling and inference cost` (`sec:origin-cost`) — laid out
3 / 2 / 2 / 3 paragraphs. Again structural, not a re-transcription; the
provenance row above still holds. Two things drove it:

- **The inference pipeline was undescribed.** No sentence anywhere in the
  manuscript described how a window-level probe becomes a region-level
  annotation, and ED Fig. 2, which draws exactly that, was cited nowhere. It
  now opens the section as §3.1 P2, before any result. The paragraph is new
  prose but carries no new fact: every value in it comes from the ED Fig. 2
  caption or from `supp_methods_03_origin.tex` ("Calibration and region
  aggregation", "Inference"). ED Fig. 2's own caption was retitled to match —
  see the Extended Data section below.
- **oriT and oriV were interleaved.** Every paragraph reported both tasks in
  one breath ("0.87 on oriT and 0.77 on oriV"). They are now separated, with
  §3.2 carrying the full baseline sweep once and §3.3 written purely as
  contrast — the stronger baselines, the narrower margin, the window/region
  ordering — without redefining the metrics, the splits or the novelty
  protocol. Splitting by task also tightened the numbers: the transcription
  gave ranges spanning both tasks ("Evo2 probes scored 0.55–0.72"), which are
  now per-task.

One transcribed clause was dropped, as a duplicate: old P6 ended "this
throughput difference was the reason we applied standalone GPN-Plasmids across
IMG/PR" and old P7 opened "We therefore applied the standalone GPN-Plasmids
classifiers across IMG/PR", so the two were merged into the closer of §3.4.
Sentences were added at §3.1 P2 (in full), at the head of §3.1 P3 (benchmark
design, from the Fig. 2a caption and Supplementary Methods, including the oriT
reinsertion caveat that previously appeared only in Methods and in the Supp.
Fig. S2 caption), at the head of §3.3 (the contrast framing) and at the head of
§3.4 P1. One of the last of those — that parameter count did not translate
monotonically into accuracy — is the only genuinely new *claim* in the section;
it is read straight off Tables 1 and 2, but the artifact never stated it, so it
carries an inline `% FLAG` asking for author sign-off.

The split also closed a citation gap. Eleven floats had no main-text citation
at all: ED Figs. 2–4, Supp. Figs. S2–S8, and Tables 1–2 with Supp. Tables S2–S3
were cited only from their own captions. Nature Biotechnology numbers Extended
Data and Supplementary figures by order of first citation, so the numbering was
undefined. Citations were placed in an order that reproduces the **existing**
numbering exactly, so no float in `extended_data.tex` or `supp_figures.tex`
needed reordering: ED 2 → §3.1, ED 3 → §3.2 then §3.3, ED 4 → §3.4; S2, S3 →
§3.1, S4, S5 → §3.2, S6, S7 → §3.3, S8 → §3.4. If you move a citation, re-check
that order or the figures renumber.

**2. Nature-style unnumbered sections, per your instruction.** There are no
section numbers, so the `§2` / `§3` / `§4` pointers written into the origin and
interpretation prose resolve by name through a `\secref` macro
(`\secref{sec:origin}` → "Origin classification"). Six places read a little
oddly as a result and each carries a `% TODO wording:` comment — most notably
"the §3 classifier", which renders as "the Origin classification classifier".
The wording is yours to fix; nothing was reworded at conversion time.

*Update 2026-09-01.* One of the six is now resolved. `03_origin.tex` read
"without repeating the pretraining procedure of `\secref{sec:pretraining}`",
which the §2 split would have rendered as "…the pretraining procedure of
Pretraining and model architecture" — a noun-phrase collision. It was recast to
"without repeating the pretraining described in `\secref{sec:pretraining}`", so
the heading is the object of a preposition, where a title reads naturally. The
`% TODO wording:` comment there was replaced with a `% NOTE` recording the
change. This is the second place where a structural change forced a change to
transcribed wording, after the Extended Data promotion noted below.

*Update 2026-09-01, later the same session.* The §3 split resolved three more,
leaving **one**. `\secref{sec:origin}` was the worst of them: it rendered as
"the Origin classification classifier" in both `05_interpretation.tex` and
`supp_methods_05_interpretation.tex`, and pointing it at a subsection would not
have helped. Both were recast on the pattern the §2 split settled on — the
heading as the object of a preposition — to "the origin classifier described in
`\secref{sec:origin}`". `sec:origin` deliberately stays on §3.1, which is where
the classifier is defined, so the pointer lands on the right subsection. The
third was `03_origin.tex`'s pointer to `\secref{sec:genomewide}`, which needed
no rewording at all: "the mobilization and replication analyses in
Genome-wide application" already reads as that construction, so its TODO was
simply downgraded to a NOTE. The one that remains is
`05_interpretation.tex:28`, "across IMG/PR (`\secref{sec:genomewide}`)".

**3. Methods sit in the Supplementary Information,** following GPN-Star:
`\section*{Methods}` is a one-paragraph pointer plus the journal statements,
and the transcribed Methods are under `\section*{Supplementary Methods}`.
`\label{sec:methods}` was left on the main `\section*{Methods}` so the
artifacts' literal "(Methods)" pointers still render as "Methods". A
consequence of the placement: a work cited *only* inside Methods lands in
**Supplementary References**, not the main reference list. Today that affects
skani alone.

**4. Two citations were inserted that the origin artifact did not have.**
`\cite{camargo2024img}` at IMG/PR and `\cite{shaw2023fast}` at skani in
`supp_methods_03_origin.tex`, because both works are already cited elsewhere in
this same manuscript and the entries exist. Every other missing citation was
left as a `% TODO cite:` rather than guessed. Delete these two if you would
rather the section stay citation-free until you do a full pass.

**5. The origin Methods' last subsection is titled "Inference",** which
collides with the pretraining Methods' own "Inference" heading. Kept verbatim
rather than renamed; a `% NOTE` marks it.

## What was deliberately NOT converted

These bands are working memo, not manuscript, and never appear in the paper.
Nothing from them is in the `.tex`.

**Origin artifact:** the whole "Positioning notes" section (contribution stack,
method taxonomy table, claim / don't-claim ledger, reviewer pre-emptions); the
whole "Open items" section (Blockers, and "Limitations (state proactively)");
the whole "Code references" section; the masthead stat bar; the sticky
contents rail; the theme toggle; the provenance footer; all 17 `pick` status
chips; and all 12 dashed `.why` notes under figure captions.

> One judgement call worth your eye: the three bullets under **"Limitations
> (state proactively)"** — linearization of circular plasmids, cross-database
> redundancy, and region ≠ exact origin — read closer to manuscript material
> than the rest of that band. Rule 2 of the conversion prompt classes "Open
> items" as reminders rather than the paper's own limitations prose, so they
> were left out. They may deserve a home in the Discussion.

**Interpretation artifact:** the whole "Decisions needed" section (four
callouts); the whole "Open items" section (ten bullets); the "How the figure
reads" callout above Figure 4 (it references "Decision A"); six grey
`sectnote` lines; the stat bar, contents rail, theme toggle and footer; the
`Final` / `Component` / `Keep` / `Draft` chips; and all 10 `.why` notes. Note
that the `.why` notes sit *inside* `<figcaption>` in the HTML — they were
stripped programmatically so none could leak into a published caption.

**Pretraining artifact:** the stat bar, contents rail, provenance footer, and
the two inline editorial notes saying which markdown file a Methods subsection
mirrors. `00_index.md` and `99_provenance_and_regeneration.md` are working
notes and were skipped, as the prompt directs.

## Figures

Only complete, assembled figures were used, and every `\includegraphics` points
into `figures/`, never at a source artifact repo.

Twenty-one of the 22 assets were copied from their artifacts unedited — nothing
regenerated, resized, or recoloured. **Figure 2 is the exception.** It was
originally the origin artifact's own assembled card, `figure4_composite.pdf`
(9.04 × 8.89 in); the author replaced it with `Fig_2_Main.pdf` (12.16 × 11.91
in) in commit `67d8a55`, "Better sized fig 2". That file does not exist in the
origin artifact repo, which still carries only `figure4_composite.pdf`, so
Figure 2's artwork and its companion artifact have diverged — re-syncing §3
from the artifact will not bring the new figure with it.

**Dropped as component / standalone panels:**

- pretraining: `fig1_a_oriT`, `fig1_b_oriV`, `fig1_c_ecosystem`,
  `fig1_d_imgpr_composition`, `fig1_e_sampling_rate`, `fig1_schematic`, and
  all `figSp_*`
- origin: the six "Fig. 2a"–"Fig. 2f" cards
  (`panel1_data_splitting_evaluation`, `panel_b_window`,
  `panel_c_novelty_scatter`, `panel_d_precision1`, `panel_e_tracks`,
  `inference_speed_panel`), plus the superseded `S4_main_figure_draft.png`,
  the `panel2`–`panel5b` predecessors, every `*_oriT` / `*_oriV` half of the
  stitched supplementary composites, and everything in `figures/archive/`
- interpretation: `fig5_a_pigrk_orit` and `fig5_b_psc101_oriv`
  (the "Fig. 4a" / "Fig. 4b" `Component` cards)

**One filename that no longer matches its number.** `fig5_catjac.pdf` is
Figure **4** — the interpretation artifact renumbered its label but not its
assets. Copied under the original name so the provenance stays traceable.
(Figure 2 used to be in this list as `figure4_composite.pdf`; its replacement,
`Fig_2_Main.pdf`, matches its number.)

**Three supplementary figures are raster-only.** No PDF exists for
`supp_inference_pipeline`, `supp_similarity_clustermap_oriT` or
`supp_similarity_clustermap_oriV`, so they go in as PNG. Worth regenerating as
vector before submission.

**Page fit.** Every figure uses a `\fitfig` helper that caps height as well as
width (`width=\textwidth, height=0.72\textheight, keepaspectratio`), so none
can run off the page. Three would have overflowed at `width=\textwidth` alone:
`supp_reg_maplen` (would be 9.7 in tall), `supp_win_prcurves` (9.3 in) and
`supp_win_reliability` (9.1 in). Four more needed a tighter cap because the
figure is tall *and* the caption is long — `\fitfig` takes the fraction as an
optional argument: `figS1_orit` 0.66, `figS2_oriv` 0.60, `figS3_promoter` 0.64,
`figS4_crispr` 0.50. The compiled document reports no "Float too large"
warnings, so every figure and its caption fit their page.

**Long tables break across pages.** Three transcribed parameter tables run to
32, 73 and 50 rows, so Table 3, Supplementary Table S1 and Supplementary Table
S4 are `longtable`s with repeating headers rather than single-page floats.
`\usepackage{longtable}` was added to the preamble for this.

## Build

Verified with `latexmk -pdf main.tex` (pdflatex + biber, TeX Live 2025):
compiles clean at **58 pages**, with zero LaTeX errors, zero undefined
references, zero undefined citations, zero overfull vboxes, no overfull hbox
above 8 pt, and no oversized floats. Numbering resolves as intended — main
Figures 1–4 with Figure 3 the reserved placeholder, main Tables 1–3,
Supplementary Figures S1–S19 and Supplementary Tables S1–S4, all in order.

The two-bibliography split behaves as designed: skani is the only work cited
solely inside Methods, and it is the only entry under *Supplementary
References*; everything else prints under *References*.

Grepping the compiled PDF's text for the memo band names, the status-chip
vocabulary and `TODO cite` returns nothing, confirming no working-memo content
reached the page.

## Extended Data Figures (promoted 2026-09-01)

Nature Biotechnology displays Extended Data alongside the main figures online,
so five figures were promoted out of the Supplementary set, in their order of
appearance. They live in `sections/extended_data.tex`, `\input` from the
`\section*{Extended Data Figures}` block in `main.tex`.

| ED | Was Supp. Fig. | Asset | Content |
|---|---|---|---|
| 1 | S1 | `figS1_combined.pdf` | IMG/PR collection, corpus construction, cluster hierarchy, diversity-aware sampling |
| 2 | S3 | `supp_inference_pipeline.png` | window → intergenic-region origin-inference pipeline (retitled 2026-09-01; see §3 note above) |
| 3 | S6 | `supp_region_tracks_8.pdf` | calibrated window-probability tracks, additional regions |
| 4 | S7 | `supp_win_layer.pdf` | probe layer selection by validation AUPRC |
| 5 | S17 | `figS5_umap_feature.pdf` | embedding projection by coarse functional annotation |

Captions moved across byte-for-byte — GPN-Star uses the same convention for
both bands (no bold lead sentence, opening directly with the description).
Only the `\label` prefixes changed, `fig:supp-*` → `fig:ed-*`, so `\edfref`
reads correctly. None of the five captions carries a `\cite`, so although the
Extended Data section sits *before* `\maintextpartfalse` and its citations
would count as main-text, the two reference lists are unchanged.

**One cross-reference had to be split.** The interpretation artifact writes
"Supplementary Figs. S5–S7" as a single contiguous range over the three UMAP
panels. The first of those is now Extended Data Figure 5, so the range no
longer exists as one span; §5 now reads "Extended Data Figure 5 and
Supplementary Figures 13–14 …". A `% NOTE` at that line records why. This is
the only place where promotion forced a change to the transcribed wording, and
it changes only the pointer, not the claim.

## Global renumbering

Supplementary floats are numbered per-section in the artifacts and were
renumbered globally in order of appearance. All internal pointers moved with
them. The figure rows below are **after** the five Extended Data promotions.

| Global | Was | Content |
|---|---|---|
| Fig. S1 | pretraining S2 | windowing |
| Fig. S2–S8 | origin S2, S3, S6–S10 | clustermaps, PR curves, calibration, forest, length, per-split |
| Fig. S9–S14 | interpretation S1–S4, S6, S7 | *oriT* maps, *oriV* maps, promoters, CRISPR, two UMAPs |
| Table S1 | pretraining Table S1 | pretraining parameters |
| Table S2–S3 | origin Supp. Tables 1–2 | region metrics; window AUPRC |
| Table S4 | interpretation Table S1 | interpretation parameters |

Supplementary tables were not affected by the promotion. The origin artifact's
Tables 1–3 are **main-text** tables there and stayed main-text here.

## Bibliography

Added to `songlab.bib` under a `%%% GPN-Plasmids` banner, transcribed field by
field from the artifacts' own published reference lists — nothing inferred:
`camargo2024img`, `redondosalvo2020pathways`, `jain2018high`,
`kalchbrenner2016neural`, `mukherjee2023twenty`, `shaw2023fast`,
`islam2026plasann`. Reused from the existing bib: `devlin2018bert`,
`benegas2023dna` (GPN), `nguyen2024sequence` (Evo).

`jain2018fast` was already taken by a different Jain paper (MashMap2), hence
`jain2018high` for the ANI paper.

### Citations still owed — these need you

**The origin and interpretation artifacts ship no bibliography and no inline
citation markers at all.** There was nothing to convert for those two sections,
and no bibliographic data for the works they name exists anywhere I could read,
so none was invented. Every one is marked in place with `% TODO cite:`; grep
for that string. The full list:

*Introduction* — the bare `[ref]` on horizontal transfer of resistance /
virulence / metabolic traits (no work is named in the source); oriTfinder2;
OriV-Finder.

*Origin classification (§3)* — Evo2; PlasmidGPT; oriTfinder2; OriV-Finder;
BLASTN; oriTDB2; the *oriT*-diversity study (six consensus *oriT*s); the
*oriT*-transfer-network study (87 validated *oriT*s); MMseqs2; PLSDB;
Pyrodigal-gv; EMBOSS Water / Smith–Waterman; GraphPart; Addgene; AdamW;
isotonic regression.

*Model interpretation (§5)* — the categorical-Jacobian construction (cited in
prose only, as "the construction used to extract co-evolutionary signal from
protein and genomic language models"); PlasAnn (entry added as
`islam2026plasann`, but the artifact carries no marker — insert if wanted);
bioframe; the pSC101 and pIGRK primary sources; the pHTbeta, NAH7, R388,
pTA1040 and PA83 locus sources; UMAP (`mcinnes2018umap` already exists in
`songlab.bib`) and torchdr; RegulonDB; FIMO / MEME Suite; pymemesuite;
CRISPRCasFinder; ViennaRNA (the artifact's own note says cite it only if an
analysis using it is actually reported).

## Still placeholders

- §4 Genome-wide application and its reserved Figure 3
- Discussion — no artifact supplies one
- Extended Data Figures — no artifact supplies any; the heading is kept as a slot
- Supplementary Text — nothing maps to it
- Title, funding, acknowledgements, data/code availability, author
  contributions, correspondence — all inherited from the template
