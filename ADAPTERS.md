# Adapters

Adapters connect AI-Workflow to tools, MCPs, IDEs, agent hosts, issue trackers, or organization-specific standards.

**Adapters are optional.** This personal workflow package does not ship organization-specific adapters (knowledge search, internal doc readers, org Java standards, etc.).

## Rules

- The Core Workflow does not depend on adapters.
- Adapters must not change the main phases.
- Adapters may provide commands, shortcuts, integrations, or local conventions.
- If an adapter is missing, use the neutral behavior documented in the workflow.

## Examples (optional, per repository)

| Adapter type | Purpose |
|--------------|---------|
| Issue tracker publisher | GitHub, GitLab, Jira, Linear, etc. |
| Repository docs reader | Internal or generated docs before broad search |
| Organization standards | Language or security policies beyond repo files |
| IDE / agent host | `.cursor/`, `.github/`, host-specific skill registration |
| Agent host file | `AGENTS.md`, `CLAUDE.md` — optional mirror of `AI_WORKFLOW.md` |

## Neutral defaults

When no adapter is configured:

- Write specs and plans to the **configured planning location**, or `.scratch/<feature>/` as local fallback.
- Read repo standards from `AI_WORKFLOW.md`, `docs/ai-workflow/*`, `CONTRIBUTING.md`, `STYLE.md`, ADRs.
- Review with `code-review` checklists on demand only.
