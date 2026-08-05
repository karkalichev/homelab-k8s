# homelab-k8s

GitOps manifests for homelab applications, synced by ArgoCD via the App-of-Apps pattern.

## Structure

```
apps/                          # ArgoCD Application definitions (watched by root-apps)
├── trakiabillboard.yaml
manifests/
├── trakiabillboard/           # Raw Kubernetes manifests per project
│   ├── nginx-deployment.yaml
│   ├── postgres.yaml
│   └── redis.yaml
```

- **`apps/`** — one ArgoCD `Application` per project; each entry appears as a separate card in the ArgoCD UI.
- **`manifests/<project>/`** — Deployments, Services, PVCs, etc. for that project.

Database credentials are managed outside this repo (Terraform + `TF_VAR_*` in the bootstrap layer). For production, consider Sealed Secrets or External Secrets Operator.
