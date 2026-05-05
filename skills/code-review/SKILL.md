---
name: code-review
description: Use when requesting review before merge or between slices; when acting as reviewer (mid-slice or final pass, or someone else's PR); when the diff is large or ambiguous vs plan or interface-design; when receiving review feedback and you must verify before changing code or replying without empty agreement.
---

# Code review

## Overview

Structured **diff + plan** review for the solo workflow: two gates (spec alignment, then quality/evidence), binary rubric, clear severity. Orchestration and delivery norms: [workflow-conventions.md § Cross-skill alignment](../../reference/workflow-conventions.md#cross-skill-alignment).

**When acting as reviewer, announce:** `Using the code-review skill for structured review.`

**Principles:** minimum context that still decides (diff + slice interface-design + plan); on feedback — **verify in repo** before implementing.

## When to use

- After a meaningful slice, before merge, or when blocked and need a fresh pass.
- **Mid-slice** and **final** passes per table below.
- **Someone else’s PR:** treat as final pass; align with their `validation-*` / PR scope.

## When not to use

- **No diff or no plan/spec anchor** — gather artifacts first.
- **Chat-only “LGTM”** without rubric — use the checklist or decline the theater.

## Passes and scope

| Pass | Scope | When |
|------|--------|------|
| **Mid-slice** | Slice diff + that slice’s **interface-design** | Before starting the next slice |
| **Final** | Narrow diff (Simple/Hotfix) or branch surface (Normal/Complex) | Before `task-delivery` / external handoff |

Solo mode: two **discipline moments**, not two people.

## Binary rubric (excerpt)

Mark each **yes / no / N/A justified** in narrative or `validation-*` (final).

- Matches **interface-design** from `plan-*` / slice (no surprise contracts).
- **Vertical** slices (not layer-only drops).
- No function/method **>20 lines** without justification in plan or code.
- No hardcoded secrets.
- Single responsibility inferable from names.
- **Tests:** baseline command per track; regressions covered or justified; behavior changes have proof paths.
- No new `TODO` without ticket.
- No obvious stack anti-patterns (N+1, mixed layers, bad error/transaction patterns).
- Public contracts not broken without versioning story.

Full track DoD may extend this — see orchestrator and `plan-*`.

## How to request review (developer / orchestrator)

1. Resolve base/head (or slice SHAs).
2. Run reviewer pass with [reviewer-prompt.md](reviewer-prompt.md) — fill placeholders, attach diff or range.
3. Triage: **Critical** now; **Important** before continue; **Minor** log or backlog; **push back** with evidence if wrong.

## How to receive feedback

```
READ → UNDERSTAND (rephrase or ask) → VERIFY in code → ASSESS for THIS repo → REPLY or IMPLEMENT (one item at a time, each verified)
```

**Avoid:** performative agreement, implementing before disambiguation, empty thanks — replace with technical summary and code.

**External / PR feedback:** suggestion until verified (`grep` usage before “proper” refactors on dead paths). Conflict with product owner → stop and align.

**GitHub:** reply in-thread where applicable.

Fix order: blockers → simple → complex; verify each fix.

## Common mistakes

| Mistake | Fix |
|---------|-----|
| Review without plan/interface-design | Attach slice contract |
| Skip mid-slice | Review slice diff before next slice |
| Blind implementation | Verify each finding |
| Nits as Critical | Honest severity |

## Further reading

- [reviewer-prompt.md](reviewer-prompt.md)
