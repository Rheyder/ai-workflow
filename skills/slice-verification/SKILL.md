---
name: slice-verification
description: Verify the current slice with evidence after TDD GREEN. Use before commit announcement. Not for whole branch or diff review.
---

# Slice Verification

**Verify phase (slice)** — confirm the **current vertical slice** works end-to-end with evidence. Not for:

- Whole-branch AC → `finishing-a-development-branch` Step 1
- Diff quality review → `code-review`
- Writing the next test → `tdd`

## Overview

Confirm **this slice** works in practice — not the whole feature branch. Run focused checks, capture evidence, then report.

**Evidence:** Follow [CONVENTIONS.md](../../CONVENTIONS.md#evidence-before-claims) (slice scope). Report per [verification-report.md](verification-report.md).

## When to use

- After TDD GREEN, before commit announcement (HITL).
- User asks to smoke-test the current slice.

## When not to use

- Whole branch AC → `finishing-a-development-branch` Step 1
- Diff review → `code-review`

## Workflow position

```
git-workflow-and-versioning → tdd → slice-verification (light) → [HITL commit per slice]
  → … all slices … → code-review → finishing-a-development-branch (full verify + git)
```

## Boundaries

| Skill | Role |
|-------|------|
| `finishing-a-development-branch` | Full branch verification + push/PR/merge menu |
| `code-review` | After all slices committed; reviews diff |
| `diagnose` | Slice validation fails, cause unclear |
| `tdd` | Slice validation fails, cause clear |
| `frontend-testing` | Optional UI check for this slice when frontend-facing |

## Expected input

- Current slice scope (issue, conversation, or last GREEN test).

## Expected output

- Report per [verification-report.md](verification-report.md) with command evidence.

## Process

### 1. Understand this slice's behavior

From the current slice scope (issue slice, conversation, or last GREEN test): identify what behavior was added or changed.

**Stop if** slice scope is unclear — ask the user.

### 2. Run automated checks (slice scope)

Detect commands from the repo. Run tests/build/lint **relevant to touched code** — not necessarily the full suite unless the slice clearly requires it.

**Stop on failure** — route per Failure routing.

### 3. Direct behavior check (minimal)

Exercise **only** what this slice changed:

- **HTTP** — affected route(s) if applicable
- **CLI** — command path touched by slice
- **UI** — invoke `frontend-testing` only when this slice is UI-facing

Skip runtime steps when the slice is fully covered by automated tests with no runtime surface — document skip in report.

### 4. Inspect evidence

If runtime was exercised: check logs for errors during the slice scenario. No secrets in logs.

### 5. Produce report

Output using [verification-report.md](verification-report.md). Recommend: safe to announce slice commit or fix first.

## Detect project commands

Read project scripts and CI config before inventing commands. Examples by stack (use **only** what the repo defines):

- Node — test/build/lint scripts in `package.json`
- JVM — Maven wrapper or Gradle wrapper targets
- Go — `go test`, `go build`
- Python — `pytest`, Makefile, or `pyproject.toml` scripts

Prefer documented project commands over guesses.

## Failure routing

```
Slice validation failed
  ├─ Cause unclear → invoke diagnose
  ├─ Cause clear → return to tdd (RED)
  └─ UI-only gap → frontend-testing if available, then re-verify
```

## Handoffs

| Outcome | Next step |
|---------|-----------|
| Pass | `git-workflow-and-versioning` — announce slice ready to commit (HITL) |
| Fail, unclear | `diagnose` |
| Fail, clear fix | `tdd` |

## Red flags

- False "verified" claims — [CONVENTIONS.md](../../CONVENTIONS.md#evidence-before-claims)
- Running full AC / whole-branch suite here (belongs in `finishing`)
- Using `code-review` as substitute for slice behavior checks
- Skipping report before commit announcement

## Checklist

- [ ] Slice behavior identified
- [ ] Relevant automated checks run; failures documented with output
- [ ] Minimal direct check on changed behavior (or explicit skip with reason)
- [ ] Report produced; no false "verified" claims
