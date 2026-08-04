# CODVO.AI — Take-Home Assignments

This repository holds the take-home assignments CODVO.AI uses for senior
engineering and architecture hires. Each task is self-contained in its own
folder.

If you were sent a link to one of these, go to that folder and start with its
`TASK.html`.

---

## Assignments

### [`predictive-maintenance/`](predictive-maintenance/)

**Predictive Equipment Failure — Industrial Pump Fleet**
Role: Data Science Architect · Effort: ~3 days · Presentation: 1 hour

Build an end-to-end, reproducible ML solution that forecasts when industrial
pumps are heading toward failure, from multivariate time-series sensor data.
Run-to-failure history for 80 pumps, no pre-labeled target, censored units in
the fleet. Choosing the framing is part of the task. Scored partly on a live
inference run against unseen pumps.

Start here: [`predictive-maintenance/TASK.html`](predictive-maintenance/TASK.html)

### [`agentic-surgical-scribe/`](agentic-surgical-scribe/)

**Agentic Surgical Scribe**
Role: Agentic AI Architect · Effort: ~3 days · Presentation: 1 hour

Turn raw operating-room speech into a structured, evidence-linked operative
record, a draft note a surgeon would sign, and a coding proposal that can be
defended to a payer. Multi-agent orchestration, grounding, guardrails and
evaluation of a generative system. Scored partly on a live run against unseen
cases.

Start here: [`agentic-surgical-scribe/TASK.html`](agentic-surgical-scribe/TASK.html)

---

## Reading the brief

`TASK.html` is a single self-contained HTML file. GitHub shows HTML as source
rather than rendering it, so clone the repository and open the file in a
browser:

```
git clone https://github.com/balasista/take-home-tasks.git
cd take-home-tasks
open predictive-maintenance/TASK.html      # macOS
xdg-open predictive-maintenance/TASK.html  # Linux
start predictive-maintenance\TASK.html     # Windows
```

Each task's own `README.md` is the quick map: data, deliverables, the output
contract, and the timeline.

## Ground rules, common to every assignment

- AI coding assistants are allowed and expected. You must understand and be able
  to defend every line, because we probe deeply.
- Document your assumptions rather than emailing questions. Stating a reasonable
  assumption clearly is itself a signal we value. If something genuinely blocks
  you, do reach out.
- Commit incrementally. We read the history.
- Do not over-build. A focused, well-reasoned solution beats a platform.

Questions about a specific assignment: reply to the email it came with.
