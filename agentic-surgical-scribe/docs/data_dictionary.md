# Data dictionary

Read this before you write a prompt. The semantics of `null` in particular are
not conventional, and getting them wrong will cost you more than any modeling
decision.

---

## `data/transcripts/*.jsonl`

Twenty cases. One JSON object per line, in time order. Each line is one utterance
as the speech-to-text service emitted it.

| Field | Type | Meaning |
|---|---|---|
| `case_id` | string | Matches the filename. Repeated on every line. |
| `utt_id` | string | `u0001`, `u0002`, ... Unique within a case, monotonic with time. **This is the unit of evidence.** |
| `t_start_s` | float | Seconds from the start of recording, which is roughly when the patient entered the room. |
| `t_end_s` | float | End of the utterance. |
| `speaker` | string | `SPK_1` ... `SPK_5`. The diarizer's speaker cluster. Clustering is reliable: the same person is consistently the same `SPK_n` within a case. |
| `speaker_role_hint` | string | The diarizer's **guess** at the role: `surgeon`, `assistant`, `scrub_tech`, `circulator`, `anesthesia`, or `unknown`. It is wrong roughly one time in ten. |
| `asr_confidence` | float | 0 to 1. Low values cluster on anatomical terms, eponyms and drug names, which is where recognition actually fails. |
| `text` | string | The recognized text. Not corrected. Not punctuated for you. |

Cases run 44 to 76 minutes and 312 to 541 utterances. Flattened to id, role and
text a whole case is roughly 5k to 7k tokens; the raw JSONL as shipped is 17k to
26k. Either way it fits in one context window, which is deliberate: see §6 of
`TASK.html`.

### Two things about `speaker` and `speaker_role_hint`

They are different fields and they fail differently. `speaker` is a clustering
result and is dependable. `speaker_role_hint` is a classification on top of it
and is not. There are also more people in the room than there are roles in the
list.

---

## `data/gold/*.json`

The clinical reviewer's record for 12 of the 20 cases. This is your labeled set.

It carries `case_id`, `fields` (each with `value` and `evidence`), and `coding`.
It deliberately does **not** carry `confidence`, `review_queue`, `note_markdown`
or `run`, because those are properties of a system rather than properties of the
truth. Your output carries them; the reviewer's record does not.

Field semantics, which are the whole assignment:

| Field | Type | Notes |
|---|---|---|
| `procedure_performed` | string | The procedure **as completed**, not as booked. |
| `converted_to_open` | bool | Contemplating conversion is not converting. |
| `cholangiogram_performed` | bool | Performed, not discussed, not declined, not "would be indicated if". |
| `critical_view_of_safety` | `achieved_and_stated` / `not_achieved` / `null` | See below. |
| `estimated_blood_loss_ml` | int | Milliliters. If revised during the case, the record takes the surgeon's final statement. |
| `drain_placed` | bool | |
| `specimen_disposition` | string | In the transcript's own terms. |
| `complications` | array | `[]` and `null` mean different things. See below. |
| `counts_correct` | bool | A count that was wrong and then reconciled is a correct count. |

### `null` means "the transcript does not say"

It is a legitimate value, it is scored as **correct** when the transcript is
genuinely silent, and it is not an error state. One field slot in five across
the corpus is null. A system that never emits one is not being careful, it is
guessing.

### `[]` and `null` are not the same, for `complications`

- `[]` means complications were addressed and there were none. The surgeon said so.
- `null` means nobody addressed complications at any point in the case.

These are different clinical claims and they are scored separately.

### `critical_view_of_safety` has no "not documented" value

- `achieved_and_stated`: the surgeon explicitly stated the critical view was obtained.
- `not_achieved`: the surgeon explicitly stated it could not be obtained.
- `null`: the transcript never addresses it.

There is no fourth value. A surgeon who obtains the critical view and never says
so produces a `null`, and the resulting case reads as undocumented in the audit
even though the operation was safe. That gap is the reason this project exists.

---

## `data/gold/*.md`

The signed operative note for those same 12 cases, in the institution's
template. This is your style corpus and your narrative reference. Note how the
notes render an absent fact: they say "Not documented", they do not omit the
line and they do not fill it in.

---

## `data/reference/cpt_codes.csv`, `data/reference/icd10_codes.csv`

| Column | Meaning |
|---|---|
| `code` | The code. |
| `descriptor` | The official descriptor. |

**These tables are the entire valid code universe.** A code outside them is an
invalid submission even if it is a real code in the wider code set. Both tables
contain codes that do not apply to any case here; discriminating is the job.

---

## `data/sample_case/`

One case end to end: `CASE_0001.jsonl`, `CASE_0001.gold.json`,
`CASE_0001.note.md`. It is also present in `transcripts/` and `gold/`. Build
against this one first.

---

## `schemas/op_record.schema.json`

The output contract, as JSON Schema. Your submission is validated against it
before it is scored, so validate locally first.

---

## What is not here

- **No audio.** Speech-to-text is a commodity and it is not what we are hiring for.
- **No phase labels.** If your architecture wants surgical phase, derive it.
- **No labels for the 8 unlabeled cases.** Deciding what to do with unlabeled
  data is part of the assignment.
- **No metric.** See §8 of `TASK.html`.
