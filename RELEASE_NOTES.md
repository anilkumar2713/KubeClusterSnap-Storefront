# KubeClusterSnap V2.4.0 — Multi-Kubeconfig & Custom Cluster Manager Release Notes

**Release Date:** July 29, 2026  
**Version:** `v2.4.0`  
**Severity:** Feature & Major Capability Release

---

## Executive Summary

KubeClusterSnap `v2.4.0` introduces **Multi-Kubeconfig & Custom Cluster Connection Manager**. Developers and DevOps teams can now connect KubeClusterSnap to any custom Kubernetes cluster (remote k3s, AWS EKS, Azure AKS, GCP GKE, Kind, Minikube) by uploading or pasting custom Kubeconfig YAML files directly from the UI or REST API. Switch your active dashboard context with 1-click from the top Navbar dropdown.

---

## Key Highlights & Feature Improvements

### 1. 🔌 Multi-Kubeconfig & Custom Cluster Manager
* **API Endpoints:** `GET /api/v1/clusters`, `POST /api/v1/clusters`, `POST /api/v1/clusters/switch/{cluster_id}`, `DELETE /api/v1/clusters/{cluster_id}`
* **Capabilities:** 
  - Register custom cluster connections via `.kube/config` file upload or raw YAML text.
  - Built-in connection tester verifies API server reachability and auth before saving.
  - Dynamic `ApiClient` construction in Kubernetes Python SDK driver for active cluster context.
* **UI Controls:** Added **Cluster Switcher Dropdown** in the Navbar showing active cluster status indicator, list of connected clusters, and trigger for `AddClusterModal`.

### 2. 📸 Volume Snapshotting & State Cloning ("Snap" Feature)
* **API Endpoints:** `POST /api/v1/environments/{name}/snap`, `GET /api/v1/snapshots`, `GET /api/v1/snapshots/download/{snap_id}`
* **Capabilities:** Restores database state (Redis, Postgres, MySQL) into isolated preview environments using `initContainer` (`alpine:latest`) and `emptyDir` volume mounts.

### 3. In-Browser Interactive TTY Shell (`kubectl exec` over WebSockets)
* **WebSocket Endpoint:** `/ws/environments/{name}/pods/{pod_name}/shell`
* **Technology:** Uses `@xterm/xterm` with `@xterm/addon-fit` for rich ANSI color rendering and terminal auto-resizing.

---

## Version History

### Version v2.3.0
* Volume Snapshotting & Container State Cloning ("Snap" Feature).

### Version v2.2.0
* Interactive In-Browser Container Terminal (`kubectl exec` over WebSockets).

### Version v2.1.0
* Control Plane API Key Authentication Middleware & Namespace Sanitization.

---

## Upgrade Instructions

### Upgrading via Helm

```bash
# Upgrade installation to v2.4.0
helm upgrade --install kubeclustersnap ./charts/kubeclustersnap -n testing
```

---

## License & Intellectual Property

**Copyright (c) 2026 Anil Gottapu. All Rights Reserved. Proprietary & Confidential.**
