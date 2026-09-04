# GPN-Plasmids — internal review, 2026-09-04

Produced by a full content review of the manuscript against the Nature
Biotechnology Article format and the scientific-writing skill's evidence-binding
and scientific-fidelity checks. Three independent audits were run over the
sources — numerical consistency, claim-versus-evidence, and structure/narrative
— and their findings were then verified by hand against the Methods before
being written down here. Where a finding did not survive that check it is
recorded in §5 rather than dropped, so the same claim is not re-raised later.

**Bottom line.** The manuscript's internal contradictions are fixed. What
remains are five issues that cannot be fixed by editing, because they are
claims the current experiments do not support. Three of them are cheap to
close. One of them is stated verbatim in your own Methods.

---

## 1. Blockers that need new runs

### 1.1 Layer selection leaks every test fold — CONFIRMED, stated in your Methods

`sections/supp_methods_03_origin.tex:156-159`, verbatim:

> "the reported layer was the one maximizing validation AUPRC **averaged over
> both tasks**, with each task's value itself the **mean over the five splits**.
> Layer selection therefore uses only validation columns, but **because parts
> rotate, each part is a validation set in one split and a test set in another**."

The selection statistic is averaged over all five validation folds. The union of
the five validation folds is the union of the five test folds. So the single
reported layer is chosen using information from every test fold, and every
number in Tables 1 and 2 is conditioned on it. The Results present those numbers
as clean held-out performance.

There is a second, independent leak in the same sentence: the layer is chosen on
**both tasks jointly**, so the *oriT* number is tuned partly on *oriV* test data
and vice versa.

**Fix:** nested selection — choose the layer per split using only that split's
validation fold, and per task. Re-report. If that is infeasible before
submission, say plainly in the Results that the layer is selected globally and
treat the comparison as indicative.

### 1.2 The "random-ranker floor" is the downsampling ratio — CONFIRMED

The Results and both table captions give 0.048 as the positive-window
prevalence. `sections/supp_methods_03_origin.tex:218` states negatives were
"sampled without replacement at **20:1**". And 1/(20+1) = 0.0476 ≈ 0.048.

So 0.048 is a property of the evaluation population you constructed, not of
plasmid sequence. On a real plasmid the origin-containing fraction of intergenic
windows is far lower, so AUPRC 0.78 against a 0.048 baseline does not transfer
to the database-wide application the paper claims. Nothing in the Results or
Discussion says this.

**Fix:** report window AUPRC at undownsampled prevalence for at least one split,
and state the negative-sampling procedure in the Results rather than only in
Methods.

### 1.3 The novelty threshold may not mean what the Abstract says — UNRESOLVED

`sections/supp_methods_03_origin.tex:214-215` defines similarity as "percent
identity × **query coverage** / 100", with novel meaning ≤ 20. **The Methods
never say what the query is.** If the query is the 512 bp scored window, as the
surrounding text implies, then a 100 bp origin that is 100% identical to a
training origin scores 100 × (100/512) ≈ 19.5 and is labelled novel. The metric
would then be unable to distinguish "no homolog" from "a short perfect homolog
inside a long window" — and short origins are the normal case here.

This is the load-bearing claim of the paper, so the ambiguity has to go either
way.

**Fix:** state the query explicitly in Methods. If it is the window, normalise by
**origin** length instead and recompute Figure 2c, or report the distribution of
best-hit identity and length among windows called novel.

### 1.4 Diversity-aware sampling is never ablated

It is named in the title framing, the abstract, the Introduction and the
Discussion's opening sentence as the reason the model works. All supporting
evidence is descriptive statistics of the sampler itself (715× → 4.0×, Fig. 1e).
No model was trained with sequence-uniform or one-per-PTU sampling, so nothing
connects the scheme to any downstream number.

**Fix:** one matched ablation at reduced step budget — same architecture, same
token budget, uniform sampling — reporting probe AUPRC and mAP. Absent that, the
sampler must be described as a design choice, not as a cause of the results.

### 1.5 The interpretation section has no null model

The categorical Jacobian of a randomly initialised model, or of shuffled
sequence, is never shown. Periodic and anti-diagonal structure at repeats could
arise from the convolutional prior alone. Compounding this, Methods record that
the displayed panels were selected partly on the model output, and the only
quantitative test anywhere in the section is negative.

**This is the cheapest blocker to close.** Running the categorical Jacobian on an
untrained checkpoint over the two main-figure loci is a single afternoon and
would earn the current claim outright.

---

## 2. Fixed in this pass

Every item here was a place the manuscript contradicted itself. Nothing was
softened on taste.

| Where | Was | Now |
|---|---|---|
| Abstract | "representations that organize plasmid DNA by function" | association with functional annotation, matching what §5 and the Discussion actually allow |
| Introduction | "isolates origins with no training homolog" | "below an operational similarity threshold", matching Methods |
| Introduction | reference tools "effectively blind" | narrowed; Table 1 shows OriV-Finder at 0.73, competitive |
| Discussion | ensemble gain "indicates" non-redundancy | "consistent with", and variance reduction named as an equally good explanation |
| Discussion | four promoters "from a motif scan" | three from the scan, one by consensus substring match, per Methods |
| §2.3 | "every output position conditions on the whole window" | nominal receptive field; the effective field is smaller and unmeasured |
| §3.1 | homology separation stated flatly | restored: 25 of 112 *oriT* origins reinserted, so that split is similarity-aware |
| §3.1 | label provenance absent | added: positives were located by BLASTN, so every one is alignment-findable by construction |
| §2.2 | misclassified fragments "tend to survive" | "may survive"; the rate is never measured |

The last two in §3.1 were cut by me in the earlier length trim as Methods
detail. That was a misjudgement and they are back.

Also fixed earlier in the session: AUPRC appeared 11 times in the displays and
zero times in the main text; the abstract's "no detectable homology"; a missing
Discussion citation; and the availability statements, which were one-line
placeholders and are now an itemised checklist.

---

## 3. Decisions only you can make

1. **"Internalized the cis-regulatory grammar."** Flagged inline in
   `01_introduction.tex` and on the §5 heading, deliberately not changed. Either
   run the null model in §1.5 and keep it, or retitle. A short accurate
   alternative that preserves the heading set: "Dependency maps align with
   annotated repeats".
2. **oriTfinder2 may be run outside its design envelope.** It uses relaxase and
   secretion-system gene context on whole plasmids; the harness feeds it isolated
   intergenic regions. A tool scoring below a longest-region rule on its own task
   usually indicates harness mis-specification. The Introduction's motivating
   premise rests on that number. The existing ablation varies the database
   contents, not the invocation mode, so it does not address this.
3. **The Evo2 scaling claim.** The non-monotonicity sentence still carries its
   original sign-off flag, and the differences it rests on sit inside the
   split-to-split spread on n = 5.
4. **§4 forward references.** Six places assert the genome-wide result as
   completed fact. You have said a co-author is adding the section; those
   sentences are marked so they are not lost, but if the section slips they all
   have to change together.

---

## 4. Worth doing, not blocking

- **The paper never makes its own best argument.** §5 shows the model internalised
  repeat and promoter architecture; §3 shows the probe generalises to unfamiliar
  origins. The second is explained by the first, and that is exactly why a
  language model should beat an alignment tool. No sentence connects them. One
  sentence in §5's closer and one in the Discussion would convert §5 from a
  curiosity into the mechanistic argument that carries the paper.
- **Pretraining exposure of the benchmark is never addressed in the main text.**
  The holdout is 3 PTUs and 6 sequences out of 693,638. Whether the 336 curated
  origins' plasmids were in the pretraining corpus is the first question a
  reviewer of a language-model paper asks.
- **No pretraining quality metric anywhere.** No perplexity, held-out masked-token
  loss, or loss curve. Training stopped at 375,000 of a configured 800,000 steps
  with no convergence evidence.
- **The Discussion does not position the work.** MOB-suite, PlasmidFinder, geNomad
  and oriTDB appear nowhere in it, and nothing tells a reader how to use a ranked
  candidate list: no operating point, no expected false-discovery rate.
- **Two missing baselines**, both cheap: a GC/AT-composition region ranker, and a
  raw BLASTN ranker. Without the first, nothing separates "learned origin
  architecture" from "learned local composition", which matters because a
  supplementary figure reports the embedding projection varies strongly with
  composition.
- **"Genome-wide" is the wrong word** for an analysis across many plasmids. The
  abstract already says "database-wide".
- **Two promoter panels use a different checkpoint** from the released model
  (`v3_noflex_295k` vs `375k`), disclosed only in a parameter table.
- **A supplementary panel's caption disowns its own label** as "a motif assignment
  the project has since retracted". Regenerate it.
- **Probes were trained with no fixed seeds**, so no reported number is exactly
  reproducible.

---

## 5. Audit claims that did NOT survive verification

Recorded so they are not re-raised.

- **"Evo2 best layers sit at the low edge of a truncated sweep."** Not supported.
  Sweeps were 10–24, 10–31 and 10–22 for 1B, 7B and 40B; the selected layers were
  15, 15 and 16, comfortably inside. **The real asymmetry is the opposite one:**
  `supp_methods_03_origin.tex:160` records that \model probes were trained on
  layers 36–72 but "reporting considered GPN layers 60–72", and the selected
  layer is **60 — exactly the lower edge of the reporting window**. A better
  \model layer may exist in 36–59 and have been excluded. This understates your
  own model rather than overstating it, but the restriction needs a stated reason.
- Corpus arithmetic, the quoted metric values, the 85× and 15× parameter ratios
  and the 45× latency ratio were all checked and reconcile.
- The exploratory negatives in §5 (pIGRK IR1/IR2, the NAH7 null, the IR4 interval
  reported as post-hoc and non-significant) are handled correctly and should be
  kept as they are.

---

## 6. Numerical audit (added 2026-09-04)

The numerical pass rendered the figure PDFs and compared artwork against text,
so several of these are text-versus-artwork conflicts that no source-only check
would find.

### 6.1 Fixed

| Issue | Resolution |
|---|---|
| "Ranking accuracy is broadly stable across plasmid-length bins" | Contradicted by the figure: *oriT* mAP runs 0.93 / 0.95 / 0.89 / **0.69** across the four bins. Now says stable up to 100 kb and falling in the largest bin. Long plasmids are where a database-wide run spends its effort, so this matters. |
| Reference-tool ablation "at most 0.007" | True of the **mean** only. Per-split differences reach 0.024 for *oriT*, and the figure it cites is a per-split heatmap. Scoped to the mean, with the per-split range stated. |
| "Eight additional origins … spanning three architectural classes" | The three classes describe only the four *oriV* panels. Scope corrected. |
| Table 1 caption, "per-split spread of every method" | Supplementary Table 2 omits the train-plus-validation reference-tool row. Caption no longer overpromises. |
| Epoch count 102 | 217 M / 2.14 M = 101.4. Corrected to 101; the exponent used elsewhere needs to follow. |
| Methods referring to "each promoter panel title" | The artwork has no panel titles, and panel **d** carries no information-content value at all. Reference corrected; the missing value still needs supplying. |
| My own provenance comments | Two claimed content had moved to Methods when it had not. The DR2/P*mobK* spacer clause is now genuinely in Methods; the other comment is corrected. |

### 6.2 A flag of mine that was wrong — withdrawn

I had flagged the best one-hot split on novel windows, 0.28, as "looking
transposed" because it equals that method's overall test mean of 0.277. **That
comparison was wrong.** Figure 2c puts the one-hot *novel-window* means near
0.18–0.21, so a best novel split of 0.28 sits correctly above its own novel
mean, and the match with 0.277 is coincidence. Both numbers are restored as
originally transcribed, and the qualitative hedge I substituted is gone. No
action needed from you — recorded so the number is not doubted again.

### 6.3 Unresolved — these need you

1. **The Moderate-PTU count is arithmetically impossible as printed.** Methods
   give 24,850 Moderate PTUs in one place and 24,848 Moderate representatives
   per epoch in another. Moderate allocation is deterministic — exactly one
   representative per PTU, zero variance — so the two must be equal. Fixing it
   to 24,850 makes the epoch total 166,**194**, which then contradicts the
   166,192 printed in the Figure 1d caption and in Methods. One of the three
   numbers has to change and the other two follow. (The Unique arm checks out
   exactly: ⌊0.564 × 147,672⌋ = 83,287.)
2. **Figure 2a and the Methods describe opposite cross-validation rules.** The
   artwork shows split 1 as train parts 1–3, validate part 4, test part 5. The
   Methods say for split *N*, part *N*−1 tests and part *N* validates, which
   makes test = validate − 1. The figure has test = validate + 1. One is wrong,
   and it describes the benchmark.
3. **Figure 2f ships a "pending" cell** for the Evo2-40B timing. A main display
   item cannot go out with that in it. You have said you are adding the number;
   the Introduction's 85×/45× clause is already marked to follow it.
4. **pTA1040: which stem-loop is omitted.** Caption and Methods both say the
   source's *fifth* loop is not shown. The artwork labels the four drawn loops
   IR2–IR5, which implies the *first* was dropped. One of the two is wrong.
5. **Figure 1d artwork still reads "~160k"** against 166,192 in text and caption.
   Still live.
6. **If the *oriT* count changes from 87 to 91**, the stated total of 142
   experimental sequences must become 146. The two move together.
7. **Supplementary Table 2 gives PlasmidGPT *oriT* P@1 as 0.319; the figure shows
   0.318.** The table is right — the per-split values average to 0.3186 — so the
   figure appears to truncate rather than round.
8. **Extended Data Figure 1d attributes the 10.1 kb median to the full
   collection**, while Methods give it for the training corpus. The populations
   differ by 0.9%, so this is probably benign, but the same figure is attributed
   to two different sets.

### 6.4 Checked and correct

Every Results metric reconciles with Tables 1–2 and Supplementary Tables 2–3 at
the two-decimal convention. The parameter count, the 6,049 bp receptive field,
the 85× and 15× parameter ratios, the 44.8× latency ratio, the whole filtering
chain down to 693,638, the PTU category percentages, and every crop interval in
every interpretation caption were verified against the rendered artwork and are
correct.

---

## 7. The uncited background claim, resolved (2026-09-04)

The Introduction asserted, with no citation, that "pretraining on the wrong
sequence distribution can transfer worse than not pretraining at all". A
literature check found the claim **splits into two halves that are not equally
supported**:

- **"gLM pretraining does not automatically beat simple baselines"** — well
  established. Tang et al. 2025 (*Genome Biology*) report that highly tuned
  from-scratch one-hot models match or beat pretrained DNA language models on
  regulatory tasks, and DART-Eval (NeurIPS 2024) finds current DNA language
  models "do not offer compelling gains over alternative baseline models for
  most tasks, while requiring significantly more computational resources".
- **"A mismatched corpus is worse than no pretraining at all"** — **no published
  genomics paper shows this.** The closest published result is Gu et al. 2022,
  in biomedical NLP, where pretraining from scratch beats continual pretraining
  from a general-domain model. Zoph et al. 2020 show pretraining actively
  hurting, but that is a label-regime effect in vision, not corpus mismatch.

The sentence now claims only the supported half, carries four verified
citations, and signposts the stronger half as this paper's own finding. That is
honest, and it is also better structure: your PlasmidGPT result currently
arrives unheralded in a subsection about inference cost, and this sets it up.

Your own manuscript is in fact the best available evidence for the strong half.
`supp_methods_03_origin.tex` records that all PlasmidGPT layers were swept and
its best still underperformed the from-scratch CNN, with PlasmidGPT pretrained
on engineered Addgene constructs. Worth making that point explicitly rather than
leaving it as a table row — subject to the confound noted in §3, that PlasmidGPT
also differs in tokenization, objective and the embedding upsampling step.

### Bibliography hygiene found along the way

- **`tang2024evaluating` is the superseded two-author bioRxiv preprint** of the
  Tang 2025 *Genome Biology* paper. It is now marked superseded in place and the
  published four-author version added as `tang2025evaluating`. Nothing in this
  manuscript cited the preprint, but another might.
- **`robson2023guanine` is the superseded GUANinE v1.0 preprint**; v1.1 is
  published in *Bioinformatics*. Not cited here.
- **`marin2024bend` would be a stretch** for this claim. BEND's actual finding is
  that gLM embeddings can *approach* expert methods, not that they lose to
  simple baselines.

### Optional addition

ConvNova (ICLR 2025) shows a well-designed CNN outperforming DNA foundation
models on more than half of the tasks across several benchmarks, with fewer
parameters. That is directly relevant to your architecture choice, not just to
this sentence. It is not added, because its author list differs between the
arXiv and ICLR versions (Daniel vs Yanjun Shao) and that should be settled
before citing.

### Do not cite for this claim

"Are Genomic Language Models All You Need?" matches the search pattern but its
result is **positive** — gLMs are competitive with and sometimes beat protein
language models. Citing it as negative evidence would be a misattribution.
