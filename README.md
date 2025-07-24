# ArgoCD Multi-Environment App Deployment

This repository manages the GitOps deployment of two sample applications—`game-2048` and `simpleWeb`—across **dev**, **staging**, and **prod** environments using **Argo CD** and **Helm**.

## 📁 Project Structure

```
.
├── LICENSE
├── rootApp.yaml                  # Root ArgoCD Application for managing children apps
├── argo-apps/                    # ArgoCD Application manifests and Helm values per env
│   ├── applications/             # ArgoCD Application CRs per app/env
│   ├── game-2048-{env}/values.yaml
│   └── simpleWeb-{env}/values.yaml
└── charts/                       # Helm charts for each app
    ├── game-2048/
    │   ├── templates/            # K8s manifests for deployment, service, ingress
    │   └── values.yaml           # Default Helm values
    └── simpleWeb/
        ├── templates/
        └── values.yaml
```

## 🚀 Applications

### Deployed Apps

* `game-2048`
* `simpleWeb`

### Environments

* `dev`
* `staging`
* `prod`

Each application is defined and configured per environment with its own `values.yaml`.

## 🔁 GitOps Workflow

1. **rootApp.yaml**: A parent ArgoCD `Application` that syncs child apps from `argo-apps/applications/`.
2. **ArgoCD Applications**: One per app/environment (`game-2048-dev.yaml`, etc.)
3. **Helm Charts**: Each app has its own Helm chart under `charts/`.

Changes pushed to this repo (or merged via Pull Request) are picked up automatically by Argo CD and deployed to the correct environment.

## 🧪 Example App Values

Each app/env folder includes a `values.yaml` that controls:

* Image tag
* Replica count
* Resource limits
* Ingress config

Example:

```yaml
image:
  repository: your-docker-registry/simpleweb
  tag: "1.0.0"

replicaCount: 2

ingress:
  enabled: true
  host: simpleweb.dev.example.com
```

## 🛠️ Getting Started

1. **Install Argo CD** on your cluster
2. **Apply root application**:

   ```bash
   kubectl apply -f rootApp.yaml
   ```
3. Argo CD will auto-discover and sync all child apps.

## 🔄 Next Steps or Alternatives

* **Per-Environment Root Apps**: Consider creating a separate `rootApp.yaml` for each environment (e.g., `rootApp-prod.yaml`, `rootApp-dev.yaml`) to ensure clear separation between production and non-production workloads.
* **Scoped Folder Watchers**: Adjust Argo CD’s `path` in each `Application` to point to environment-specific subfolders (e.g., `argo-apps/applications/prod/`) to improve isolation and reduce risk of cross-environment misconfiguration.
