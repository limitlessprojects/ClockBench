# ClockBench Benchmark Specification v0.1

Status: **draft for public review** — tear it apart in Issues; v1.0 freezes
after the first parity run reproduces.

## 0. Neutrality policy

- No implementation's maintainer scores their own implementation in the
  parity lane; cross-review is mandatory.
- Every contested scoring decision gets a public changelog entry with reasons
  and an appeal path (open an Issue tagged `governance`).
- All result artifacts are signed (sha256 of inputs + code commit) and
  reproducible from the archive. No unverifiable numbers on the scoreboard.

## 1. Clocks under evaluation (v0.1 roster)

| Clock | Source | Coefficients provenance | Known training overlap risks |
|---|---|---|---|
| Horvath2013 (pan-tissue) | Horvath 2013, Genome Biology | original 353-CpG table | trained on dozens of GSE sets incl. blood |
| Hannum2013 | Hannum 2013, Mol Cell | original 71-CpG table | **trained on GSE40279** |
| PhenoAge | Levine 2018, Aging | 513-CpG table; biolearn#220 flags coefficient drift in re-implementations | GSE42861-adjacent cohorts |
| GrimAge / GrimAge2 | Lu 2019 / Lu 2022 (aging-us) | surrogate plasma-protein estimators + meta weights | large training corpora incl. EPIC-era data — verify per dataset |
| DunedinPACE | Belsky 2022, eLife | 193-CpG pace model | trained on Dunedin + replications |
| PC-clock (PCDNAmAge) | Higgins-Chen 2022, Nat Commun | principal-component reweighting | reduced leakage sensitivity by construction — include as control |
| CausAge / DamAge / AdaptAge | Ying 2024, Nat Aging | causality-enriched | newer corpora — verify |

Roster changes require a manifest bump + changelog entry.

## 2. Implementation matrix (parity lane)

For each clock, every available implementation runs on the same input matrix:
- `pyaging` (Python)
- `methylclock` (R/Bioconductor, v2 registry)
- `biolearn` (Python)
- original author code (R, where public)

**Parity divergence score** per clock:
`D = max_i,j |age_i - age_j|` over implementations i,j on sample n, reported
as median + p95 + max across samples, plus the per-implementation
preprocessing contract actually applied (normalization family, imputation
policy, SNP/probe filtering, cell-composition handling).

Hypothesis registered before first run: cross-implementation divergence for
the same clock is comparable in magnitude to cross-clock differences on the
same samples. If true, cross-study comparisons using "GrimAge" without an
implementation contract are not measuring what they claim.

## 3. Data (see datasets/manifest-v0.1.yaml)

- Seed: GSE40279 (450k blood), GSE87571 (EPIC blood), GSE112618 (EPIC blood).
- Expansion via biolearn's `library.yaml` + community nominations.
- **Leakage ledger**: every clock x dataset pair carries
  `overlap: none | partial | training` with evidence links. `training` pairs
  are excluded from the primary scoreboard and shown in a separate
  in-sample column (for honesty, not ranking).
- Inputs are frozen beta matrices (noob-normalized) distributed as hashes;
  per-lane preprocessing perturbations are applied on top by the harness, not
  by contributors.

## 4. Metrics (primary + reported-always)

Primary: MAE (yrs), Pearson r, Spearman rho — computed out-of-sample only.
Always reported: calibration (OLS slope/intercept of chronological ~ predicted
+ binned ECE), subgroup slices (sex, age deciles, platform), parity divergence
score (lane B), robustness deltas (lane C: +/- perturbation of normalization,
imputation, and 5% masked-CpG stress).
No single composite score. The scoreboard shows the matrix; readers weight it.

## 5. Lanes C-E (v0.1 scope notes)

- **Robustness (C)**: normalization family sweep (dasen/noob/functional),
  imputation policy sweep (zero-fill/mean/reference/KNN — the pyaging#51
  class), masked-CpG stress.
- **Calibration & subgroups (D)**: per-lane reporting, no retraining.
- **Portability (E, v0.2)**: 450k->EPIC restricted probes; mouse/mammalian
  array clocks; cross-tissue (Apsley 2025 cross-tissue protocol as reference).

## 6. Versioning

`spec vX.Y` + `manifest vX.Y` + `results vX.Y` are versioned independently;
a scoreboard row always cites all three. Spec changes: PR + 7-day comment
window. Manifest changes: PR + changelog. Results: regenerated on schedule by
CI; historical artifacts never overwritten.
