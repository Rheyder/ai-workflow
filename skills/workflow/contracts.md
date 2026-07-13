# Workflow Contracts

Input, action, and output per phase. Canonical overview: [WORKFLOW.md](../WORKFLOW.md).

## 1. Spec

**Input:** Idea, problem, bug, request, or raw requirement.

**Action:**

- Understand the problem.
- Remove critical ambiguities.
- Define scope and out of scope.

**Output:**

- Short spec.
- Acceptance criteria.
- Known risks.

**Skill:** `to-spec` (optional: `context-discovery`).

---

## 2. Plan

**Input:** Approved or sufficiently clear spec.

**Action:**

- Break into vertical slices.
- Order by dependency and risk.
- Define test / verification strategy per slice.

**Output:**

- Issues or small tasks.
- Criteria per task.
- Command or method of verification.

**Skill:** `to-issues`.

---

## 3. Branching

**Input:** Selected task or slice.

**Action:**

- Confirm clean working tree.
- Create or select working branch.
- Record change baseline.

**Output:**

- Branch ready.
- Known baseline.

**Skill:** `git-workflow-and-versioning`.

---

## 4. Build

**Input:** Task, acceptance criteria, baseline.

**Action:**

- Implement the smallest useful change.
- Prefer TDD for new business logic, bugs, or new behavior.
- Avoid extra scope.

**Output:**

- Implemented code.
- Tests created or updated.
- Notes on relevant decisions.

**Skill:** `tdd`.

---

## 5. Verify

**Input:** Implemented change.

**Action:**

- Run tests.
- Validate behavior.
- Check UI, API, integration, or affected flow when applicable.

**Output:**

- Verification evidence.
- Known failures or success confirmation.

**Skills:** `slice-verification` (current slice); `finishing-a-development-branch` Step 1 (whole branch before ship).

---

## 6. Review

**Input:** Diff, change context, verification evidence.

**Action:**

- Review correctness, tests, security, maintainability, integration, delivery — **only applicable axes**.
- Classify findings by severity.
- State blockers before cosmetic suggestions.

**Output:**

- Findings by severity.
- Objective recommendations.
- Delivery blockers if any.

**Skill:** `code-review` (checklists on demand).

---

## 7. Simplify

**Input:** Working, verified code.

**Action:**

- Remove accidental complexity.
- Improve names.
- Reduce duplication.
- Preserve behavior.

**Output:**

- Simpler code.
- Tests still passing.

**Skill:** `code-simplification`.

---

## 8. Ship

**Input:** Verified, reviewed, simplified change (or accepted risk).

**Action:**

- Prepare delivery.
- Run final checklist ([DEFINITION_OF_DONE.md](../DEFINITION_OF_DONE.md)).
- Summarize change, risk, and evidence.

**Output:**

- Ready for merge, PR, release, or handoff.
- Explicit next step.

**Skill:** `finishing-a-development-branch`.
