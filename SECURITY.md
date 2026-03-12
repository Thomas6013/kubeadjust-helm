# Security Policy

## Supported versions

Only the latest chart version receives security fixes.

| Version | Supported |
|---|---|
| latest | ✅ |
| older | ❌ |

## Reporting a vulnerability

Please **do not** open a public GitHub issue for security vulnerabilities.

Use [GitHub private vulnerability reporting](https://github.com/Thomas6013/kubeadjust-helm/security/advisories/new) instead.

We aim to respond within 7 days and publish a fix within 30 days of confirmation.

## Security design

KubeAdjust is **read-only by design** — the Helm chart enforces this at every layer:

### RBAC — strictly read-only

The `ClusterRole` created by this chart uses **only** `get`, `list`, and `watch` verbs.
No `patch`, `update`, `delete`, or `create` permissions are granted — ever.

```yaml
verbs: ["get", "list", "watch"]
```

### Pod security hardening

Both backend and frontend pods enforce:

| Setting | Value |
|---|---|
| `readOnlyRootFilesystem` | `true` |
| `runAsNonRoot` | `true` |
| `allowPrivilegeEscalation` | `false` |
| `capabilities.drop` | `["ALL"]` |

### No data persistence

The backend is stateless — no database, no cache. Bearer tokens are forwarded per-request and never stored or logged server-side.

### NetworkPolicy

An optional `NetworkPolicy` is included (`networkPolicy.enabled=true`) to restrict ingress/egress to only what is necessary.

### Secrets management

OIDC credentials (`clientSecret`, `sessionSecret`) and SA tokens should be stored in Kubernetes `Secret` objects and referenced via `oidc.existingSecret` / `oidc.existingTokenSecret` — not embedded in `values.yaml`.
