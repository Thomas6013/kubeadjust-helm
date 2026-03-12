# Contributing to kubeadjust-helm

Thank you for your interest! This repository contains **only the Helm chart** for KubeAdjust.

## Scope

| What | Where |
|---|---|
| Helm chart bugs / new values | **Here** — open an issue or PR |
| Dashboard features, backend, frontend | [kubeadjust](https://github.com/Thomas6013/kubeadjust) |
| Security vulnerabilities | See [SECURITY.md](SECURITY.md) |

## Making a change

1. Fork the repository
2. Create a feature branch: `git checkout -b feat/my-change`
3. Make your changes to `Chart.yaml`, `values.yaml`, or `templates/`
4. Run the lint checks locally:

   ```bash
   helm dependency update .
   helm lint . --strict
   helm template kubeadjust . > /dev/null
   ```

5. **Bump `version`** in `Chart.yaml` (semver — `patch` for fixes, `minor` for new values, `major` for breaking changes)
6. Document any new value in `values.yaml` with an inline comment
7. Open a pull request — describe what changed and why

## Releasing

Releases are **fully automated** via [helm/chart-releaser-action](https://github.com/helm/chart-releaser-action).

Every merge to `main` that increments `version` in `Chart.yaml` will:
1. Package the chart
2. Create a GitHub Release with the `.tgz` artifact
3. Update the `gh-pages` branch with the new `index.yaml`

The Helm repository (`helm repo add kubeadjust https://thomas6013.github.io/kubeadjust-helm`) is then automatically up to date.

## Values documentation

All values must have an inline comment in `values.yaml` explaining what they do and any constraints (e.g. `# must be ≥32 chars`).

## Code of conduct

Be respectful. This is a small open-source project maintained in spare time.
