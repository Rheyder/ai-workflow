# Triage Labels

Canonical triage roles for local markdown planning. Copied to `docs/ai-workflow/triage-labels.md` at optional bootstrap.

| Canonical role | Default status string | Meaning |
|----------------|----------------------|---------|
| `needs-triage` | `needs-triage` | Needs evaluation |
| `needs-info` | `needs-info` | Waiting on reporter |
| `ready-for-execution` | `ready-for-execution` | Fully specified, ready for AFK execution |
| `ready-for-review` | `ready-for-review` | Requires human decision or implementation |
| `wontfix` | `wontfix` | Will not be actioned |

When a skill mentions a role (e.g. "apply the AFK-ready triage label"), use the corresponding `Status:` string from this table.

Edit the right-hand column to match your repository vocabulary.
