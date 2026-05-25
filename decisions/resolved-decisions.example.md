# Resolved Decisions

Deliberate divergences from the best-practices guide. The auditor reads this file and
treats everything here as **intentional** — it will not flag these as gaps. If new
guidance conflicts with a decision below, the auditor flags the conflict for your review
rather than overriding it.

Copy this file to the path you set as `DECISIONS_PATH` (e.g. `docs/resolved-decisions.md`)
and replace the examples with your own.

## Format

Each entry: the decision, why it diverges from the guide, and the date you settled it.

---

### 1. (example) Interactive sessions default to the strongest model

- **Diverges from:** guide §4 / §7 — "match model to task; don't default everything to
  the most expensive model."
- **Why it's deliberate:** interactive work here is reasoning-heavy and runs on a flat
  subscription, not metered per-token, so the stronger model is the right trade for this
  context. Headless/automated paths still route to cheaper models.
- **Settled:** 2026-01-01

### 2. (example) A specific skill has `disable-model-invocation: true`

- **Diverges from:** guide §3 — skills should be model-invocable.
- **Why it's deliberate:** this skill is destructive and must only run when a human types
  the slash command; it is never needed on the headless path.
- **Settled:** 2026-01-01

---

Delete the examples above and add your own.
