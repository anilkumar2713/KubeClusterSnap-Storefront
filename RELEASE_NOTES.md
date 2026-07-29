# KubeClusterSnap V2.3.0 — Volume Snapshotting & Container State Cloning Release Notes

**Release Date:** July 27, 2026  
**Version:** `v2.3.0`  
**Severity:** Feature & Major Capability Release

---

## Executive Summary

KubeClusterSnap `v2.3.0` introduces **Database & Container Volume State Cloning ("Snap" Feature)** and **Interactive In-Browser TTY Shell Terminals**. Developers can now capture an instant, byte-exact compressed volume snapshot of any running container data directory (`/data`, `/var/lib/mysql`, etc.) and clone it into a brand-new, isolated preview environment pre-hydrated with your state.

---

## Key Highlights & Feature Improvements

### 1. 📸 Volume Snapshotting & State Cloning ("Snap" Feature)
* **API Endpoints:** `POST /api/v1/environments/{name}/snap`, `GET /api/v1/snapshots`, `GET /api/v1/snapshots/download/{snap_id}`
* **Capabilities:** 
  - Captures live container data paths using streaming base64-encoded tarballs over WebSocket exec.
  - Automatically injects a `snap-restore-init` `initContainer` (`alpine:latest`) and `emptyDir` volume into newly launched workloads when `clone_from_snap_id` is supplied.
  - Restores database state (Redis, Postgres, MySQL) prior to main container startup.
* **UI Controls:** Added **📸 Snap** button in the Control Plane action grid and a **State Snapshot Cloning** dropdown in the Launch Workload modal.

### 2. In-Browser Interactive TTY Shell (`kubectl exec` over WebSockets)
* **WebSocket Endpoint:** `/ws/environments/{name}/pods/{pod_name}/shell`
* **Technology:** Uses `@xterm/xterm` with `@xterm/addon-fit` for rich ANSI color rendering, keyboard cursor navigation, and terminal auto-resizing.
* **Capabilities:** Enables live bidirectional container command execution (`ls`, `cat`, `ps aux`, `top`, `curl`, environment inspection).

### 3. Security & RBAC Hardening
* **ClusterRole Updates:** Explicit rules added for `pods/exec` and `pods/log` to support namespace-scoped execution without requiring full cluster-admin privileges.

---

## Version History

### Version v2.2.0
* Interactive In-Browser Container Terminal (`kubectl exec` over WebSockets).
* Multi-namespace Helm flexibility (`.Release.Namespace`).

### Version v2.1.0
* Control Plane API Key Authentication Middleware.
* RFC 1123 Custom Namespace Input Sanitization.
* Service Replacement NodePort Preservation.

---

## Upgrade Instructions

### Upgrading via Helm

```bash
# Upgrade installation to v2.3.0
helm upgrade --install kubeclustersnap ./charts/kubeclustersnap -n testing
```

---

## License & Intellectual Property

**Copyright (c) 2026 Anil Gottapu. All Rights Reserved. Proprietary & Confidential.**
