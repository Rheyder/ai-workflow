# Delivery & Quality Gates

**Review axis only** — PR/release readiness signals in the diff. Do not confuse with:

- **`finishing-a-development-branch` Step 1** — runs branch verification with evidence before push/PR.
- **`slice-verification`** — per-slice evidence after GREEN.

Focus: whether the change is **likely to pass** delivery gates (CI, lint config, build scripts). Do not re-litigate what automation already enforces; note gaps if CI config changed or checks are missing.

## Checks

- [ ] Lint/format config changes consistent with project tooling
- [ ] CI workflow changes valid (triggers, jobs, secrets references, artifact paths)
- [ ] New code covered by existing test/lint jobs in CI (or CI updated to include it)
- [ ] Build scripts and package manifests coherent with the change
- [ ] Breaking pipeline steps called out if the diff touches deploy or release paths
- [ ] Required checks for this repo would plausibly pass (or author noted what was run)
- [ ] No committed generated artifacts that should be build outputs unless repo convention allows

If nothing in the diff touches this dimension: report `N/A — no applicable changes`.
