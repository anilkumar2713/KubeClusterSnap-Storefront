# KubeClusterSnap V2.2.0 — Interactive Web-Based Container Terminal Release Notes

**Release Date:** July 26, 2026  
**Version:** `v2.2.0`  
**Severity:** Feature & Enhancement Release

---

## Executive Summary

KubeClusterSnap `v2.2.0` introduces **Web-Based Interactive Container Terminals (`kubectl exec` over WebSockets)**. Developers can now open an interactive TTY shell directly inside running pod containers from the web UI, executing commands (`bash`/`sh`) without requiring local `kubectl exec` CLI access.

---

## Key Highlights & Feature Improvements

### 1. In-Browser Interactive TTY Shell (`kubectl exec` over WebSockets)
* **Component:** `backend/app/routers/terminal.py` & `frontend/src/components/PodTerminalModal.tsx`
* **WebSocket Endpoint:** `/ws/environments/{name}/pods/{pod_name}/shell`
* **Technology:** Uses `@xterm/xterm` with `@xterm/addon-fit` for rich ANSI color rendering, keyboard cursor navigation, and terminal auto-resizing.
* **Capabilities:** Enables live bidirectional container command execution (`ls`, `cat`, `ps aux`, `top`, `curl`, environment inspection).

---

## Version History & Fixes

### Version v2.1.0
* Control Plane API Key Authentication Middleware (`KL-2026-001`).
* RFC 1123 Custom Namespace Input Sanitization (`KL-2026-002`).
* Service Replacement NodePort Preservation (`KL-2026-003`).

---

## Upgrade Instructions

### Upgrading via Helm

```bash
# Upgrade installation to v2.2.0
helm upgrade kubeclustersnap ./charts/kubeclustersnap -n testing
```

---

## License & Intellectual Property

**Copyright (c) 2026 Anil Gottapu. All Rights Reserved. Proprietary & Confidential.**
