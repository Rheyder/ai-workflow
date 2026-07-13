# Definition of Done

A change is **ready** when:

- [ ] **Objective is clear** — problem, scope, and acceptance criteria are understood.
- [ ] **Scope matches the request** — implemented work maps to what was asked; out-of-scope work is called out.
- [ ] **Relevant verification ran** — with fresh evidence in this session ([CONVENTIONS.md](CONVENTIONS.md#evidence-before-claims)).
- [ ] **Available evidence is recorded** — or success is stated with evidence.
- [ ] **Code was reviewed** — proportional to risk (`code-review` or fast-path risk review).
- [ ] **Accidental complexity reduced when practical** — after behavior is protected (`code-simplification` when warranted).
- [ ] **Known risks are communicated** — security, data, breaking changes, operational impact.
- [ ] **Next step is explicit** — merge, PR, handoff, patch, follow-up task, or re-run verification.

Delivery may be merge, publication, handoff, patch, or human review — no specific tool or issue tracker is required.

## By workflow phase

| Phase | Done means |
|-------|------------|
| Spec | Short spec + acceptance criteria + known risks |
| Plan | Slices/tasks + order + how each slice will be verified |
| Branching | Clean baseline + working branch recorded |
| Build | Code + tests for new/changed behavior |
| Verify (slice) | [slice-verification](slice-verification/verification-report.md) report with evidence |
| Verify (branch) | [finishing Step 1](finishing-a-development-branch/verification-report.md) before integration |
| Review | Findings by severity; blockers resolved or accepted |
| Simplify | Tests still pass; behavior unchanged |
| Ship | Integration path chosen; user HITL on remotes when applicable |

## Fast path

For eligible small changes (see [WORKFLOW.md § Fast path](WORKFLOW.md#fast-path)), a lighter checklist is acceptable: understand → implement → verify → risk review → summary — but **evidence before claims** still applies.
