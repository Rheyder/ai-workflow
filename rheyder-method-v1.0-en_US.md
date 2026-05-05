# Rheyder method — AI-assisted delivery workflow (v1.0 — fused materialization)

Method for organizing work with agents and skills: risk triage → tracks with stages, named artifacts, and canonical roles. **Cross-cutting principles** (single definition in **Core concepts** and **Interface-design and vertical slices**): contracts respected; vertical slices; concision (`caveman`).

**This document** is the normative baseline **v1.0** (tracks, extended conventions, templates) **aligned** with `.cursor/` in this checkout (skills, agents, rules). **Last revised:** 2026-05-04.

**Audience:** solo developer (default mode for this repo) with IDE agents; the team enters only via GitHub PR review.

**Prerequisites:** local skills; `git worktree` on demand; harness/CI when the track requires.

**Maintenance:** when creating or removing artifacts under `.cursor/` or new method `.md`/`.mdc`, update **this** page and [.cursor/README.md](../README.md).

**Incremental implementation:** when materializing a new slug, keep **self-containment** under `.cursor/` and the **minimum viable skill** criterion in [Skills and agents / Roadmap](#materialization-roadmap). **Derive** new content from upstream bases **mattpocock/skills** and **obra/superpowers** (repository identifiers — URLs in [Appendix B — Methodological bases](#appendix-b--methodological-bases)); always merge into text published under `.cursor/`. **`SKILL.md` authoring (writing-skills + write-a-skill):** canonical copy in **`.cursor/reference/skill-authoring/`** and consolidation by the **`skill-writer`** agent (see [Catalog](#catalog)). For agent execution, the reference is files under `.cursor/` (forbidden to use only “read § of this method” as a single step — see [Self-contained Cursor materialization](#self-contained-cursor-materialization)). **Deferred candidate** rules (e.g. `interface-design-slices.mdc`) remain valid until promotion — table under [Rules `.mdc`](#rules-mdc).

Section **00** is triage; sections **1–4** map 1:1 to tracks **Simple**, **Normal**, **Complex**, and **Hotfix**. Everything that is cross-cutting norm sits in **Core concepts**, **Interface-design and vertical slices**, or **Execution conventions** — wording is unique there and referenced from tracks.

## In 30 seconds

- **Execution pattern:** see [**Interface-design and vertical slices**](#interface-design-and-vertical-slices) and [**Concision (`caveman`)**](#concision-caveman).
- Pick **one** track in `00 — Triage`; watch for wrong-track signals during execution.
- Materialize artifacts with **standard YAML header** (`status`, `version`, `date`).
- Use `task-orchestrator` to coordinate slices and stop criteria; **`workflow-harness`** (dedicated skill not yet materialized in this repo — see Catalog) or repeatable commands documented in `plan-*`/README: always apply `verification-before-completion`.
- Within the slice **before advancing:** **two intra-slice reviews** (`slice-spec-reviewer` → `slice-quality-reviewer`, see Cursor agents) + **`code-review`** where applicable; **final pass** before delivery keeps **`code-review`** and materializes **`validation-*`**. Meet track and slice **DoD**.
- **Commit and push are always user HITL** (Conventions/Commit and delivery) — agent announces and waits.
- Follow `knowledge-*` policy per track; the team reviews the **GitHub PR** — final external gate.

## Solo mode (default for this repo)

You accumulate the `task-orchestrator`, `developer`, and `reviewer` roles. They exist as **method roles** triggered as **skills, rules, and `.cursor/agents/*`**. In this checkout, **`developer`** maps to **`slice-developer`** subagent; **intra-slice `reviewer`** functions map to **`slice-spec-reviewer`** (plan/spec alignment) and **`slice-quality-reviewer`** (quality + evidence); the **orchestrator** is **`task-orchestrator`**. **Cursor skill authoring/refinement:** use **`skill-writer`**, pointing to **`.cursor/reference/skill-authoring/`** (writing-skills + write-a-skill packages); materialization usable by the agent always lives under `.cursor/`.

- **HITL = you deciding consciously.** Canonical points: **every commit/push** (see Conventions/Commit and delivery), Ralph blow-ups, gates between phases in Complex, overlapping scope across parallel branches.
- **The only external reviewer is the team, on the GitHub PR.** Treat `validation-*` as **self-review** before opening the PR.
- Everything the method calls a “process barrier” (gates, review scope, DoD) becomes **your discipline**, not paperwork between people.
- When the text says “agent X” or “role Y”, read it as **a prompt/skill you invoke**, with clear output. The separation is mental — do not try to play two people in the same pass.

## Table of contents

- [Core concepts](#core-concepts)
- [Interface-design and vertical slices](#interface-design-and-vertical-slices)
- [00 — Triage](#00--triage)
- [1 — Simple](#1--simple)
- [2 — Normal](#2--normal)
- [3 — Complex](#3--complex)
- [4 — Hotfix](#4--hotfix)
- [Execution conventions](#execution-conventions)
- [Templates](#templates)
- [Skills and agents](#skills-and-agents)
  - [Self-contained Cursor materialization](#self-contained-cursor-materialization)
- [workflow-pack package and symlink](#workflow-pack-package-and-symlink-in-consumer-projects)
- [Appendix A — Inventory vs derivation](#appendix-a--inventory-vs-derivation)
- [Appendix B — Methodological bases](#appendix-b--methodological-bases)

**Recommended reading order:** Solo mode → Triage → Core concepts → chosen track → Conventions/Templates as needed.

---

## Core concepts


| Term               | Meaning                                                                                                                                                                                                                         |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **HITL**           | *Human-in-the-loop*. Solo: explicit pause where you **choose** something the agent should not choose alone. Canonical points listed under **Solo mode**; commit/push detail under **Conventions/Commit and delivery**.          |
| **Vertical slice** | Thin end-to-end delivery slice tied to a named portion of interface-design (not “just one layer”).                                                                                                                             |
| **Interface-design** | Contracts and boundaries already agreed (API, types, schemas, UI, ADR, OpenAPI, Storybook, spec/plan sections) that code must respect. May be **minimum contract bullets** when no structured design exists yet.              |
| **Ralph loop**     | Iterative agent cycle until stop criterion (tests, green slice, attempt limit). Has a **hard ceiling** — see Conventions/Loop.                                                                                                   |
| **Gate**           | Mandatory barrier before advancing (review between slices, security gate, end of phase in Complex).                                                                                                                             |
| **Harness**        | Repeatable layer around execution (CI, smoke, tests, eval) with explicit stop criteria. Does not decide scope nor replace HITL.                                                                                                  |
| **Slug**           | Stable `kebab-case` identifier for skills, agents, and references.                                                                                                                                                             |


### Tags in tables


| Tag       | Meaning                                                                     |
| --------- | --------------------------------------------------------------------------- |
| `[skill]` | Reusable instruction (checklist, format, criterion). Does not run alone.  |
| `[agent]` | Role that executes with tools; loads skills.                               |


---

## Interface-design and vertical slices

Single global rule of the method — **priority over divergent replicas** in `.mdc` or skills until deliberate promotion. For **humans** reading the method, this section is the source; for **agents** operating only with Cursor artifacts, each affected skill, rule, or agent must **embed in its own file under `.cursor/`** the necessary excerpt (what to check, what is forbidden, what to name in `plan-*`) — see [Self-contained Cursor materialization](#self-contained-cursor-materialization).

Marker `**REQUIRED:**` in a skill means flow obligation **described in the skill itself** (or injected rule), not “open the method as the only step”. Optional citation of this doc’s slug or anchor is a reminder for humans, not a substitute for excerpt in `.cursor/`.

**Interface-design — rule:** all produced or changed code must respect **current interface-design** (contracts and boundaries already agreed in spec/plan/ADR/OpenAPI/Storybook/UI/tokens, or **minimum contract bullets** when no structured design exists).

- **Forbidden:** implementing new contracts “from memory” that conflict with design or anticipate design not yet materialized.
- **If design must change:** update **first** (or in verifiable lockstep) the artifact — aligned with **Spec drift** (see Conventions/Artifacts).

**Vertical slices — rule:** planning and execution are **always** organized as vertical slices — end-to-end increment traceable to a defined portion of interface-design. “Infra-only” / “layer-only” plan without deliverable value contract **violates** the rule.

**In `plan-*`, each slice must name:**

- (a) which portion of interface-design it covers (link/section, or minimum contract bullets);
- (b) end-to-end value criterion (slice DoD — see Templates).

**On review (between slices and final):** confront diff/code against `plan-*` on (i) cited interface-design and (ii) vertical cut. Material divergence becomes spec drift, not silently approved.

**Shortcuts (Simple Express, trivial Hotfix)** do not waive the rule: the patch must stay aligned with existing design; if nothing is written, register minimum contract in bullets before expanding public surface.

---

## 00 — Triage

**Quick decisive question:**

- Production incident / active degradation? → **Hotfix**
- No incident: multiple systems or high cost of error? → **Complex**
- No incident: need formal spec to close scope? → **Normal**
- Otherwise (localized change, low risk) → **Simple**

**Safety check (independent of track):** does the change touch authN/authZ, crypto, PII, or sensitive data?
→ If yes: explicit security review gate before merge (`security-review` skill or checklist in `plan-*`). Record in `plan-*`.


| Track       | When to use                                                                                           |
| ----------- | ----------------------------------------------------------------------------------------------------- |
| **Simple**  | Little ambiguity, localized change, low risk; no formal spec.                                       |
| **Normal**  | Feature-sized; clear scope after specification; traceability with standard artifacts.               |
| **Complex** | Multiple systems, architecture decisions, high cost of error; discovery/spike and gates between phases.|
| **Hotfix**  | Broken production / incident; minimal fix with post-fix record.                                     |


**Explicit distinction:** **Hotfix** prioritizes containment/restoration of incident; **Simple** prioritizes fast delivery without incident.

### Wrong-track signals (check during execution)

→ Reclassify to **Complex** if:

- Two or more systems appeared not mapped at triage.
- The plan had to be rewritten after implementation started.
- Data, security, or unforeseen public contract risk emerged.
- `plan-*` exceeded 7 slices.
- Brainstorming produced 3+ questions without clear answers.

→ Reclassify to **Normal** if:

- **Simple** produced a formal `spec-*` (scope grew).
- **Hotfix** required refactor beyond minimal fix.

→ Reclassify to **Simple/Hotfix** (descending) if:

- **Complex** discovery reduced risk to a single controlled slice.

---

## 1 — Simple

**Objective:** deliver fast with minimal barrier.

### Modes: Express vs Assisted


| Mode          | Entry criterion (all)                                                                      | Stages                                                                                                  |
| ------------- | ------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------- |
| **Express**   | One area · no changed public contract · no migration · fast validation available           | Brainstorming → `developer` → user announcement → **HITL: commit/push** (Conventions/Commit and delivery) |
| **Assisted**  | Any Express criterion fails                                                                | Full 6-stage flow below                                                                                |


If an Express criterion fails during execution, migrate to Assisted without negotiation.

**Fast validation available (Simple Express criterion):** at least **one** of — project linter, automated tests focused on what changed, smoke documented in `plan-*` or README — **runnable locally in ≤ 2 minutes** after the change and run **in this session** before declaring the slice ready. Manual UI check only counts with **reproducible steps** (1–2 lines in artifact). If the repo has no such command yet, **do not use Express** until `plan-*` lists a minimal command (may be a dedicated slice).

### Stages — Assisted


| #   | Stage           | Skill / Agent                                                                      | Artifact                         |
| --- | --------------- | ---------------------------------------------------------------------------------- | -------------------------------- |
| 01  | Brainstorming   | `brainstorming`                                                                    | bullets in chat                  |
| 02  | Plan            | `plan-writing`                                                                     | `plan-YYYY-MM-DD-name.md`        |
| 03  | Implementation  | `task-orchestrator` (+ `developer`, `tdd-cycle`, `workflow-harness`)             | —                                |
| 04  | Review (diff)   | `reviewer` final pass (loads `code-review`; scope: diff + direct dependencies)      | `validation-YYYY-MM-DD-name.md`  |
| 05  | Delivery        | `task-delivery`                                                                    | `changelog-YYYY-MM-DD-name.md`   |
| 06  | Learning        | `knowledge-base`                                                                   | `knowledge-*` (optional)         |


**Notes:**

- `plan-*`, even simple, follows global **vertical slices** + **interface-design** per slice.
- Worktree only if another initiative runs in parallel (see Conventions/Branches and worktrees).
- Ralph with default ceiling; Simple harness weight — see Conventions/Loop and orchestration.

---

## 2 — Normal

**Objective:** specify and plan before coding in slices; review over the full change surface on the branch.


| #   | Stage           | Skill / Agent                                                                   | Artifact                         |
| --- | --------------- | ------------------------------------------------------------------------------- | -------------------------------- |
| 01  | Brainstorming   | `brainstorming`                                                                 | `spec-YYYY-MM-DD-name.md`        |
| 02  | Plan            | `plan-writing`                                                                  | `plan-YYYY-MM-DD-name.md`        |
| 03  | Implementation  | `task-orchestrator` (+ `developer`, `tdd-cycle`, `workflow-harness`)           | —                                |
| 04  | Review (branch) | `reviewer` final pass (loads `code-review`; scope: branch vs merge target)      | `validation-YYYY-MM-DD-name.md`  |
| 05  | Delivery        | `task-delivery`                                                                 | `changelog-YYYY-MM-DD-name.md`   |
| 06  | Learning        | `knowledge-base`                                                                | `knowledge-*` (recommended)      |


**Notes:**

- `plan-*` materializes vertical slices + interface-design per slice before coding larger scope (global rule).
- Rollback in `plan-*` when applicable (structure: Templates/Rollback).
- Normal harness weight — see Conventions/Loop and orchestration.

---

## 3 — Complex

**Objective:** reduce uncertainty before coding; gates between sensitive phases.

**Pure spike:** discovery may end as `spec-YYYY-MM-DD-name-spike.md` without code if the decision is “do not build”.


| #   | Stage                                   | Skill / Agent                                                                        | Artifact                                  |
| --- | --------------------------------------- | ------------------------------------------------------------------------------------ | ----------------------------------------- |
| 01  | Discovery / spike                     | `technical-discovery`                                                                | short; may embed in `plan-*`/`spec-*`      |
| 02  | Brainstorming + spec hardening          | `brainstorming` (+ NFR, limits, rollback)                                            | `spec-YYYY-MM-DD-name.md`                 |
| 03  | Phased plan (with decisions / ADR-lite)| `plan-writing`                                                                       | `plan-YYYY-MM-DD-name.md`                 |
| 04  | Implementation                        | `task-orchestrator` (+ `developer`, `tdd-cycle`, **`workflow-harness` mandatory**)    | —                                         |
| 05  | Review (branch)                       | `reviewer` final pass (loads `code-review`)                                          | `validation-YYYY-MM-DD-name.md`           |
| 06  | Delivery                              | `task-delivery`                                                                      | `changelog-YYYY-MM-DD-name.md`            |
| 07  | Learning                              | `knowledge-base`                                                                     | `knowledge-*` (**mandatory**)             |


**Notes:**

- **Spec hardening** (part of step 02) exits when: (a) NFRs defined or marked N/A; (b) limits and constraints documented; (c) rollback recorded **or** explicit decision that it does not apply (structure: Templates/Rollback).
- **Decisions / ADR-lite** live as a `plan-*` section (or separate ADR when the repo has convention).
- **HITL pauses** between sensitive phases when `plan-*` defines (beyond canonical HITL: Conventions/Commit and delivery).
- Same artifacts as Normal — difference is **rigor + gates + HITL pauses**, not new file types.

---

## 4 — Hotfix

**Objective:** restore / contain production; no “bonus” refactor.


| #   | Stage               | Skill / Agent                                              | Artifact                         |
| --- | ------------------- | ---------------------------------------------------------- | -------------------------------- |
| 01  | Quick brainstorming | `brainstorming`                                            | bullets                          |
| 02  | Systematic debug    | `systematic-debugging`                                     | —                                |
| 03  | Implementation      | `task-orchestrator` (lean loop; trivial = `developer` only) | —                                |
| 04  | Review (focused diff)| `reviewer` final pass (loads `code-review`)               | `validation-YYYY-MM-DD-name.md`  |
| 05  | Delivery            | `task-delivery`                                            | `changelog-YYYY-MM-DD-name.md`   |
| 06  | Learning            | `knowledge-base` (optional)                                | see post-mortem in Templates     |


**Notes:**

- **Compared to Normal:** omits long `spec-*` and structured `plan-*` — bullets + debug.
- **Do not confuse** with skipping step 01 (minimum clarification remains).
- **Minimal rollback** in `plan-*` or `changelog-*` when there is a written plan.
- **Serious incident** → variant `knowledge-YYYY-MM-DD-name-postmortem.md` (Templates/Post-mortem).
- Minimal harness tied to symptom (reproduction + regression/smoke) — see Conventions/Loop and orchestration.

---

## Execution conventions

### Artifacts

**Name:** `type-YYYY-MM-DD-name.md`. **Types:** `spec-*`, `plan-*`, `validation-*`, `changelog-*`, `knowledge-*`. Multiple initiatives same day need discriminative `name`. Revisions same day: `spec-2026-05-01-payment-v2.md`.

**Standard YAML header:**

```yaml
---
status: draft | approved | superseded
version: v1
date: YYYY-MM-DD
---

```

Superseded artifact → mark prior as `status: superseded` (do not delete).

**Spec drift:** if implementation diverges **materially** from `spec-*` before merge: record divergence in `plan-*` **or** update `spec-*` to new version before merge. Material = scope change, public contract, interface-design, risk, rollback, technical constraint, or observable behavior.

**Merge rule:** do not merge with knowingly stale `spec-*` without explicit note in `plan-*`.

**Sensitive data:** do not record secrets in artifacts; mask tokens/credentials in evidence; for incidents, prefer link to official evidence vs raw dump.

### Branches and worktrees

**Branch naming:** `<track>/<artifact-slug>` (e.g. `normal/2026-05-01-payment-refactor`). Slug must match artifact names.

**Worktree — when to create (any one suffices):**

- Another active initiative exists in the same repo.
- Track is **Complex**.
- **Hotfix** running in parallel with ongoing Normal/Complex.

Otherwise: branch directly, no worktree.

**Parallel branches with overlapping area:**

- Detect early: `git diff main...branch-a -- <path>` vs `git diff main...branch-b -- <path>`.
- Overlap → explicit merge order in both `plan-*`; second rebases on first after merge.
- Overlap on public contract / security → HITL before any implementation advances.

### Commit and delivery (mandatory HITL)

**Single canonical rule:** **commit and push are always done by the user.** The agent never runs `git commit` / `git push` on its own initiative — regardless of track, mode, or slice size.

**Flow:**

1. Slice (or final delivery) meets corresponding **DoD** — including green review pass and `verification-before-completion` satisfied.
2. Agent **explicitly announces** to user: *slice ready to commit*, listing (a) touched files, (b) summary of changes (1–3 bullets), (c) suggested commit message, (d) next slice or status.
3. User decides:
   - **(a) commits on their own** — agent proceeds to next slice or waits for new instruction;
   - **(b) instructs agent** to run (`stage + commit + push`) — only then `task-delivery` runs Git commands.

**`task-delivery` in solo flow:** **prepares** commit (selective stage, summary, suggested message, `changelog-*` update) and **announces**. Runs Git only when user asks explicitly in that interaction. **There is no implicit commit** at end of slice.

**Post-slice announcement — minimum format:**

```markdown
**Slice N ready to commit.**
- Files: `<path1>`, `<path2>` ...
- Change: <1-3 bullets>
- Suggested message: `<type>(<scope>): <short description>`
- Next: <slice N+1 / awaiting decision / track DoD satisfied — ready for push and PR>

Commit yourself or should I run it?
```

**Variants:**

- **Intermediate slice** → announcement above; no push (unless `plan-*` defines push per slice).
- **Final delivery (track DoD satisfied)** → announcement includes materialized `validation-*`, CI/harness status, and proposal to open PR. Push and PR opening are also HITL.
- **Hotfix** → same flow; announcement may be shorter, but **commit stays HITL**.

**Why canonical:** avoids silent agent commits, keeps `git push` as natural HITL point in solo mode (see Core concepts / HITL), and keeps authorship/audit under user control.

### Review

**Reviewer (method) at two moments — Cursor materialization in this repo:**

| Moment | Scope | When | Artifact |
| ------ | ----- | ---- | -------- |
| **Intra-slice (double gate)** | (1) Plan/spec/slice ↔ code — **`slice-spec-reviewer`** agent + **`code-review`** skill per rubric; (2) Quality + evidence — **`slice-quality-reviewer`** + **`code-review`**. **Mandatory** orchestrator order: spec before quality. | Gate before closing current round/slice (numbered flow in [.cursor/agents/task-orchestrator.md](../agents/task-orchestrator.md)) | Typical `validation-*` only on **final pass**; intra-slice: log in chat or `plan-*` |
| **Final pass (branch / delivery)** | Overall context: focused diff Simple/Hotfix; full branch surface Normal/Complex — **`code-review`** skill, materializes **`validation-*`** | Before `task-delivery` | **Materializes `validation-*`** |

Not two people — distinct passes that in the IDE may be separate subagents/prompts or mental discipline on same user.

**PR variant without authorship (reviewing someone else’s PR):** final pass only, full diff; `validation-*` references PR and minimum touched interface if no plan.

**Code-review rubric (binary checklist):**

- Implementation adheres to interface-design cited in `plan-*`/slice (no “surprise” contracts)
- Structure aligned with vertical slices in `plan-*` (no layer-only delivery)
- No method/function over 20 lines without justification comment
- Zero hardcoded secrets (grep or secret-scanner verifiable)
- Each class has single identifiable responsibility by name
- **Tests / coverage:** if repo exposes metric on **merge-target CI**, coverage did not regress vs that baseline in this evidence; else if local coverage command exists, did not regress vs value recorded in `validation-*` or prior slice **of this** initiative (first time: record “before” once); if **no** coverage metric — existing tests for changed behavior pass and critical new paths have tests **or** explicit justification in `plan-*`/PR (mark N/A with list of what ran)
- No new `TODO` without linked issue/ticket
- No known anti-patterns of **this repo’s stack and style** (guide/ADR/README conventions if any); if no guide — minimum checklist: no obvious N+1 on changed paths, no inconsistent layer mixing, no ignoring stack error/transaction pattern
- Public contracts / external APIs not broken without versioning

**Definition of Done by track:**


| Track       | DoD                                                                                      |
| ----------- | ---------------------------------------------------------------------------------------- |
| **Simple**  | Green rubric + CI ok + `changelog-*`                                                      |
| **Normal**  | Green rubric + CI ok + approved `validation-*` + PR opened                             |
| **Complex** | Green rubric + CI ok + approved `validation-*` + HITL gates done + PR approved        |
| **Hotfix**  | Green rubric (focused diff) + regression/smoke passing + `changelog-*` or PR with acceptance |


For all, DoD assumes **interface-design and vertical slices** when explicit plan exists.

**Slice DoD** (per slice in `plan-*`):

Slice complete when:

- it is **vertical** (end-to-end, with value/contract criterion);
- code respects referenced interface-design (or divergence handled via Spec drift);
- objective delivered without expanding scope;
- slice tests passed + relevant local regression;
- open items became new slice / risk / pending decision;
- no unrecorded material divergence vs `spec-*`/`plan-*`;
- **“slice ready to commit” announcement delivered to user** in canonical format (see Conventions/Commit and delivery).

Next slice starts only when: (a) prior is green **and** user decided on commit (committed, asked agent to commit, or explicitly cleared to proceed without commit), **or** (b) prior is explicitly `blocked`.

**Cross-cutting principle (`verification-before-completion`):** no rubric item, track DoD, or slice DoD may be marked green without **fresh evidence produced now** — not “should pass”, not “passed last run”. Applies to Commit and delivery gate and before declaring Hotfix applied. See skill under **Direct adoption**.

### Loop and orchestration

**Ralph ceiling:**

- Maximum **3 attempts per slice** before mandatory HITL.
- **Attempt (Ralph) = one orchestrator pass round until quality accepted (`slice-developer` implementation + `tdd-cycle` where applicable → `slice-spec-reviewer` → `slice-quality-reviewer`)**, per [task-orchestrator.md](../agents/task-orchestrator.md). Aborted cycles due only to tooling/environment **do not count** — fix environment and continue.
- `plan-*` may override ceiling with explicit rationale.
- When invoking HITL, record in `validation-*` or `plan-*`: current attempt; blocking hypothesis; decision; next authorized step.

**Criterion:** if 3rd attempt produced no verifiable progress, next step is not “try again” — escalate (trim scope, change approach, ask for help).

**Stop criteria:** define **before** the loop measurable criteria (green tests, max time, slice “done”). Review gate between slices includes mandatory plan ↔ code check.

**Trivial vs orchestrated:**

Trivial when **all**:

- Localized area change (no relevant cross-cutting dependency).
- Does not alter public contract beyond what interface-design already defines.
- No data migration, infra, or sensitive security.
- Fast validation available (regression or smoke).

Any failure → orchestrated track flow (loop / slices / gates).

**Harness — weight by track** (skill **`workflow-harness`** when `.cursor/skills/workflow-harness/SKILL.md` exists *or* equivalent documented procedure):

- **Simple/Hotfix** — only what proves patch (reproduction + test/regression or smoke).
- **Normal** — explicit verification matrix during implementation; `plan-*` cites which checks run per slice.
- **Complex** — same as Normal + gates between sensitive slices when `plan-*` defines; harness **mandatory** in sense of **repeatable evidence** (CI or commands in README/`plan-*`); if dedicated skill missing in repo, document commands per slice.

**Minimum harness in repo (level 0):** **one** documented command (README or `plan-*`) proving sanity — e.g. `npm run lint`, `npm test`, `pytest`, stack equivalent. **CI not mandatory** for this level (CI is evolution). **First initiative** needing repeatable verification: include in `plan-*` a slice to **add + document** that command when missing.

**When there is not even a minimal command:** gate becomes manual test/lint/smoke execution with log in `validation-*` (`verification-before-completion`). Avoid eternal no-command state: treat building level 0 as explicit work in `plan-*`.

`validation-*` may (and should when applicable) cite harness outputs: logs, eval, CI status, smoke traces.

### Knowledge

**`knowledge-*` policy by track:**


| Track       | `knowledge-*`                                      |
| ----------- | -------------------------------------------------- |
| **Complex** | Mandatory                                          |
| **Normal**  | Recommended                                        |
| **Simple**  | Optional                                           |
| **Hotfix**  | Optional — minimum acceptance in `changelog-*` or PR |


**Learning in Complex** — do not skip without equivalent documented substitute. **Serious Hotfix** uses post-mortem variant — single definition in Templates/Post-mortem.

**Minimum metrics (optional, recommended Normal/Complex):**

```markdown
## Metrics
- Total time (brainstorming → push):
- Track reclassification: (no | yes — which and why)
```

Extra metrics (Ralph iterations, rework, escaped defects) — optional after volume (e.g. 6+ initiatives).

### Concision (`caveman`)

In any track and stage, agents and skills must be **concise**: bullets, verifiable criteria, only what’s needed for decision / execution / artifact.

`caveman` skill lives at **`.cursor/skills/caveman/SKILL.md`** (no always-on `.mdc` in this repo) — **opt-in** by prompt; **explicit opt-out** valid (“don’t use caveman on this task”, verbose mode).

### Track realignment

Execution differs from triage? Slice count growing? Open questions? → see **00 — Triage / Wrong-track signals**.

---

## Templates

### `plan-*`

**Minimum structure per slice:**

```markdown
### Slice N — <name>
interface-design: <links/sections in spec/ADR/OpenAPI/UI — or minimum contract bullets>
vertical-slice: <end-to-end criterion / value delivered>
status: pending | in-progress | done | blocked
done-when:
  - <verifiable criterion 1>
  - <verifiable criterion 2>
```

**Blocked slice:**

```markdown
### Slice N — <name>
status: blocked
blocked-by: <external dependency>
owner: <person/team/system>
next-check: YYYY-MM-DD
fallback: <follow another slice | pause | reduce scope | HITL>
```

**Rollback (mandatory Complex; optional/when applicable elsewhere):**

```markdown
## Rollback
- **Activation condition:** (e.g. error > X%, smoke failure post-deploy)
- **Steps:**
  1. ...
  2. ...
- **Owner:** (you / role)
- **Estimated time:**
- **Post-rollback validation:** (how to confirm restoration)
```

**Timebox (optional):** if Ralph or slice exceeds agreed time/attempts → HITL to re-cut.

**Chat context:** prefer **paths** and excerpts already in artifacts over pasting large code blocks.

### Handoff and resume

**Triggers:**

- (a) ~70% context consumed (signal: shorter/repetitive answers);
- (b) when closing vertical delivery (feature, PR, milestone);
- (c) before voluntarily ending session with work in progress.

In Cursor composer-2: up to ~75% when session is predominantly reading already-indexed code.

**Handoff package:**

```markdown
## Resume package — paste at top of new chat

- **Objective (one sentence):**
- **Track / phase:**
- **Branch / worktree:**
- **Artifacts:** (paths for @)
- **Already done:**
- **Next concrete step:**
- **Decisions / do-not:**
- **Open risks:**
  - [ ] [RISK] something that can go wrong — impact — mitigation
  - [ ] [UNCERTAINTY] something to validate — what to check
  - [ ] [PENDING DECISION] deferred choice — who decides / when
- **Accumulated context:** ~XX% — generated by [automatic | end of delivery | manual]
- **Verification commands:** (tests, lint, harness)
- **Attach in new chat (@):**
```

**Starter package — existing initiative** (no prior handoff):

```markdown
## Starter package — existing initiative

- **Known objective:**
- **Existing branch / worktree:**
- **Artifacts found:**
- **Apparent code state:**
- **Last relevant commit:**
- **Risks / uncertainties detected:**
- **First safe action:**
```

**Same chat, another day (optional):** note at end of `plan-*` three bullets — current slice (status), next action, something odd — or use **Resume package** if new chat context.

**Slug `chat-handoff-bullets`:** the **template format** of boxes below stays canonical in this section; a skill with this slug may still be materialized under `.cursor/skills/` when useful (today: use template here and in `plan-*`/chat).

### PR (for the team)

Since the team only sees the PR, it is the **primary communication artifact** — treat as external gate.

```markdown
## Summary
<2–4 bullets of what changes>

## Track and context
- Track: Simple | Normal | Complex | Hotfix
- Initiative / artifacts: <paths to spec-*, plan-*, validation-*>

## Slices delivered
- Slice 1 — <name> (link)
- Slice 2 — <name> (link)

## Look here first
- <where you want sharp team review>

## Rollback
<link to plan-* section or bullets>

## Self-checklist
- [ ] Binary rubric green
- [ ] CI ok
- [ ] Track DoD satisfied
```

### `SKILL.md` — versioning

YAML header (Rheyder convention) on every `SKILL.md`; for **discovery / description-only triggers** (“description trap”), see **writing-skills** package in **`.cursor/reference/skill-authoring/writing-skills/`**; for structure and file split see **write-a-skill** in **`.cursor/reference/skill-authoring/write-a-skill/`**. Consolidated flow: **`skill-writer`** agent + same tree in `skill-authoring/`:

```yaml
---
name: slug-name
description: Use when <third-person triggers — only what loads the skill; no mini-workflow in description — see [Templates / SKILL.md — versioning](#skillmd--versioning) and `.cursor/reference/skill-authoring/`>.
version: 1.0.0
last-updated: YYYY-MM-DD
method-version-compatible: rheyder-method >= 1.0
---

```

**Bump convention:** `patch` — text; `minor` — new criteria/steps; `major` — interface change (slug, expected artifacts, agents).

### Post-mortem (serious Hotfix)

Variant: `knowledge-YYYY-MM-DD-name-postmortem.md`.

```markdown
## Post-mortem
- **Root cause:**
- **Timeline:**
- **Impact:**
- **Detection:**
- **Fix applied:**
- **Preventive action:**
- **Preventive action owner:**
```

Mandatory when any: **(a)** users or external system **noticed** unavailability, visible data loss, or severe degradation (duration not criterion in solo mode — see note); **(b)** **same symptom** returns after fix (recurring incident); **(c)** involves authN/authZ, crypto, PII, or sensitive data. **Note:** when SLA/business applies, add quantitative threshold (e.g. minutes down) in `plan-*` or ADR — until then qualitative triggers suffice in solo mode.

---

## Skills and agents

### Self-contained Cursor materialization

Artifacts under `**.cursor/skills/**`, `**.cursor/agents/**`, and `**.cursor/rules/**` obey:

1. **Execution with loaded file only** — except what Cursor injects (`alwaysApply`, agent discovery) and **optional** paths to other skills in the **same** `.cursor/` when product requires; each such referenced file is **also** self-contained (no chain A→B→C where B only says “see A”).
2. **Upstream bases** — **mattpocock/skills** and **obra/superpowers** (Git in [Appendix B](#appendix-b--methodological-bases)), plus **this document** or excerpts in `plan-*` / `validation-*`, are **authoring/review input** where consolidated text **does not** yet exist under `.cursor/`; published `.cursor/` **embeds** excerpt per [Appendix A](#appendix-a--inventory-vs-derivation). **Explicit exception:** **writing-skills** and **write-a-skill** have **canonical copy** in **`.cursor/reference/skill-authoring/`** — prefer that path (and **`skill-writer`**) for `SKILL.md` authoring.
3. **Forbidden** to use “read section § … of this doc” (or only an external link) as **sole** normative step without summary/checklist **in** the skill, agent, or rule — except that for **skill-writing methodology**, required reading may be **only** what is under `.cursor/reference/skill-authoring/` + `skill-writer` instructions, as long as **final artifact** (`SKILL.md` in `.cursor/skills/`) stays self-contained.

For **depth** on checklist/TDD applied to skills (incl. “description trap”), use **`.cursor/reference/skill-authoring/writing-skills/`** (and pressure subagents per `testing-skills-with-subagents.md` in same directory). There is **no** invocable skill slug `writing-skills` in Catalog — **`skill-writer`** agent consolidates the flow.

### Catalog

**Merged v1.0**: table below aligns method with **first wave materialized** in `.cursor/` this checkout. **(pending)** entries are canonical method slugs **without** `SKILL.md` yet — keep stage in flow and document in `plan-*`/chat until file exists.

| Slug | Type | Coverage |
| -------------------------------- | ------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `brainstorming` | skill | Clarification / grill-me; profile by **00 — Triage** — see **Brainstorming — profile by triage**. **File:** `.cursor/skills/brainstorming/SKILL.md`. (human alias: *grill-me*) |
| `plan-writing` | skill | `plan-*` with vertical slices + interface-design per slice + rollback (when applicable). **File:** `.cursor/skills/plan-writing/SKILL.md`. |
| `tdd-cycle` | skill | Red / green / refactor per slice. **File:** `.cursor/skills/tdd-cycle/SKILL.md`. (bases: mattpocock `tdd` + superpowers TDD — inventory) |
| `git-worktrees` | skill | Worktree when “when to create” criterion is true. **File:** `.cursor/skills/git-worktrees/SKILL.md`. |
| `code-review` | skill | Binary rubric + adherence (interface-design, slices, security). Used on review passes. **File:** `.cursor/skills/code-review/SKILL.md`. |
| `verification-before-completion` | skill (norm injected by rule) | Evidence before claiming completion. **Always-on rule:** `.cursor/rules/verification-before-completion.mdc`. |
| `task-delivery` | skill | Prepares delivery + HITL announcement. **File:** `.cursor/skills/task-delivery/SKILL.md`. |
| `systematic-debugging` | skill | Hotfix and systematic debug. **File:** `.cursor/skills/systematic-debugging/SKILL.md`. |
| `improve-codebase-architecture` | skill | Architectural deepening under interface-design. **File:** `.cursor/skills/improve-codebase-architecture/SKILL.md`. |
| `caveman` | skill | Concision; `SKILL.md` only, no always-on `.mdc`. **File:** `.cursor/skills/caveman/SKILL.md`. |
| `language` | rule `.mdc` | pt-BR for user-facing text; rest `en_US`. **File:** `.cursor/rules/language.mdc`. |
| `base-creation` | rule `.mdc` | Self-contained authoring in clone. **File:** `.cursor/rules/base-creation.mdc`. |
| `karpathy-guidelines` | rule `.mdc` | LLM coding discipline. **File:** `.cursor/rules/karpathy-guidelines.mdc`. |
| `knowledge-base` | skill **(pending)** | Learn step / `knowledge-*` — **no** `.cursor/skills/knowledge-base/` yet; manual process or next materialization slice. |
| `technical-discovery` | skill **(pending)** | Discovery / spike (Complex step 01) — **no** dedicated `SKILL.md` here; repeat bullets in `spec-*`/`plan-*` until materialized. |
| `security-review` | skill **(pending)** | Security gate — checklist in `plan-*` or future skill. |
| `workflow-harness` | skill **(pending)** | Repeatable commands — use README/`plan-*` + evidence; **no** dedicated skill this checkout. |
| `chat-handoff-bullets` | skill **(pending)** | Format in [Templates / Handoff](#handoff-and-resume); optional future skill. |
| `task-orchestrator` | agent | Orchestrates vertical slice, Ralph, handoffs. **File:** `.cursor/agents/task-orchestrator.md`. |
| `slice-developer` | agent | **developer** role — implementation + TDD. **File:** `.cursor/agents/slice-developer.md`; prompt: `.cursor/agents/slice-subagents/slice-developer-prompt.md`. |
| `slice-spec-reviewer` | agent | Intra-slice SPEC/plan ↔ code review. **File:** `.cursor/agents/slice-spec-reviewer.md`; prompt: `.cursor/agents/slice-subagents/slice-spec-reviewer-prompt.md`. |
| `slice-quality-reviewer` | agent | Intra-slice quality + evidence review. **File:** `.cursor/agents/slice-quality-reviewer.md`; prompt: `.cursor/agents/slice-subagents/slice-quality-reviewer-prompt.md`. |
| `skill-writer` | agent | `SKILL.md` authoring/refinement: consolidates TDD-for-skills (writing-skills), progressive structure (write-a-skill), and `anthropic-best-practices.md` **from** `.cursor/reference/skill-authoring/`. **File:** `.cursor/agents/skill-writer.md`. |

**Mapping `developer` / `reviewer` / solo mode:** see [Solo mode](#solo-mode-default-for-this-repo) and [Conventions / Review](#review). **In this repo:** files under `.cursor/agents/` (no `workflow-pack/`). **Consumption in another clone:** [workflow-pack package and symlink](#workflow-pack-package-and-symlink-in-consumer-projects).

### Rules `.mdc`

Cross-cutting norms materialized as Cursor **rules** (`.mdc`) — parallel to skills/agents **Catalog**: skills describe flows with exit criterion; rules inject **always-on** or **path-scoped** policy without replacing Catalog workflow slugs. **In this repository** they live under **`.cursor/rules/*.mdc`** (see table below).

**Canonical frontmatter (`.mdc`):**

```yaml
---
description: <text ≤ 1024 chars — rule purpose>
alwaysApply: true | false
globs: [<optional — glob patterns when rule is path-scoped>]
---

```

**Categories:**

1. **Discipline-enforcing always-on** — constant behavioral pressure in workspace (e.g. evidence before completion; Karpathy-style guidelines).
2. **Repo convention scoped by glob** — norm applied only where `globs` match (e.g. `plan-*.md`, `spec-*.md` at repo root or `.cursor/rheyder-method*.md`).
3. **Override default agent behavior** — redefines default expectation (e.g. forbid unsupervised commit/push).
4. **Persistent context** — working mode or cross-cutting policy that is not a skill with its own binary criterion (e.g. solo mode — accumulated roles, HITL).

**When to promote content → canonical `.mdc` rule**

Rule for **new** norm **not** already covered by v1.0 implementation program (`plan-*`). Create `.mdc` when **all**:

- (a) text is **cross-cutting norm** (applies beyond one skill-exit flow);
- (b) needs to stay **loaded by default** (`alwaysApply: true`) or **repeated stably** via `globs`;
- (c) keeping it only in prompts or scattered bullets would cause **drift** vs method.

Before that: section in this document, template in `plan-*`, or **self-contained** skill that already embeds necessary norm excerpt — not a separate rule.

**Exception:** rules foreseen in **`plan-*` for v1.0 implementation** enter via roadmap (including **deferred** candidates — do not materialize until plan-defined trigger).

**Active `.mdc` rules** (program scope; dedicated slugs outside Catalog as skill/agent):


| Rule                                 | Category                         | Frontmatter         | Canonical source in method                                                   | Path in this repo                                                                                                                                                              |
| ------------------------------------ | --------------------------------- | ------------------- | -------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `karpathy-guidelines.mdc`            | discipline-enforcing always-on    | `alwaysApply: true` | [Appendix B — Methodological bases](#appendix-b--methodological-bases)       | `.cursor/rules/karpathy-guidelines.mdc`                                                                                                                                 |
| `verification-before-completion.mdc` | discipline-enforcing always-on    | `alwaysApply: true` | [Conventions / Review](#review) (rubric / principle) + Catalog entry | `.cursor/rules/verification-before-completion.mdc`                                                                                              |
| `language.mdc`                       | persistent context / always-on  | `alwaysApply: true` | Language convention (pt-BR vs `en_US`) for project                                              | `.cursor/rules/language.mdc` |
| `base-creation.mdc`                  | persistent context / always-on   | `alwaysApply: true` | [Self-contained Cursor materialization](#self-contained-cursor-materialization) | `.cursor/rules/base-creation.mdc` |
| `modo-solo.mdc`                      | convention / persistent context  | `alwaysApply: true` | [Solo mode](#solo-mode-default-for-this-repo)            | `.cursor/rules/modo-solo.mdc`                                                                                                                   |
| `commit-push-hitl.mdc`               | override default behavior | `alwaysApply: true` | [Conventions / Commit and delivery](#commit-and-delivery-mandatory-hitl)          | `.cursor/rules/commit-push-hitl.mdc`                                                                                                            |


**`caveman`:** **does not** become `.mdc` — skill at **`.cursor/skills/caveman/SKILL.md`**; concision stays loadable, not always-on.

**Deferred for reactive v1.1** (promote only if pilot records real failure — see `plan-*`):


| Candidate rule                             | Why defer                                                               | Promotion trigger                                           |
| ------------------------------------------ | ----------------------------------------------------------------------- | ------------------------------------------------------------- |
| `interface-design-slices.mdc`              | Norm already referenceable from `plan-writing` skill; rule would duplicate long text | Real failure where code written without `plan-*` loaded |
| `artefatos.mdc` (`globs: ["**/plan-*.md", "**/spec-*.md", ".cursor/rheyder-method*.md"]`) | Low volume, convention fits method templates                          | 3+ artifacts diverging from standard YAML header              |


**Do not become rule (recorded decision):**

- `branches-worktrees.mdc` — covered by `git-worktrees` skill (level 5); short `<track>/<slug>` branch convention may live as bullet in `modo-solo.mdc` rule.

### Brainstorming — profile by triage (single skill)

Canonical slug: `brainstorming`. **Profile** follows triage outcome (not informal choice). **hotfix**, **express**, **express-assisted**, and **full** modes are internal profiles documented in `.cursor/skills/brainstorming/SKILL.md`, not separate Catalog slugs.


| Triage outcome          | Profile               | Notes                                                                                                                 |
| ----------------------- | --------------------- | --------------------------------------------------------------------------------------------------------------------- |
| **Hotfix**              | **hotfix**            | Minimum bullets: reproducible symptom or equivalent evidence, minimal fix scope, **do-not** for this fix.           |
| **Simple — Express**    | **express**           | Bullets: **what** + **how to validate** (see *Fast validation* in **1 — Simple**); no obligation for `spec-*`. |
| **Simple — Assisted**   | **express-assisted**  | Like **express**, output sufficient to feed `plan-writing` (clear tie to slices or explicit risks).   |
| **Normal**              | **full**              | Output oriented to materialize `spec-*` (PRD-like structure when applicable).                                        |
| **Complex**             | **full**              | Same **full** in brainstorming; **spec hardening** (NFR, limits, rollback) in following track steps.          |


**Exit criterion (summary):** *hotfix* — reproduction/evidence + minimal scope + do-not; *express* — change + validation; *express-assisted* — prior + bridge to plan; *full* — no decision gaps forcing invented scope in `plan-*`.

**Anti-drift:** when using skill, explicit prompt preamble: `Profile: <hotfix \| express \| express-assisted \| full> | Track: <...> | Output: bullets in chat \| spec-* (Normal/Complex).` In *hotfix* and *express*, at most **two levels** (title + bullets); no long PRD sections — prefer **questions** if depth missing. If Simple Express yields **3+ open decisions** or large new contract need → **stop** and reclassify (see **00 — Triage / Wrong-track signals**).

### Typical composition

`task-orchestrator` + `git-worktrees` (when applicable) + **documented** harness (commands in `plan-*`/README — `workflow-harness` skill still pending) →
**per slice:** `slice-developer` + `tdd-cycle` where applicable → **`slice-spec-reviewer`** (with `code-review`) → **`slice-quality-reviewer`** (with `code-review`) → **user summary** (Commit and delivery) → **HITL: commit** →
**final pass** with `code-review` + **`validation-*`** → `task-delivery` → **HITL: push / PR** → (learning step / `knowledge-*` per track — **`knowledge-base`** skill still pending).

**Always-on rules:** `.cursor/rules/*.mdc`. **Concision opt-in (`caveman`):** see [Concision (`caveman`)](#concision-caveman).

### Materialization roadmap

**Merged v1.0 read** — state of this checkout. Levels **0–5** in generic text describe build order; **in this repo** most already lives under `.cursor/` **without** `workflow-pack/` folder. Use table below as **status** (not future promise):

| Level | Slugs | State in repo |
| ----- | ----- | ---------------- |
| **0 — bootstrap (authoring)** | bases **mattpocock/skills** + **obra/superpowers** (optional clone — Appendix B); **writing-skills + write-a-skill** in `.cursor/reference/skill-authoring/` | Always derive text under `.cursor/`; **`SKILL.md` authoring** prefer `skill-authoring/` + `skill-writer` agent |
| **1 — cross-cutting policies** | `caveman`, `verification-before-completion`, `karpathy-guidelines` (+ `language`, `base-creation` **added** in this project) | **Materialized** — skills + rules under `.cursor/` |
| **2 — flow entry** | `brainstorming`, `plan-writing` | **Materialized** |
| **3 — core execution** | `tdd-cycle`, `code-review`, `task-delivery` | **Materialized** |
| **4 — agents** | `task-orchestrator`, `slice-developer`, `slice-spec-reviewer`, `slice-quality-reviewer`, `skill-writer` + prompts in `slice-subagents/` (except `skill-writer`) | **Materialized** (normative `developer`/`reviewer` roles map to first three subagents) |
| **5 — complementary** | `git-worktrees`, `systematic-debugging`, `improve-codebase-architecture` | **Materialized** |
| *(expanded program pending)* | `technical-discovery`, `security-review`, `workflow-harness`, `knowledge-base`, `chat-handoff-bullets` | **No** `.cursor/skills/<slug>/` — follow method with bullets in `spec-*`/`plan-*` or materialize later |


**“Minimum viable skill” criterion (v1.0.0):**

- (a) standard YAML header filled;
- (b) explicit **trigger** (when to load);
- (c) expected **input** (context / required artifacts);
- (d) numbered **steps** verifiable;
- (e) binary **exit criterion** (not “style”);
- (f) reference to artifact(s) it materializes, if any.

Skill failing (a)-(e) returns to saved prompt until next round.

### Cursor mapping (real paths)

| Slug | Path | How to invoke |
| ---- | ------- | ------------- |
| Always-on rules (6) | `.cursor/rules/*.mdc` | Automatic load per Cursor |
| `brainstorming` … `task-delivery` | `.cursor/skills/<slug>/SKILL.md` | Mention by skill / track stage |
| `task-orchestrator` | `.cursor/agents/task-orchestrator.md` | Subagent / dedicated chat |
| `slice-developer` | `.cursor/agents/slice-developer.md` + `slice-subagents/slice-developer-prompt.md` | Implementation step |
| `slice-spec-reviewer` | `.cursor/agents/slice-spec-reviewer.md` + `slice-spec-reviewer-prompt.md` | Intra-slice gate 1 |
| `slice-quality-reviewer` | `.cursor/agents/slice-quality-reviewer.md` + `slice-quality-reviewer-prompt.md` | Intra-slice gate 2 |
| `skill-writer` | `.cursor/agents/skill-writer.md` | Skill authoring or review (on-demand) |
| References | `.cursor/reference/workflow-conventions.md`, `tdd-public-api-tests.md`, **`skill-authoring/`** (writing-skills + write-a-skill) | Cited by skills or `skill-writer` |

**IDE reference:** [.cursor/README.md](../README.md).

### workflow-pack package and symlink in consumer projects

**In this methodology monorepo:** workflow lives **directly** in `.cursor/skills/`, `.cursor/agents/`, and `.cursor/rules/` — **no** versioned `.cursor/workflow-pack/` here.

**Consumption in another repository:** you may **copy or symlink** `.cursor/` folder (or subfolders only) from this project; alternatively some teams package a subset in `workflow-pack/` and link to target per OS — same content, different layout. **Local rules** for app stack live in `.mdc` that complement (without contradiction without record) pack rules.

**Subagents:** Cursor exposes definitions in **`/.cursor/agents/*.md`**. This repo follows **orchestrator + three agents per slice** (implementation and double review) listed in [Catalog](#catalog), plus **`skill-writer`** for on-demand skill authoring.

**Hooks:** Cursor hook automations — **after** stable skills/agents/rules — remain outside v1.0 minimum scope of this document.

### When to promote prompt → canonical skill

Rule for **new** prompts **not** already listed slugs in **Catalog** nor covered by **v1.0 implementation program** (`plan-*`). Create `SKILL.md` when **all**:

- (a) you used same prompt in **3+ distinct initiatives**;
- (b) prompt has **clear exit criterion** (not only “writing style”);
- (c) agent loads wrong when you skip manual context.

Before that: saved prompt, not skill.

**Exception:** skills and agents listed on **materialization** roadmap enter via implementation `plan-*`, not only 3-use rule; **pending** slugs in [Catalog](#catalog) depend on new materialization slice.

---

## Appendix A — Inventory vs derivation

Skills enter **direct inventory** (minimal adoption/adaptation) or serve as **base** for canonical skills with Rheyder slug. **mattpocock/skills** and **obra/superpowers** are **upstream packages** for authoring (see [Appendix B](#appendix-b--methodological-bases)); merge content into `.cursor/` artifacts per [Self-contained Cursor materialization](#self-contained-cursor-materialization). **writing-skills and write-a-skill packages** have **mirrored copy** in **`.cursor/reference/skill-authoring/`** — not mandatory agent read in production except when **authoring** skills via `skill-writer`.

### Direct adoption


| Rheyder slug                     | Upstream base (paths relative inside package)                                                         | Note                                                                 |
| -------------------------------- | -------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------- |
| `improve-codebase-architecture`  | mattpocock/skills — `skills/engineering/improve-codebase-architecture/`                                | Implements interface-design rule.                                 |
| `caveman`                        | mattpocock/skills — `skills/productivity/caveman/`                                                     | Concision policy.                                                |
| `verification-before-completion` | obra/superpowers — `skills/verification-before-completion/`                                            | Meta principle: no completion claim without fresh evidence. |


### Derivation (consolidation from bases)


| Rheyder slug           | Bases                                                                                                                                                                                                                                      |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `brainstorming`        | mattpocock `grill-me` + `grill-with-docs` + `to-prd` + superpowers `brainstorming`                                                                                                                                                         |
| `plan-writing`         | superpowers `writing-plans` + mattpocock `to-issues`                                                                                                                                                                                       |
| `tdd-cycle`            | mattpocock `tdd` + superpowers `test-driven-development`                                                                                                                                                                                   |
| `git-worktrees`        | superpowers `using-git-worktrees`                                                                                                                                                                                                          |
| `code-review`          | superpowers `requesting-code-review` + `receiving-code-review`                                                                                                                                                                             |
| `task-delivery`        | superpowers `finishing-a-development-branch`                                                                                                                                                                                               |
| `karpathy-guidelines`  | public Karpathy notes on common LLM coding pitfalls (see Appendix B); consolidated into canonical text in this method’s `SKILL.md`                                                                                        |
| `systematic-debugging` | superpowers `systematic-debugging` + mattpocock `diagnose` (enrichment: feedback loop as central skill + technique list to build loop + `[DEBUG-<id>]` convention for cleanup + perf branch for performance regressions) |


**On `brainstorming` with 4 bases:** when materializing single `SKILL.md`, absorb only what is distinctive per source:

- `grill-me` (mattpocock) — *relentless* interview, *one question at a time*, recommendation per question; in Rheyder method `brainstorming` profile is **fixed by 00 — Triage** (see **Skills / Brainstorming — profile by triage**).
- `grill-with-docs` (mattpocock) — *lazy* glossary maintenance (`CONTEXT.md`) and ADRs *sparingly* (3 criteria: hard to reverse, surprising without context, real trade-off);
- `to-prd` (mattpocock) — PRD-shaped output structure (Problem Statement, Solution, User Stories, Implementation Decisions, Out of Scope) when output is `spec-*`;
- `brainstorming` (superpowers) — completion contract and stop criterion.

If materialized slug grows too large, split: `to-prd` becomes base for separate requirements synthesis skill and `brainstorming` keeps grilling only.

**On `code-review` (`requesting-code-review` + `receiving-code-review`):** in this repo model, context isolation becomes **subagents** `slice-spec-reviewer` and `slice-quality-reviewer` + `code-review` skill for rubric; in solo mode they remain **distinct passes** over time, not people.

### SKILL.md authoring (bases without Rheyder slug)

This method **does not** define invocable Cursor slug **`writing-skills`**. When creating or refining `SKILL.md` artifacts under `.cursor/skills/`, use (authoring):


| Tool | Path this checkout |
| ---------- | ------------- |
| superpowers (writing-skills) | `.cursor/reference/skill-authoring/writing-skills/` — `SKILL.md`, `testing-skills-with-subagents.md`, `anthropic-best-practices.md`, etc. |
| mattpocock (write-a-skill) | `.cursor/reference/skill-authoring/write-a-skill/SKILL.md` |
| Consolidator agent | `.cursor/agents/skill-writer.md` — reads both packages above and applies § [Templates / `SKILL.md` — versioning](#skillmd--versioning) + § Self-contained + **minimum viable skill** criterion in roadmap |
| upstream clones (optional) | Separate clones of **mattpocock/skills** and **obra/superpowers** (URLs Appendix B) only for upstream diff — **not** execution dependency this checkout |

**Note:** update `.cursor/reference/skill-authoring/` when integrating new versions **obra/superpowers** / **mattpocock/skills** (explicit maintenance slice if needed).


### Canonical agents

[Catalog](#catalog) agents materialize orchestration, implementation, track review, and **skill authoring** (`skill-writer`); solo and role mapping under [Solo mode](#solo-mode-default-for-this-repo). Slice parallelism follows superpowers `dispatching-parallel-agents` logic when documented in `plan-*`.

---

## Appendix B — Methodological bases

**Git repositories for bases** (throughout doc, **mattpocock/skills** and **obra/superpowers** refer to these projects):

- **mattpocock/skills** — [https://github.com/mattpocock/skills](https://github.com/mattpocock/skills)
- **obra/superpowers** — [https://github.com/obra/superpowers](https://github.com/obra/superpowers)

**Spot reference:**

- **Karpathy — LLM coding pitfalls (reference)** — [https://x.com/karpathy/status/2015883857489522876](https://x.com/karpathy/status/2015883857489522876) — conceptual base for `karpathy-guidelines`
