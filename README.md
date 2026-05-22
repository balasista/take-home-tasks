# CODVO.AI — Take-Home: Predictive Equipment Failure

**Role:** Data Science Architect &nbsp;·&nbsp; **Effort:** ~3 days &nbsp;·&nbsp; **Presentation:** 1 hour

Build an end-to-end, reproducible ML solution that forecasts when industrial
pumps are heading toward failure, from multivariate time-series sensor data.

> **Read the full brief first:** open [`TASK.html`](TASK.html) in a browser.
> It covers the scenario, framing options, deliverables, the live-inference
> contract, evaluation criteria, and timeline. This README is the quick map.

---

## Repository layout

```
.
├── TASK.html                     ← full assignment brief — START HERE
├── README.md                     ← this file
├── data/
│   ├── train.csv                 ← run-to-failure history, 80 pumps (~16.6k rows)
│   ├── train_units.csv           ← per-unit metadata + the `event` flag
│   └── sample_test_input.csv     ← example of the live-inference input schema
├── docs/
│   └── data_dictionary.md        ← every column defined — read before modelling
└── submission/                   ← put your deliverables here
```

## The data in 30 seconds

- **`train.csv`** — one row per operating cycle (~1 hour) for 80 pumps, long
  format, 20 sensor/operating columns.
- **`train_units.csv`** — `event = 1` → the pump ran to failure (last cycle =
  failure point); `event = 0` → censored (still healthy when monitoring
  stopped). **72 failed, 8 censored.**
- There is **no pre-labelled target.** Choosing the framing — RUL regression,
  failure-within-window classification, survival, or your own — is part of the
  task. See §5 of `TASK.html`.
- The data is real-world historian data. Not every channel is informative.
  Assessing channel quality is part of the job.

Full definitions: [`docs/data_dictionary.md`](docs/data_dictionary.md).

## What to submit

Extend this repository with:

1. A **reproducible pipeline** — data prep → features → training → evaluation.
2. The **trained model artifact**.
3. An **inference interface** (see contract below).
4. **Honest evaluation** with unit-level splits (no leakage across a pump's
   own timeline).
5. An updated **README** with your design write-up: framing, architecture,
   key decisions, assumptions, and next steps.

Pin your dependencies, seed randomness, and make sure a fresh clone runs.

## Live-inference contract

In the last 15 minutes of the presentation we run your inference code on unseen
pumps (same schema as `sample_test_input.csv`). Whatever framing you chose, your
inference must emit a **predicted Remaining Useful Life** per unit:

```
unit_id,predicted_rul
PUMP_T01,57
PUMP_T02,33
...
```

Extra columns (failure probability, confidence interval, failure-mode guess)
are welcome. It must run **offline, on CPU, in a few minutes**. Rehearse the
exact command before the call — if it does not run, we cannot score it.

## Timeline

| When | What |
|---|---|
| Day 0 | You receive this repo |
| ~3 days | Design and build |
| Presentation day | Final commit + 1-hour presentation |

Questions before then? Reply to your assignment email.
