# Roadmap

## Current state

The chart is distributed directly from this repository. Install via:

```bash
git clone https://github.com/Thomas6013/kubeadjust-helm.git
helm install kubeadjust ./kubeadjust-helm --namespace kubeadjust --create-namespace
```

## Planned

### Helm repository (gh-pages)

Enable `helm repo add` installation without cloning:

```bash
helm repo add kubeadjust https://thomas6013.github.io/kubeadjust-helm
helm install kubeadjust kubeadjust/kubeadjust
```

**What this requires:**
1. Enable GitHub Pages on this repo (Settings → Pages → source: `gh-pages` branch)
2. Add `.github/workflows/release.yml` using [helm/chart-releaser-action](https://github.com/helm/chart-releaser-action)
3. Every merge to `main` that bumps `version` in `Chart.yaml` will automatically package and publish the chart

### Chart testing (ct lint)

Add [chart-testing](https://github.com/helm/chart-testing) to the CI pipeline for stricter validation including schema checking and best-practice enforcement.

### OCI registry

Publish the chart to an OCI registry (`ghcr.io`) as an alternative to the Helm HTTP repository:

```bash
helm install kubeadjust oci://ghcr.io/thomas6013/kubeadjust-helm/kubeadjust --version 0.19.0
```

### Artifact Hub listing

List the chart on [Artifact Hub](https://artifacthub.io/) for discoverability.

---

## Non-goals

- The chart will never add write permissions to the ClusterRole
- No bundled ingress controller or cert-manager — users bring their own
