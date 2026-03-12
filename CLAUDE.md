# kubeadjust-helm — CLAUDE.md

Context file for Claude Code.

---

## What this repo is

Standalone Helm chart for [KubeAdjust](https://github.com/Thomas6013/kubeadjust) — a read-only Kubernetes resource dashboard. Extracted from the main repo in v0.19.0 to allow independent versioning.

**The chart lives at the repo root** (not in a `charts/` subdirectory). Install directly:

```bash
helm install kubeadjust . --namespace kubeadjust --create-namespace
```

---

## Repository layout

```
Chart.yaml           # Chart version + appVersion — single source of truth for chart versioning
values.yaml          # All configurable values with inline comments
.helmignore          # Files excluded from helm package

templates/
  _helpers.tpl       # Named templates (fullname, labels, SA name, OIDC secret names)
  deployment.yaml    # Backend + frontend Deployments
  rbac.yaml          # ClusterRole (get/list/watch only) + ClusterRoleBinding
  service.yaml       # Backend + frontend Services (ClusterIP)
  serviceaccount.yaml
  ingress.yaml       # Optional Ingress (ingress.enabled=true)
  networkpolicy.yaml # Optional NetworkPolicy (networkPolicy.enabled=true)
  oidc-secret.yaml   # Generated Secret for OIDC credentials (when oidc.enabled=true)
  NOTES.txt          # Post-install instructions

deploy/              # Example manifests to apply manually (not part of helm package)
  oidc-secret.yaml         # Secret template for OIDC clientSecret + sessionSecret
  oidc-tokens-secret.yaml  # Secret template for SA tokens (OIDC multi-cluster)
  viewer-serviceaccount.yaml  # Standalone SA + ClusterRole for remote cluster setup

.github/
  workflows/
    lint.yml         # helm lint --strict + helm template smoke tests (runs on every push/PR)
  ISSUE_TEMPLATE/
    bug_report.md
    feature_request.md
```

---

## Key rules

- **Read-only RBAC always**: ClusterRole uses only `get`, `list`, `watch`. Never add `patch`, `update`, `delete`, `create`.
- **Chart version** is in `Chart.yaml` → `version`. Bump on every change (semver). `appVersion` tracks the KubeAdjust application version.
- **No release workflow yet**: distribution is via `git clone` + `helm install .`. See ROADMAP.md for the planned gh-pages Helm repo.
- **Inline value comments**: every value in `values.yaml` must have a comment explaining what it does.

---

## CI (lint.yml)

Runs on every push and PR to `main`:

1. `helm repo add metrics-server ...` — registers the metrics-server repo (needed to resolve the sub-chart dependency)
2. `helm dependency update .` — downloads `metrics-server` chart into `charts/` (local, not committed)
3. `helm lint . --strict` — validates the chart
4. Five `helm template` smoke tests — ensures templates render without error for: default, OIDC, multi-cluster, Ingress+TLS, Prometheus+NetworkPolicy

---

## Versioning

Chart version (`Chart.yaml → version`) is independent of the KubeAdjust app version (`appVersion`). Bump `version` on any chart change, even if `appVersion` is unchanged.

---

## Links

- Main app repo: https://github.com/Thomas6013/kubeadjust
- CHANGELOG: [CHANGELOG.md](CHANGELOG.md)
- Roadmap: [ROADMAP.md](ROADMAP.md)
- Security policy: [SECURITY.md](SECURITY.md)
