# Context Policy

## Objective

Reduce unnecessary context and token use while keeping decisions, risks, and evidence traceable.

## Rules

- Load **only the skill for the current phase** (plus [CONVENTIONS.md](CONVENTIONS.md) when implementing, verifying, or reviewing).
- Do **not** load all review checklists by default — `code-review` selects axes by risk.
- Prefer **structured summaries** over long transcripts.
- Preserve **decisions, risks, and evidence** — drop narrative filler.
- Do **not** repeat the full spec in every phase; link or cite bullets.
- Pass **short artifacts** between phases (see below).
- Keep **specs and plans short** by default; expand only when risk, ambiguity, or the user requires it.

Aligns with [CONVENTIONS.md § Concise communication](CONVENTIONS.md#concise-communication).

## Adapters and context

Load adapters only when:

- they are relevant to the current phase;
- they are configured in the project;
- the benefit justifies the context cost.

See [ADAPTERS.md](ADAPTERS.md). Checklists are not loaded by default.

## Minimal artifacts between phases

### Spec

- problem;
- in / out of scope;
- acceptance criteria;
- known risks.

### Plan

- tasks or slices;
- order;
- verification method per slice.

### Build

- files touched;
- technical decision (one line each);
- tests added or updated.

### Verify

- commands or methods run;
- pass/fail with evidence (not claims alone).

### Review

- findings;
- severity;
- blockers.

### Ship

- summary;
- risks;
- next step.

## Toolbox

Toolbox skills are **not** on the default path. Load them only when the problem needs optional bootstrap, diagnosis, teaching, handoff, architecture exploration, or authoring new skills. See [README.md](README.md).
