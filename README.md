# neurocascade: model card, validation ledger and frozen benchmark

neurocascade is a simulation engine for CNS drug discovery. Given a compound's
preclinical package (receptor binding panel, in-vitro ADME, rodent PK and brain
exposure), it predicts the human consequences: PK, the dose that engages the
target, receptor occupancy over time, and eventually the expected effect and
liability profile, each with a measured uncertainty band.

It chains pharmacokinetics → brain partition → receptor binding → circuit →
behavioural readout as coupled ODEs. Its value is judged by retrospective
benchmarking: known CNS drugs are given only their preclinical data, and the
predictions are scored against what was later measured in humans.

This repository publishes the evidence trail. The engine source is private for
now and available to reviewers on request.

## Contents

| Path | What it is |
|---|---|
| [`docs/model-card.md`](docs/model-card.md) | What the model predicts today, its scope, and what it must not be used for |
| [`docs/validation-ledger.md`](docs/validation-ledger.md) | Every validation run, passes and failures alike, with sources, split design and caveats |
| [`docs/roadmap-product.md`](docs/roadmap-product.md) | The Phase 28–32 product roadmap |
| [`docs/prediction-api.md`](docs/prediction-api.md) | The prediction interface |
| [`benchmark/drugs.json`](benchmark/drugs.json) | The 185-drug CNS benchmark (antipsychotics, anxiolytics, antidepressants, clinical failures) |
| [`benchmark/split.json`](benchmark/split.json) | The frozen train/held-out split (130/55) |

## Current state, in brief

- Validated end to end: oral dose → striatal D2 occupancy for antipsychotics,
  against human PET data held out from fitting (haloperidol MAE 0.035
  occupancy units). Validation is narrow: one receptor, one region, small n.
- Recorded failures: clozapine (pre-registered falsification of
  occupancy-as-sole-mediator), ziprasidone, depot formulations.
- Not yet built: the effect/liability model and the held-out benchmark run
  (roadmap Phases 30–32).

## Benchmark integrity

The split was drawn on 2026-10-01, before any effect model existed, and is
never redrawn. Its SHA-256 is pinned in the engine's test suite and in the
ledger:

```
54a08acfe76b21237bd3152653184796c875f064c66408c9449980f9df6628d9  benchmark/split.json
```

## License

Documentation: CC BY 4.0. Benchmark data is derived in part from ChEMBL
(release 37, CC BY-SA 3.0) and is shared under the same terms.

Eniola Olutogun · ORCID [0009-0001-9272-6735](https://orcid.org/0009-0001-9272-6735)
