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
