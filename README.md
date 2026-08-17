# gha — shared GitHub Actions workflows for the DiSabatino LLC estate

Private. Callers must be repos owned by `MatthewDiSabatino` (Settings → Actions → Access = "Accessible from repositories owned by the user").

| Workflow | Purpose | Callers |
|---|---|---|
| `python-ci.yml` | ruff (contract pinned in the repo) + tests, optional ai-gm-engine install (`master` or the Dockerfile pin), pip cache, least-priv permissions, timeout | engine · gateway · tabletop-bots · stonetop-bot · ops |
| `build-image-railway.yml` | GHCR build → Trivy → SBOM → cosign (static key, no tlog) → `railway redeploy` → **poll `/health` for `build_rev == sha`** → `deploy/` tag | stonetop-bot · tabletop-bots (gateway keeps its own: ruleset-vendoring gate) |
| `workflow-lint.yml` | actionlint + zizmor over the caller's `.github/` | everyone |

Rules: every `uses:` is SHA-pinned with a `# vX.Y.Z` comment (Dependabot maintains both). Every job has `timeout-minutes` and least-privilege `permissions`. Callers set `concurrency`.

Check names as seen by rulesets: `<caller job id> / test`, `<caller job id> / workflow-lint`, `<caller job id> / build-image`.

Origin: ops CI/CD maturity plan (2026-08-17), Phase 1b.
