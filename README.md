# loanhub-gitops

Declarative Kubernetes manifests for the LoanHub platform — the **single source of truth**
that **ArgoCD** watches and reconciles onto the EKS cluster. CI updates image tags here;
ArgoCD applies the change with a zero-downtime rolling update.

Part of the LoanHub polyrepo:
[`loan_app_backend `](https://github.com/raj-pro/loan_app_backend ) ·
[`loan_app_frontend`](https://github.com/raj-pro/loan_app_frontend) ·
[`loanhub-infra`](https://github.com/raj-pro/loanhub-infra) ·
[`loanhub-gitops`](https://github.com/raj-pro/loanhub-gitops)

## Layout (Kustomize base + overlays)

```
gitops/
├── apps/                  # ArgoCD Application manifests (app-of-apps pattern)
├── base/
│   ├── backend/           # Deployment, Service, HPA, ConfigMap, NetworkPolicy
│   └── frontend/          # Deployment, Service
└── overlays/
    ├── dev/               # dev patches (replicas, resources, image tags, host)
    └── prod/              # prod patches
```

## Conventions (Phase 5/6)

- **Deployments**: resource requests/limits, liveness + readiness probes, `HPA` for the backend.
- **Services**: `ClusterIP`; a single **nginx Ingress** routes `/` → frontend and `/api/*` → backend.
- **Config**: `ConfigMap` for non-secret settings; **Secrets** via External Secrets Operator
  backed by AWS Secrets Manager (DB password) — never committed here.
- **Image tags**: bumped by CI per release (semver), reconciled automatically by ArgoCD.
# Updated
