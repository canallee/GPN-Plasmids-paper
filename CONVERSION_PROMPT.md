# GPN-Plasmids paper — artifact → LaTeX conversion prompt

Hand this file to the session that builds the LaTeX manuscript. It converts three existing
"companion artifacts" into the LaTeX template in this repo. **The artifacts are the source of truth;
the LaTeX must STRICTLY MIRROR their material and change ONLY layout, numbering, formatting, LaTeX
markup, and cross-referencing — never the scientific content, wording, numbers, or claims.**

---

## 0. The template (read its conventions first)

The manuscript lives in **`GPN-Plasmids/`** (this repo):

- `GPN-Plasmids/main.tex` — the manuscript; populate it (and any `\input` files you add).
- `GPN-Plasmids/songlab.bib` — the bibliography.
- `GPN-Plasmids/figures/` — currently empty; put figure assets here.
- `GPN-Plasmids/README.md` — **READ THIS FIRST.** It documents the LaTeX conventions you MUST
  follow: the `\model` macro (use it instead of typing "GPN-Plasmids"), the cross-reference
  wrappers (use them instead of hardcoding section/figure numbers), the **two-bibliography setup**,
  and the author-hiding switch. Do not fight or restyle these — inherit them.
- `reference/GPN-Star/main.tex` — the paper this template's formatting is derived from. Use it as the
  **layout reference** for how sections, figures, Methods, Supplementary, and the two bibliographies
  are structured. Match that structure; do not invent a different one.

Build with: `cd GPN-Plasmids && latexmk -pdf main.tex` (TeX Live + biber, or Overleaf). The document
must compile before you finish.

---

## 1. Inputs — the artifacts

Flow reference (authoritative section↔figure map — read first):
`/home/canall/DNA_LM/plasmids_GPN/manuscript/paper_flow.md`

Artifact manuscript spines (the material to transcribe):

| § | Section | Main fig | Source |
|---|---|---|---|
| 2 | Pretraining | Figure 1 | `/home/canall/DNA_LM/plasmids_GPN/manuscript/artifact/page.html` — clean markdown source is `…/manuscript/pretraining/*.md` (01 Results, 02–06 Methods, 07 references; **skip 00_index — it's a memo**) |
| 3 | Origin classification | Figure 2 | `/home/canall/DNA_LM/BEND_ori_intervals/manuscript/artifact_origin/page.html` |
| 5 | Model interpretation | Figure 4 | `/home/canall/DNA_LM/plasmids_GPN/manuscript/artifact_interpretation/page.html` |

Figure assets live under each artifact's `figures/` directory. Copy the ones you use into
`GPN-Plasmids/figures/` and reference them there — see **Rule 5** for the full figure policy (complete
figures only, never the individual component panels; copy every asset into the repo; no edits).

The artifacts are still being edited; treat this conversion as a **snapshot of the current versions**
and keep re-syncing cheap (see Rule 4). The interpretation section's wording is frozen by the author
("come back later") — transcribe it exactly, do not fix it.

---

## 2. Section & figure order (LaTeX must auto-number to these)

```
§1 Introduction ..................... PROVIDED — transcribe verbatim from §8 below
§2 Pretraining ...................... main figure = Figure 1
§3 Origin classification ............ main figure = Figure 2
§4 Genome-wide application (Spencer)  PLACEHOLDER; reserves Figure 3
§5 Model interpretation ............. main figure = Figure 4
Abstract ............................ PROVIDED — transcribe verbatim from §7 below
```

Place sections and figure floats in this order so LaTeX auto-numbers §1–§5 and Figures 1–4. For
§4 / Figure 3 (unwritten) insert a clearly-marked placeholder section and a placeholder figure float
so the interpretation figure still lands on Figure 4. Use `\label`/`\ref` (or the template's
cross-ref wrappers) everywhere — **no hardcoded section or figure numbers.**

---

## 3. Hard rules

**1. FIDELITY.** Transcribe each section's manuscript spine verbatim. You may change only:
HTML→LaTeX markup (`<em>`→`\emph`, entities→LaTeX, the `.eq`/`<div>` equation blocks → real LaTeX
math), section/figure/citation numbering, float placement, and the model name → `\model`. You may
NOT paraphrase, summarize, reorder, "improve," or correct anything in the science — including the
interpretation section. If a passage reads oddly, transcribe it as-is and add a `% TODO` comment;
do not fix it.

**2. LaTeX CONTAINS MANUSCRIPT CONTENT ONLY.** Include exactly these bands from each artifact:
Main text (Results prose), Figures + captions, Tables + captions, Methods, Supplementary
(figures/tables + captions), and References. Everything else is working memo and MUST NOT be
converted — it never appears in the paper. Concretely, DROP these sections/blocks wherever they occur
(origin and interpretation still carry them; pretraining does not):
  - "Decisions needed" and any decision callouts
  - "Positioning notes" — including *Contribution stack*, *Method taxonomy*, *Claim / don't-claim
    ledger*, and *Reviewer pre-emptions*
  - "Open items" — including *Blockers* and *"Limitations (state proactively)"* (these are reminders,
    not the manuscript's own limitations prose)
  - "Code references"
  - action items, reminders, to-dos, status chips (exact/approx/planned/…), and the dashed `.why`
    provenance notes beneath figures

Rule of thumb: if it is advice to the authors, a plan, a reviewer-strategy note, a code pointer, or a
status marker — it is NOT manuscript material and does not go into the LaTeX. If unsure whether a
block is manuscript vs. memo, leave it OUT and add a `% NOTE` listing what you skipped so the author
can confirm.

**3. PLACEHOLDERS & PROVIDED TEXT.** Only the genome-wide-application section (§4, Spencer's) is a
placeholder — a one-line `\section` stub plus a visible `% PLACEHOLDER — to be written/uploaded by
author` comment; do not draft or invent its content. The **Abstract is provided in §7** and the
**Introduction (§1) in §8** — transcribe both verbatim (Abstract into the abstract environment; §1 as
the Introduction section). The fidelity rule applies: only LaTeX markup, the `\model` macro, and
`[ref]`→`\cite{…}` conversion may change — do not reword, reorder, or trim. In the Introduction,
convert each `[ref]` marker to the matching `\cite{…}` (from the merged bibliography), or leave a
`% TODO cite: …` where the reference is not yet present.

**4. MODULAR + TRACEABLE.** One `.tex` file per section (e.g. `sections/02_pretraining.tex`,
`03_origin.tex`, `05_interpretation.tex`, plus intro/genomewide placeholders and methods/supplementary
as the template's structure dictates), `\input` from `main.tex`. At the top of each, a comment naming
its **source artifact + path + version/date**, so a section can be re-synced when its artifact
changes.

**5. FIGURES — complete figures only; copy every asset into the repo.** The artifacts often display a
section's assembled figure AND its individual component panels as separate cards. The complete figure
is marked "Final" / "Assembled"; the extras are marked "Component" / "standalone panel" — e.g. the
interpretation artifact shows the composite **Figure 4** plus "Fig. 4a" and "Fig. 4b" standalone
panels, and origin shows **Figure 2** plus per-panel cards. The paper uses ONLY the complete assembled
figure for each main and supplementary figure — **DROP every individual component / standalone-panel
card.** And **COPY every figure asset you use into `GPN-Plasmids/figures/`** (prefer the PDF/vector
file over the PNG), then reference it from there with `\includegraphics`. Do not point
`\includegraphics` at the source artifact repos, and do not edit, regenerate, resize, or recolor any
figure — copy the asset in as-is.

---

## 4. Bibliography

Each artifact has its own inline citations and reference list. Merge them into the template's
bibliography (`songlab.bib`, following the README's two-bibliography setup): DEDUPLICATE shared
entries (IMG/PR, Evo, GPN, BERT, ByteNet, GOLD, skani, oriTfinder2, OriV-Finder, GraphPart, etc.
recur across sections), give stable cite keys, reuse any entry already present in `songlab.bib`, and
convert every inline superscript citation in the artifacts to `\cite{…}` (or the template's citation
command). Preserve every reference that appears in any artifact — drop none, add none. Route
main-text vs. methods/supplementary citations into whichever of the two bibliographies the README
specifies.

---

## 5. Methods & Supplementary

- **Methods:** follow the template/GPN-Star convention for placement (inline vs. a single end-of-paper
  Methods block). Transcribe each artifact's Methods faithfully, with equations as real LaTeX math —
  the pretraining `*.md` files already hold LaTeX math you can reuse; convert the interpretation
  artifact's `.eq` blocks to LaTeX.
- **Supplementary:** collect every supplementary figure/table from all three artifacts, RENUMBER them
  globally in order of appearance (S1, S2, …; the artifacts number per-section), update all
  supplementary cross-references, and carry captions verbatim. Place them per the template's
  supplementary structure.

---

## 6. Verify before finishing

- The document COMPILES (`latexmk -pdf main.tex`); fix only LaTeX/layout errors, never content.
- Section numbers resolve to §1–§5 and main figures to Figure 1–4 in the intended order; §4 and
  Figure 3 are visible placeholders; the interpretation figure is Figure 4.
- Spot-check three or four paragraphs against the source artifacts — text must be identical, only
  markup/numbering different.
- No "Decisions needed" / "Positioning notes" / "Open items" / "Code references" / status-chip /
  `.why` content leaked in (grep the built `.tex` for those band names to confirm).
- All inline citations resolve against the merged bibliography; no broken `\ref`/`\cite`; the
  `\model` macro and cross-ref wrappers are used, not hardcoded strings/numbers.
- Every figure is the complete assembled figure (no "Component" / standalone-panel figures), and
  every figure asset is copied into `GPN-Plasmids/figures/` and referenced locally (no external
  artifact-repo paths in `\includegraphics`).

**Deliverable:** a compiling LaTeX paper in `GPN-Plasmids/` with §2/§3/§5 fully transcribed from the
artifacts, the Abstract from §7 and the Introduction from §8, §4 the only placeholder, merged
bibliography, main Figures 1–4 (Fig 3 reserved) and globally-numbered supplementaries, and a short
note (comment or `CONVERSION_NOTES.md`) recording which artifact + version each section came from.

---

## 7. Abstract — transcribe verbatim into the abstract environment

Use this as the paper's Abstract, a single paragraph. Fidelity rule applies: change only to LaTeX
markup — italicise *oriT* / *oriV* (`\emph{}` or `\textit{}`), keep "×" as-is, and render the model
name with the `\model` macro. Do not reword, reorder, or trim.

> Plasmids drive bacterial adaptation and are central tools for engineering microbes, yet the
> non-coding elements that govern their transfer and replication — origins of transfer (*oriT*) and
> vegetative replication (*oriV*) — remain unannotated across the metagenomic majority of plasmids.
> Existing annotation methods are reference- and alignment-based, so they fail on the divergent,
> host-uncharacterized sequences that now dominate plasmid databases. We present GPN-Plasmids, a
> genomic language model trained by masked-nucleotide prediction on the plasmid sequences of the
> IMG/PR database with a diversity-aware sampling scheme that captures the breadth of metagenomic
> plasmid diversity rather than sequencing depth. Without any labels, GPN-Plasmids learns
> representations that organize plasmid DNA by function. For identifying origin-containing regions, a
> lightweight classifier on its frozen embeddings generalizes to origins with no detectable homology
> to the training data, outperforms reference-based tools and models trained from scratch, and matches
> genomic language models up to 85× larger at roughly 45× less inference compute. Applied across
> IMG/PR, it annotates putative origins genome-wide and reveals previously undescribed transfer and
> replication gene architectures. Interpreting the model shows that it has internalized the
> cis-regulatory architecture of *oriT*, *oriV*, promoters and other functional elements at
> single-nucleotide resolution. GPN-Plasmids offers a scalable, alignment-free route to annotating the
> functional elements of diverse and uncultured plasmids.

---

## 8. Introduction (§1) — transcribe verbatim

Use this as the paper's Introduction (~4 paragraphs). Fidelity rule applies: change only to LaTeX
markup — italicise *oriT* / *oriV*, render the model name with the `\model` macro, convert each
`[ref]` to the matching `\cite{…}` (or a `% TODO cite: …` note), and render the `(Fig. 1a,b)` /
`(Fig. 1)` pointers with the template's figure cross-reference wrappers. Do not reword, reorder, or
trim.

> Plasmids are mobile, extrachromosomal DNA elements that shape bacterial evolution and are central
> tools of microbial engineering. By transferring genes horizontally between cells they spread traits
> — antibiotic resistance, virulence, and metabolic capabilities — between otherwise unrelated
> bacteria [ref], and by replicating independently of the chromosome they persist as stable accessory
> genomes. The two processes that define a plasmid's life cycle also make it useful in the laboratory:
> conjugative transfer moves engineered DNA between hosts, and vegetative replication maintains it.
> Each is governed by a short, non-coding cis-regulatory element — the origin of transfer (*oriT*),
> where a relaxase nicks the plasmid to initiate conjugation, and the origin of vegetative replication
> (*oriV*), where a replication initiator binds to begin replication (Fig. 1a,b). Identifying these
> origins is therefore a prerequisite both for understanding how a plasmid moves and replicates and
> for repurposing it as an engineering vector.
>
> Metagenomic sequencing has expanded the known plasmid universe by orders of magnitude, cataloguing
> hundreds of thousands of plasmid sequences — billions of nucleotides — most from uncultured lineages
> with no characterized host [ref, IMG/PR]. Yet the elements that make these plasmids work remain
> largely unannotated, and origins are among the hardest features to find. *oriT* and *oriV* are
> short, intergenic, and structurally diverse, defined less by conserved sequence motifs than by
> architecture — iterons, inverted repeats, and paired promoter boxes whose spacing and symmetry,
> rather than their exact bases, carry function. The tools used to annotate them (oriTfinder2 [ref],
> OriV-Finder [ref]) are reference- and alignment-based: they recognize an origin by its similarity to
> a curated set of experimentally characterized examples, so they degrade sharply on the divergent
> origins that dominate metagenomic plasmids and are effectively blind to origins unlike the
> reference set. The result is a widening gap between the plasmids we can sequence and the ones we can
> functionally interpret — a gap that most constrains exactly the non-model, uncultured plasmids of
> greatest interest for discovery and engineering.
>
> Genomic language models (gLMs) offer a route past this reference dependence. Trained by
> self-supervision to predict masked or successive nucleotides on unannotated sequence, they learn
> representations that transfer to functional tasks and to sequences never seen in training [ref, GPN;
> ref, Evo]. This ab initio generalization is precisely what origin annotation on divergent plasmids
> demands. General-purpose gLMs, however, are trained across broad evolutionary scales and dominated
> by chromosomal genomes; plasmids — mobile, recombining, compositionally distinct, and lacking any
> universal taxonomy — are underrepresented, and pretraining on the wrong sequence distribution can
> transfer worse than not pretraining at all. Building a gLM for plasmids also raises a sampling
> problem absent for cellular genomes: plasmids have no hierarchical classification to sample across,
> and abundance in sequence databases reflects sequencing effort rather than biological diversity, so
> naive training would over-fit a handful of heavily sequenced clades.
>
> Here we present GPN-Plasmids, a genomic language model trained by masked-nucleotide prediction on
> the plasmid sequences of the IMG/PR database. To learn from the true diversity of the metagenomic
> plasmid universe rather than its sequencing depth, we designed a diversity-aware sampling scheme
> that weights training toward the breadth of plasmid diversity rather than sequencing depth (Fig. 1).
> Using GPN-Plasmids, we build a benchmark and a deployable pipeline for plasmid-origin annotation: a
> lightweight classifier on the model's frozen embeddings identifies origin-containing regions and,
> evaluated on a homology-partitioned benchmark that isolates origins with no training homolog,
> generalizes ab initio — outperforming reference-based tools and models trained from scratch, and
> matching genomic language models up to 85× larger while using roughly 45× less inference compute.
> Applied across all of IMG/PR, the classifier annotates putative *oriT* and *oriV* genome-wide and
> reveals previously undescribed transfer and replication gene architectures. Finally, interpreting
> the trained model shows that it has internalized the cis-regulatory grammar of these elements — the
> repeats, iterons, and promoter couplings of *oriT* and *oriV*, together with promoters and other
> functional elements — directly from sequence and without supervision. Together, these results
> establish GPN-Plasmids as a scalable, alignment-free foundation for annotating and engineering the
> functional elements of diverse and uncultured plasmids.
