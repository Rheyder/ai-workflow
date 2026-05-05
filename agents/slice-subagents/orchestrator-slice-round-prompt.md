# **Task-orchestrator** package — steps without a dedicated subagent (1, 2, 6, 7, 8)

Checklist and templates for the **task-orchestrator** agent in phases that **do not** use subagents `slice-developer`, `slice-spec-reviewer`, `slice-quality-reviewer`. Complements `.cursor/agents/task-orchestrator.md`.

---

## Step 1 — Branch

1. Confirm convention **`<track>/<artifact-slug>`** (solo mode in this repo).
2. Create or checkout slice branch; **do not** implement on `main`/`master` without explicit consent.
3. Isolated worktree only when `git-worktrees` or the plan requires it.

Record in chat: **active branch** + **slice name**.

---

## Step 2 — Load and review plan

1. Read `plan-*` and **full** text of current slice.
2. If there are **gaps**, **unacceptable risk**, or non-executable instructions → **stop for HITL** with an objective list of what is missing; **do not** dispatch subagents before that.
3. If unblocked: explicitly record that this round was **“accepted”** for execution (chat and/or `status` update on plan).

---

## Step 6 — Summary to the human

Aggregate in one block:

| Field | Content |
|-------|---------|
| Branch | Full name |
| Changes | What changed (bullets) |
| TDD | RED/GREEN evidence when applicable |
| Spec (step 4) | Verdict + pointers |
| Quality (step 5) | Verdict + rubric summary |
| Commands | List **and** outputs/exit codes verified this round |
| Plan | Slice status in `plan-*` |

**English** (`en_US`) for this summary and for suggested commit messages (per `.cursor/rules/language.mdc`).

---

## Step 7 — Ralph cap

- **One Ralph attempt** = run **once** path **3 → 4 → 5** until **QUALITY_OK** at step 5 **or** justified abort earlier.
- **Environment/tooling** failures do not count as an attempt until fixed and resumed.
- **Maximum 3** full cycles of that path **per slice**.
- On **3rd** without verifiable progress → **stop HITL**; record hypothesis, decision, next step in `plan-*` or `validation-*`.

---

## Step 8 — Slice DoD and delivery (HITL)

### Canonical rule

Same as `.cursor/rules/commit-push-hitl.mdc` and `.cursor/rules/verification-before-completion.mdc`; index: `.cursor/reference/workflow-conventions.md` § **Cross-skill alignment**.

### Gate before announcing

- Slice `done-when` satisfied.
- Reviews and commands with **fresh evidence** (per verification rule above).

### Post-slice announcement (copy and fill)

```markdown
**Slice N ready to commit.**
- Files: `<path1>`, `<path2>` ...
- Change: <1–3 bullets>
- Suggested message: `<type>(<scope>): <short description>`
- Next: <slice N+1 / awaiting decision / track DoD satisfied — ready for push and PR>

Commit yourself or should I run it?
```

### Final track delivery (when applicable)

- Prepare **`task-delivery`**: changelog, selective stage, message, cite `validation-*` if present, PR proposal; push/PR remain HITL.

---

## Subagent dispatch order (steps 3–5)

1. **Developer** — `.cursor/agents/slice-subagents/slice-developer-prompt.md`
2. **Spec reviewer** — only after useful developer report; `slice-spec-reviewer-prompt.md`
3. **Quality reviewer** — only after **SPEC_OK**; `slice-quality-reviewer-prompt.md`

**Forbidden:** quality before accepted spec.
