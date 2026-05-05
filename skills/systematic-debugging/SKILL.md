---
name: systematic-debugging
description: Use when there is a bug, test failure, unexpected behavior, CI/integration failure, flaky tests, performance regression, or several failed attempts in a row; when the user asks to diagnose/debug or wants root cause before a patch; before proposing fixes or instrumentation without a sustained reproduction.
---

# Systematic debugging

## Overview

**Hypothesis → evidence → minimal fix** with a **trusted feedback loop first**. Consolidates root-cause discipline and fast falsification. Domain context: glossary/ADRs in [workflow-conventions.md](../../reference/workflow-conventions.md). Orchestration + verification: [§ Cross-skill alignment](../../reference/workflow-conventions.md#cross-skill-alignment).

**Announce at the start:** `Using the systematic-debugging skill for this diagnosis.`

**Principle:** shipping a fix without a falsified root cause is failure. Breaking the letter breaks the spirit.

## When to use

- Failing tests, wrong behavior, CI, integration, production incidents.
- **Pressure** — emergencies increase guesswork; apply harder.
- **Thrashing** — symptom shifts each attempt → reset to loop + evidence.

## When not to use

- **Greenfield feature** with no bug signal — use `tdd-cycle` / planning instead.
- User only asked for a **spec or design** — route to `brainstorming` unless they also report a failure.

## Iron law

```
NO FIX WITHOUT ROOT-CAUSE WORK THAT INCLUDES A FEEDBACK LOOP TO FALSIFY IT

If Phase A (loop) is weak or missing, you do not understand the problem well enough to “just patch.”
```

## Core — the loop is the skill

Without a repeatable pass/fail the agent (or CI) can run, bisection and ad hoc logging do not substitute. Invest in **sharpening the loop** — [REFERENCE-FEEDBACK-LOOP.md](REFERENCE-FEEDBACK-LOOP.md). HITL-only loops: [scripts/hitl-loop.template.sh](scripts/hitl-loop.template.sh).

## Flow (mandatory order)

### A — Build the loop

Satisfy the reference. **Do not enter B** without a loop you trust.

### B — Reproduce

- [ ] Matches **user** symptom (not a neighboring failure).
- [ ] Stable enough, or flaky rate workable.
- [ ] Exact symptom recorded (message, bad output, metric).

### C — Investigate before fix

1. Read errors/stacks completely.
2. **Recent changes** — diffs, deps, config, env.
3. **Multi-component** — evidence at each boundary before blaming one layer.
4. **Data flow / deep bugs** — [root-cause-tracing.md](root-cause-tracing.md); fix at source.
5. **Pattern compare** — similar working code; list real differences.

### D — Hypotheses

3–5 **ranked**, each **falsifiable**:

> If **X** is the cause, then **Y** clears it / **Z** worsens it.

Show ranked list to user before expensive probes unless domain makes order obvious.

### E — Instrument

1. Debugger/REPL when available.
2. **Targeted** logs at hypothesis boundaries — not “log everything.”
3. Prefix temp logs: `[DEBUG-<id>]`. Remove with `grep` before close.
4. **Performance:** measure and bisect before guessing — profiler/timing harness.

One variable per probe; tie each to a Phase D prediction.

### F — Minimal fix + regression

1. **Regression test** before fix when seam is real (not shallow).
2. One corrective change at a time.
3. Verify: symptom gone; harness green.

If **≥3** fix attempts fail → **stop** — likely structural; align with user before a 4th.

### G — Cleanup

- [ ] Original loop no longer reproduces.
- [ ] Regression passes or seam gap documented.
- [ ] `[DEBUG-…]` gone.
- [ ] Throwaway artifacts isolated or deleted.
- [ ] Notes: which hypothesis held.

**After:** if coupling or missing seam caused pain, point to **`improve-codebase-architecture`** with facts.

## Red flags — rewind

- “Quick fix now, investigate later.”
- Many edits before isolation.
- Fix proposals without flow trace.
- “One more try” with no new evidence.

User signals (“stop guessing”, “show us…”) → back to **C**.

## Rationalizations

| Excuse | Reality |
|--------|---------|
| Too simple for process | Simple bugs still have causes. |
| No time | Thrashing costs more. |
| Test after manual confirm | No durable proof. |
| Several changes at once | Cannot attribute what fixed it. |

## Supporting files

| File | Purpose |
|------|---------|
| [REFERENCE-FEEDBACK-LOOP.md](REFERENCE-FEEDBACK-LOOP.md) | Build/sharpen loop |
| [root-cause-tracing.md](root-cause-tracing.md) | Trace origin |
| [defense-in-depth.md](defense-in-depth.md) | Layered checks **after** cause known |
| [condition-based-waiting.md](condition-based-waiting.md) | Flakes from sleeps |
| [condition-based-waiting-example.ts](condition-based-waiting-example.ts) | Example |

## Related skills

- **`tdd-cycle`** — RED proof for regressions.
- **`improve-codebase-architecture`** — structural follow-ups.

## When “no root cause”

Only after exhaustive work: document what **was** proven; mitigate and monitor. Most “unknown root cause” is incomplete investigation.
