# Contributing to ClockBench

We keep it simple, we verify everything, and we credit everyone — same house
rules as the rest of Limitless Projects.

## The three ways in

1. **Add an implementation** to the parity lane: open an Issue with
   `parity candidate` + package/version + the clock's coefficient provenance.
   A maintainer reproduces one sample end-to-end before merge.
2. **Nominate a dataset**: accession + platform + n + tissue + age metadata +
   *any* training-overlap suspicions. Nominations go into the next manifest PR.
3. **Attack the spec**: every version has a 7-day comment window. The spec
   that survives review becomes v1.0 — reviewers named in the changelog.

## Neutrality rules

- You may contribute your own clock's implementation, but you may not score it.
- Upstream bugs found by the parity lane get filed upstream with minimal
  reproducers BEFORE they appear on the public scoreboard.
- Negative results are first-class results. Fail-tests (cf. aging-us 2023)
  are the point.

## House style

Small PRs, receipts in every claim, no silent closes — every Issue and PR
gets a written reply. See the repo that started the house:
[Go-Hybrid/AUTOMATION.md](https://github.com/limitlessprojects/Go-Hybrid/blob/main/AUTOMATION.md).
