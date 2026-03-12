# Changelog

All notable changes to the KubeAdjust Helm chart are documented here.

---

## [0.19.1] - 2026-03-12

### Fixed
- Pin Helm to `v4.1.1` in CI (`azure/setup-helm@v4` now installs Helm 4.x by default since Nov 2025)
- Chart is compatible with both Helm 3.x and Helm 4.x — no chart changes required

---

## [0.19.0] - 2026-03-11

### Added
- Initial release of the chart as a standalone repository (extracted from [kubeadjust](https://github.com/Thomas6013/kubeadjust))
- Full Helm chart for KubeAdjust v0.19.0 — backend + frontend deployments, RBAC, ServiceAccount, Ingress, NetworkPolicy, OIDC secrets
- `deploy/` directory with example Secret manifests (`oidc-secret.yaml`, `oidc-tokens-secret.yaml`, `viewer-serviceaccount.yaml`)
- GitHub Actions lint workflow — `helm lint --strict` + 5 template smoke tests
- `CONTRIBUTING.md`, `SECURITY.md`, `ROADMAP.md`, `CHANGELOG.md`
- Issue templates (bug report, feature request)

### Chart features (carried over from kubeadjust repo)
- Multi-cluster support via `backend.clusters` map
- OIDC/SSO authentication (`oidc.*` values) with group-based access control
- Optional metrics-server sub-chart (`metrics-server.enabled`)
- Optional Prometheus integration (`prometheus.enabled`)
- Optional NetworkPolicy (`networkPolicy.enabled`)
- Read-only ClusterRole (get/list/watch only — no write permissions ever)
- Hardened pod security contexts (`readOnlyRootFilesystem`, `runAsNonRoot`, `allowPrivilegeEscalation: false`)
