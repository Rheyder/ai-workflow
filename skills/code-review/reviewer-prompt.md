# Reviewer prompt (sub-agent or second pass)

Replace placeholders before sending:

| Placeholder | Content |
|-------------|---------|
| `{WHAT_WAS_IMPLEMENTED}` | What was built in this range |
| `{PLAN_OR_REQUIREMENTS}` | Plan, slice, interface-design, or reference requirements |
| `{BASE_SHA}` | Base commit of range |
| `{HEAD_SHA}` | Head commit of range |
| `{DESCRIPTION}` | Short diff summary |

---

Act as production-readiness code reviewer.

**Task:**

1. Review `{WHAT_WAS_IMPLEMENTED}`.
2. Compare with `{PLAN_OR_REQUIREMENTS}` and slice interface-design when present.
3. Assess quality, architecture, and tests.
4. Classify findings by severity.
5. Binary verdict aligned with rubric in `SKILL.md` (Rubric section).

## What was implemented

{DESCRIPTION}

## Requirements / plan

{PLAN_OR_REQUIREMENTS}

## Git range

**Base:** `{BASE_SHA}`  
**Head:** `{HEAD_SHA}`

```bash
git diff --stat {BASE_SHA}..{HEAD_SHA}
git diff {BASE_SHA}..{HEAD_SHA}
```

## Supporting checklist (beyond core rubric)

**Quality**

- Separation of concerns?
- Errors/failures handled consistently with project norms?
- Types/contracts coherent?
- Reasonable DRY?
- Edge cases on changed paths?

**Architecture**

- Defensible decisions?
- Performance/security implications where touched?

**Tests**

- Tests exercise real behavior (not mocks only)?
- Critical paths covered or justified?

**Requirements**

- Plan scope respected (no surprise public contracts)?

## Output format

### Strengths

[Be specific.]

### Issues

#### Critical (blocking)

[Bugs, security, data loss, broken functionality]

#### Important (fix before continuing)

[Fragile design, meaningful test gaps, poor error handling]

#### Minor (optional)

[Style, micro-optimizations, docs]

Per item: file:line when applicable, what is wrong, why it matters, how to fix if non-obvious.

### Recommendations

[Process or structure improvements]

### Assessment

**Ready for merge / next step?** Yes / No / After fixes — one technical sentence of rationale.

## Reviewer rules

**Do:** realistic severity; specificity; explain why; acknowledge quality; clear verdict.

**Avoid:** "looks ok" without inspection; nitpick as critical; feedback outside range; vagueness.
