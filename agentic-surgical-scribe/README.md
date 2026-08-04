# CODVO.AI Take-Home: Agentic Surgical Scribe

**Role:** Agentic AI Architect &nbsp;·&nbsp; **Effort:** ~3 days &nbsp;·&nbsp; **Presentation:** 1 hour

Build a system that turns raw operating-room speech into a structured, evidence-linked
operative record, a draft note a surgeon would sign, and a coding proposal that can be
defended to a payer.

> **Read the full brief first:** open [`TASK.html`](TASK.html) in a browser.
> It covers the scenario, the output contract, the architecture choice, the mandatory
> capabilities, the live test, and how we score. This README is the quick map.

---

## Repository layout

```
.
├── TASK.html                          ← full assignment brief. START HERE
├── README.md                          ← this file
├── data/
│   ├── transcripts/*.jsonl            ← 20 cases, one utterance per line
│   ├── gold/*.json                    ← structured records for 12 of the 20
│   ├── gold/*.md                      ← the signed operative note for those 12
│   ├── reference/cpt_codes.csv        ← allowed CPT universe + descriptors
│   ├── reference/icd10_codes.csv      ← allowed ICD-10-CM universe + descriptors
│   └── sample_case/                   ← one case end to end, with expected output
├── docs/
│   ├── data_dictionary.md             ← every field defined, read before prompting
│   └── documentation_standard.md      ← the institution's note rules
├── schemas/
│   └── op_record.schema.json          ← the output contract, validated at scoring
└── submission/                        ← put extra deliverables here
```

## The data in 30 seconds

- **20 transcripts.** Ambient operating-room audio, already through speech-to-text. One
  JSON object per utterance: `utt_id`, timestamps, `speaker`, a `speaker_role_hint`, an
  ASR confidence, and the text. 312 to 541 utterances per case, 44 to 76 minutes.
  Flattened to id, role and text that is roughly 5k to 7k tokens; the raw JSONL is
  17k to 26k. Either way a whole case fits in one context window, which is
  deliberate.
- **12 gold records** written by a clinical reviewer, plus the signed note for each. This
  is your labeled set and your style corpus. The other 8 cases have no gold.
- **The case mix is not uniform** and we are not saying what is in it. Most are routine.
  Some are not.
- **Nothing is pre-labeled beyond those 12.** Deciding what "good" means, and building the
  evaluation that measures it, is a substantial part of the task.

Full definitions: [`docs/data_dictionary.md`](docs/data_dictionary.md) and
[`docs/documentation_standard.md`](docs/documentation_standard.md).

## What to submit

Extend this repository with:

1. A **reproducible pipeline** that runs from one documented command.
2. Your **evaluation harness** and the numbers it produced on the 12 gold cases.
3. A working **`--mock` mode** that runs the whole pipeline offline against fixtures.
4. An updated **README** with your write-up: architecture, the options you rejected and
   why, eval results, known failure modes, cost and latency per note, and what you would
   do with a month.

Pin dependencies. Keep keys in environment variables, never in the repo. Make sure a
fresh clone runs.

## Output contract

One file per case, `out/<case_id>.json`, validating against
[`schemas/op_record.schema.json`](schemas/op_record.schema.json). Every field carries the
transcript utterances that support it and a confidence. `null` is a legitimate value and
means "the transcript does not say."

```
python -m scribe.run --in data/transcripts/ --out out/ [--mock]
```

Two rules worth repeating from the brief:

- **A confident wrong value is worse than an honest null.** Abstention is scored as
  correct when the transcript genuinely does not contain the fact.
- **Codes come from a lookup against `data/reference/`, not from model memory.**

## Live test

In the final 20 minutes of the presentation we hand you four unseen cases from the same
service and run your command. We score schema validity, whether the cited evidence
resolves, field accuracy, abstention, hallucination rate, coding, the review queue, and
whether your run manifest actually reports cost and latency.

Rehearse the exact command on a clean clone the day before. Have `--mock` tested: if your
provider rate-limits you live, you switch and keep going.

## Timeline

| When | What |
|---|---|
| Day 0 | You receive this repo |
| ~3 days | Design and build |
| Presentation day | Final commit + 1-hour presentation (40 + 20) |

Questions before then? Reply to your assignment email.
