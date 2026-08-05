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

## trakiabillboard image

The nginx pod uses a **locally built** image (same tag as docker-compose). Docker Desktop shares images with its Kubernetes cluster:

```bash
cd /path/to/trakiabillboard.com
docker build -t trakiabillboard-php-nginx:7.4 .
```

The deployment sets `imagePullPolicy: Never` so the cluster uses that local image instead of pulling from a registry. DB credentials come from the Terraform-managed `trakiabillboard-db-secret`; `db.php` and `params-local.php` are mounted from a ConfigMap.
