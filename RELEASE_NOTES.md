# KubeClusterSnap V2.5.0 — Docker Compose Support & Dual Distribution Release Notes

**Release Date:** July 29, 2026  
**Version:** `v2.5.0`  
**Severity:** Feature & Distribution Release

---

## Executive Summary

KubeClusterSnap `v2.5.0` introduces **Standalone Docker Compose Deployment Support (`docker-compose.yml`)**. Customers and developers can now deploy the full KubeClusterSnap control plane in seconds using `docker compose up -d` without needing Helm or installing control plane manifests directly into Kubernetes.

---

## Key Highlights & Feature Improvements

### 1. 🐳 Standalone Docker Compose Support (`docker-compose.yml`)
* **Commands:** `docker compose up -d`
* **Ports:** Dashboard UI at `http://localhost:30300`, REST API at `http://localhost:30800`
* **Features:**
  - Automatically mounts host `${HOME}/.kube/config` into backend container for out-of-the-box cluster control.
  - Named Docker volumes (`kubeclustersnap-snapshots` and `kubeclustersnap-kubeconfigs`) persist volume snapshots and custom cluster connections across container restarts.

### 2. 🔌 Multi-Kubeconfig & Custom Cluster Manager
* **API Endpoints:** `GET /api/v1/clusters`, `POST /api/v1/clusters`, `POST /api/v1/clusters/switch/{cluster_id}`, `DELETE /api/v1/clusters/{cluster_id}`
* **Capabilities:** Register custom cluster connections via `.kube/config` file upload or raw YAML text. Switch active cluster context with 1-click from the Navbar dropdown.

### 3. 📸 Volume Snapshotting & State Cloning ("Snap" Feature)
* Restores database state (Redis, Postgres, MySQL) into isolated preview environments using `initContainer` (`alpine:latest`) and `emptyDir` volume mounts.

---

## Version History

### Version v2.4.0
* Multi-Kubeconfig & Custom Cluster Connection Manager.

### Version v2.3.0
* Volume Snapshotting & Container State Cloning ("Snap" Feature).

### Version v2.2.0
* Interactive In-Browser Container Terminal (`kubectl exec` over WebSockets).

---

## Quick Start & Deployment Options

### Option A: Standalone Docker Compose (`docker compose up -d`)

```bash
curl -sSL https://raw.githubusercontent.com/anilkumar2713/KubeClusterSnap-Storefront/master/docker-compose.yml -o docker-compose.yml
docker compose up -d
```

### Option B: Native Kubernetes Installation via Helm

```bash
helm upgrade --install kubeclustersnap ./charts/kubeclustersnap --create-namespace -n kubeclustersnap
```

---

## License & Intellectual Property

**Copyright (c) 2026 Anil Gottapu. All Rights Reserved. Proprietary & Confidential.**
