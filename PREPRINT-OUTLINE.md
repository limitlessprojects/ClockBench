# Preprint outline (v0.1 — one page)

**Working title:** "Implementation, not biology: cross-package parity of
epigenetic aging clocks on public methylation data"

**Question.** When two pipelines report the "same" clock (Horvath2013,
Hannum2013, PhenoAge, GrimAge, DunedinPACE) on the same public methylation
sample, how large is the implementation-induced divergence — and how does it
compare to the divergence between different clocks?

**Why it matters.** Clocks are entering trials and consumer health with no
FDA-qualified epigenetic biomarker and contested individual-level validity.
If implementation variance is comparable to clock choice, then cross-study
contradictions attributed to "the clock" may be artifacts of the pipeline.

**Data.** Manifest v0.1: GSE40279, GSE87571, GSE112618 (whole blood, 450k/EPIC,
n>1400 samples combined), expanded via biolearn's catalog. Leakage ledger
documenting train/eval overlap per clock x dataset.

**Method.** Parity lane: each clock x each implementation (pyaging, methylclock,
biolearn, original R) on identical inputs with per-implementation preprocessing
contracts recorded. Divergence score D = max pairwise |age_i - age_j| (median,
p95, max). Robustness lane: normalization family, imputation policy, masked-CpG
perturbations. Calibration lane: slope/intercept + ECE, subgroup slices.

**Expected result (pre-registered).** H1: median cross-implementation
divergence of the same clock exceeds 1 year on at least one clock. H2: for at
least one clock, implementation divergence exceeds the gap between that clock
and the next-best clock's accuracy. H3: imputation policy alone (zero-fill vs
reference-based) shifts mean predicted age by >= 0.5 years on >= 2 clocks.
Every H is falsifiable and published either way.

**Target venue.** bioRxiv preprint -> *Genome Biology* / *Nature Methods*
brief communication or *GigaScience* (infrastructure-friendly).

**Artifacts.** Manifest, harness, signed result archives, and the scoreboard
live in this repository before, during, and after review.
