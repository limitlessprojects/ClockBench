# ClockBench

**The living, neutral, leakage-aware benchmark for epigenetic aging clocks.**

> Same clock. Different packages. Different ages.
> We measure that — and everything else the papers leave out.

Epigenetic clocks now decide consumer health purchases, stratify cohorts, and
are entering clinical trials — yet the field has **no neutral referee**: every
clock is evaluated by whoever packaged it, on whatever data they chose, with
whatever preprocessing they assumed. The result is documented across the
ecosystem's own trackers:

- [`pyaging`#51](https://github.com/lucascamillomd/pyaging/issues/51) — missing
  CpGs silently filled with **0** for 67 of 91 linear clocks (imputation policy
  changes results, nobody reports which policy they used)
- [`biolearn`#220](https://github.com/bio-learn/biolearn/issues/220) —
  PhenoAge coefficients **differ from Levine et al. 2018** (the source paper)
- [`methylclock`#38](https://github.com/isglobal-brge/methylclock/issues/38) —
  **same dataset, different DNAmAge across package versions**
- [`methylclock`#40](https://github.com/isglobal-brge/methylclock/issues/40) —
  open question: should batch correction precede clock calculation? (unresolved)

Papers compare **clocks** ([Mavrommatis 2025, Nat Commun](https://www.nature.com/articles/s41467-025-66106-y);
[Apsley 2025, Aging Cell](https://onlinelibrary.wiley.com/doi/10.1111/acel.70060)).
Nobody systematically compares **implementations of the same clock** — the gap
between the clock you cite and the number your pipeline prints.

## The five lanes

| Lane | Question | Status |
|---|---|---|
| A — Accuracy | MAE / correlation of each clock vs chronological age, out-of-sample only | spec v0.1 |
| B — **Parity** | same clock × N implementations (pyaging / methylclock / biolearn / original R) × same input → divergence score | **the core novel lane** |
| C — Robustness | sensitivity to normalization family, imputation policy, batch perturbation | spec v0.1 |
| D — Calibration & subgroups | calibration curves, ECE, slices by sex / age decile / array type | spec v0.1 |
| E — Portability | 450k vs EPIC vs mouse/mammalian array; cross-tissue | v0.2 |

## Ground rules (what makes this a benchmark, not a blog post)

1. **Leakage ledger** — every clock × dataset pair carries a documented
   train/eval overlap flag. Clocks are scored on data their training did not
   see, or the overlap is stated on the scoreboard itself.
2. **Versioned manifest** — datasets pinned by accession + role in
   [`datasets/manifest-v0.1.yaml`](datasets/manifest-v0.1.yaml); results
   reference manifest versions, not vibes.
3. **Signed artifacts** — every result run ships input hashes + code commit;
   the scoreboard is reproducible from the archive.
4. **Neutrality** — no clock author grades their own clock; contested calls go
   to public changelog with reasons.
5. **Honesty clause** — negative results (a clock that fails calibration, a
   parity divergence that invalidates cross-study comparisons) are published
   as first-class results. See [THESIS.md](THESIS.md) for what we are and are
   not claiming.

## The field's open problem, on the record

The FDA Biomarker Qualification Program has qualified **8 biomarkers total —
none is an epigenetic clock** — while clocks already appear in phase-2a trial
readouts. Individual-level clinical use is contested on preprocessing and
implementation grounds (Apsley et al. 2025, *Epigenomics*). The measurement
layer needs infrastructure before it gets credibility. That infrastructure is
this repository.

## How to join (90 days, concrete)

- Clock developers: add your implementation to the parity lane —
  [`CONTRIBUTING.md`](CONTRIBUTING.md). Your clock, graded fairly, in public.
- Package maintainers: we file parity bugs upstream with minimal reproducers
  instead of scoreboard dunking.
- Labs: nominate held-out datasets for the manifest (accession + metadata).
- Everyone: read [`specs/benchmark-spec-v0.1.md`](specs/benchmark-spec-v0.1.md)
  and tear it apart in Issues. A spec that survives review becomes v1.0.

## Related infrastructure (we build on, not against)

[pyaging](https://github.com/lucascamillomd/pyaging) (Bioinformatics 2024) ·
[biolearn](https://github.com/bio-learn/biolearn) ·
[methylclock](https://github.com/isglobal-brge/methylclock) (Bioconductor;
its v2 clock registry is the model for machine-readable clock specs) ·
[ComputAgeBench](https://www.biorxiv.org/content/10.1101/2024.06.06.597715v2)
(the static scoreboard that proved the demand — this repo maintains what a
paper cannot) · [dglubokov/clocks](https://github.com/dglubokov/clocks)
(reproducible teaching course).

*A [Limitless Projects](https://github.com/limitlessprojects) build.*
