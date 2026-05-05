---
name: tdd-cycle
description: Use when implementing new behavior or fixing a bug; when production code appears before an observed failing test or tests pass on first run without RED proof; when the user asks for red-green-refactor, TDD, or tracer-bullet vertical slices one test at a time; when tying mandatory regression to a slice in this repository's solo workflow.
---

# TDD cycle

## Overview

Enforce **strict RED → GREEN → REFACTOR** on **vertical slices** (one behavior at a time through public seams). Solo orchestration, verification commands, and Git HITL are indexed at [workflow-conventions.md § Cross-skill alignment](../../reference/workflow-conventions.md#cross-skill-alignment).

**Announce at the start:** `Using the tdd-cycle skill for this implementation.`

## When to use

- New behavior, bugfix, regression guard, or behavioral refactor.
- Any situation where “tests pass” is being claimed without an observed failing test first.

## When not to use

- User explicitly opts out for a **throwaway spike** (must be discarded or isolated before shipping).
- **Generated-only** or pure config with no behavioral contract — only if the track truly has no test surface (rare; confirm once).

If you hear **“skip just this once”**, treat it as rationalization — see table below.

## Iron law

```
NO PRODUCTION CODE WITHOUT A TEST THAT FAILED CORRECTLY FIRST

Wrote code first? Revert to a point before the intended failure. Do not keep unproven code as "reference".
```

## Core pattern — vertical slice (anti-horizontal)

| Wrong (horizontal) | Right (vertical) |
|--------------------|------------------|
| RED: many tests, then GREEN: big implementation | RED → GREEN per behavior, repeat |

**Before first RED:** align public interface, domain vocabulary ([workflow-conventions.md](../../reference/workflow-conventions.md)), and priority behaviors with the human. Style: [tdd-public-api-tests.md](../../reference/tdd-public-api-tests.md). Mocks: only when unavoidable — [REFERENCE-testing-anti-patterns.md](REFERENCE-testing-anti-patterns.md).

## Micro-cycle (repeat per behavior)

| Phase | Do |
|-------|-----|
| **RED** | One test, one behavior, clear name; prefer **integration through public API**. |
| **Verify RED** | Run the **full** project test command **this session**. Failure must match the expected reason (not a typo/compile noise). If it already passes, the test is useless — fix the test. |
| **GREEN** | **Minimal** code to satisfy the assertion — no drive-by features. |
| **Verify GREEN** | Same discipline: full relevant run, fresh evidence. |
| **REFACTOR** | Only on green; naming and structure — **no behavior change**. Never refactor on red. |

## Quick checklist

- [ ] Test describes **observable behavior**, not internals.
- [ ] Uses **public contracts** when the codebase has a clear boundary.
- [ ] Saw expected failure before new production code.
- [ ] Implementation only what the current test demands.
- [ ] Suites re-run until stable.

## Tests as specification

**Good:** behavior through stable seams — survives internal refactors. **Bad:** brittle mocks on internals. Red flag: refactor **without** behavior change breaks tests.

## Rationalizations — single counter

| Excuse | Counter |
|--------|---------|
| Too simple to test | Simple code breaks too. |
| “Tests after are the same” | “After” describes what is; RED proves what **should** be with a valid failure. |
| “I verified manually” | Not repeatable under pressure. |
| “Deleting work wastes time” | Sunk cost; unproven code is debt. |
| Explore first | Exploration must be thrown away or quarantined until TDD resumes for shipped behavior. |
| Hard to mock everything | Likely over-coupling — fix design, not the rule. |

**Red flags — stop:** production before failing test • tests only at end • passes first run without RED review • “spirit not letter.”

## Bugs

Reproduce with an **automated test first** when feasible → RED → minimal fix. “Fixed” requires **fresh evidence** per Cross-skill alignment.

## Further reading

- [REFERENCE-testing-anti-patterns.md](REFERENCE-testing-anti-patterns.md)
- [tdd-public-api-tests.md](../../reference/tdd-public-api-tests.md)
