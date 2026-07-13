# Transversal conventions

Canonical rules for all skills in this package. Individual skills link here instead of repeating full text.

## Skill file shape

Core workflow skills use this structure in `SKILL.md`:

- YAML `name` and short `description` (trigger in the first sentence)
- **When to use** / **When not to use**
- **Expected input** / **Process** / **Expected output**
- **Minimal checklist**
- Long examples → `REFERENCE.md`, `EXAMPLES.md`, or checklists in subfolders

Link here for session, evidence, and concise communication instead of repeating full text.

## Single agent session

Every skill runs in **one** conversational agent in the **current** session. Complete the work yourself — do not spawn sub-agents, parallel workers, or background runs mid-skill.

When a skill needs multiple passes (e.g. design alternatives), run them **sequentially** in the same session.

A **new** session is only for an explicit handoff (`handoff`) or a user-started follow-up — not parallel workers mid-skill.

## Concise communication

**Default: short answers.** Prefer the minimum text that still fully answers the request — bullets over paragraphs, one clear recommendation over a long comparison, no filler, repetition, or postamble.

**Go longer only when it earns its length**, for example:

- Trade-offs, risks, or ambiguous requirements that need a real explanation
- Findings that must be actionable (review, diagnosis, verification reports)
- Step-by-step procedures the user must follow
- The user explicitly asks for depth, examples, or teaching

**Never shorten at the cost of:**

- [Evidence before claims](#evidence-before-claims) (command output, pass/fail, blockers)
- Required skill templates (e.g. verification or review report sections)
- Safety-critical detail (security, data loss, breaking changes)

Optional extra compression: `caveman` — only when the user invokes it; does not waive evidence or required reports.

## Evidence before claims

No “verified”, “passing”, push/PR, or merge claims without **fresh command output in this session**.

1. **Identify** — proving command for the claim
2. **Run** — full command here (not a prior report alone)
3. **Read** — output and exit codes
4. **Verify** — output supports the claim
5. **Then claim**

**Enforced in:**

| Scope | Skill |
|-------|-------|
| Current slice | `slice-verification` |
| Whole branch (before push/PR) | `finishing-a-development-branch` Step 1 |

Branch procedure and report template: [finishing-a-development-branch/verification-report.md](skills/finishing-a-development-branch/verification-report.md). Slice report: [slice-verification/verification-report.md](skills/slice-verification/verification-report.md).

## Implementation discipline

Behavioral guidelines when writing, reviewing, or refactoring code. Biased toward caution over speed; use judgment on trivial tasks. Complements [evidence before claims](#evidence-before-claims) (operational gates) with general coding habits.

### Think before coding

- State assumptions explicitly; ask when uncertain.
- If multiple interpretations exist, present them — do not pick silently.
- If a simpler approach exists, say so; push back when warranted.
- If something is unclear, stop, name what is confusing, and ask.

### Simplicity first

- Minimum code that solves the problem; nothing speculative.
- No features, abstractions, flexibility, or error handling beyond what was asked.
- If the solution is much larger than necessary, simplify.

### Surgical changes

- Touch only what the request requires; match existing style.
- Do not “improve” adjacent code, comments, or formatting; do not refactor unrelated code.
- Mention unrelated dead code; do not delete it unless asked.
- Remove orphans **your** changes created (unused imports, variables, functions).
- Every changed line should trace directly to the user's request.

### Goal-driven execution

- Turn tasks into verifiable goals (e.g. invalid-input tests before validation; reproducer before bug fix; tests green before/after refactor).
- For multi-step work, state a brief plan: `step → verify: check` per step.
- Weak criteria (“make it work”) need clarification before large implementation.

## Repo standards discovery

When implementing, simplifying, or reviewing, skim applicable repo standards: `AI_WORKFLOW.md`, `docs/ai-workflow/conventions.md`, `CONTRIBUTING.md`, `STYLE.md`, `STANDARDS.md`, `STYLEGUIDE.md`, `docs/adr/`. Mirror nearby patterns. Host-specific files (`AGENTS.md`, `CLAUDE.md`) are optional adapters — see [ADAPTERS.md](ADAPTERS.md).
