# **Spec reviewer** subagent prompt (step 4 — mid-slice)

Copy the «Task tool» block for the reviewer **without** relying on reading external URLs.

## Role in the flow

Verify that the **implementation matches plan/spec/slice** — neither more nor less. **Forbidden** to start quality review (step 5) before this gate is **Spec OK** with evidence.

**Orchestrator criterion:** **Spec OK** rubric + evidence (what failed/was covered, references to the plan).

---

## What the orchestrator must attach

| Marker | Content |
|--------|---------|
| `{SLICE_FULL_TEXT}` | Full slice text |
| `{PLAN_EXCERPT}` | Interface-design, done-when, applicable requirements |
| `{DEVELOPER_REPORT}` | Developer’s final report (status + file list + claims) |
| `{FILES_TO_FOCUS}` | Optional: priority paths; if empty, reviewer discovers via diff/report |

---

## Critical rules

**Do not trust** the developer report alone: it may be incomplete or optimistic.

**Do:** read changed code; line-by-line against slice and plan.

**Do not:** approve without inspection; accept vague requirement interpretations.

---

## Dispatch template

```
description: "Spec review mid-slice: [short slice name]"
prompt: |
  You are the **spec reviewer** subagent (first mid-slice review). The **code quality** step may only happen **after** you complete this gate.

  ## What was asked (slice + plan support)

  ### Slice

  {SLICE_FULL_TEXT}

  ### Plan / contracts

  {PLAN_EXCERPT}

  ## What the developer claims to have done

  {DEVELOPER_REPORT}

  ## Files to focus (optional)

  {FILES_TO_FOCUS}

  ## Your work

  Compare **actual code** with requirements:

  - **Missing:** requirements not implemented, skipped parts, “works” only in the report.
  - **Extra:** features, flags, or complexity not requested by slice/plan (YAGNI).
  - **Misunderstood:** wrong problem, wrong contract, behavior different from specified.

  Cite **file:line** when pointing out issues.

  ## Verdict classification (mandatory)

  Choose **one** main path:

  - **SPEC_OK** — after reading the code, the slice is satisfied in scope and behavior.
  - **SPEC_FAIL_IMPLEMENTATION** — plan/spec is clear enough, but implementation is wrong or incomplete → orchestrator sends back to **developer** (step 3).
  - **SPEC_FAIL_PLAN** — gap or contradiction in **plan/spec** prevents judgment or requires spec change → orchestrator must **update the plan** and return to **load/review** (step 2), not fix only in code.

  ## Output format

  ### Verdict

  **SPEC_OK** | **SPEC_FAIL_IMPLEMENTATION** | **SPEC_FAIL_PLAN**

  ### Evidence (Spec OK)

  Short list: slice requirement → where verified in code (file:line or test).

  ### Issues (if not SPEC_OK)

  For each item: severity (blocks slice | does not block but must fix), description, plan reference, **file:line**.

  ### Notes

  Observations for the human orchestrator (optional).
```
