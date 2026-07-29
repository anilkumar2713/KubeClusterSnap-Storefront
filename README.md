# KubeClusterSnap — Visual Kubernetes Developer IDP & Real-Time Control Plane

[![Kubernetes](https://img.shields.io/badge/Kubernetes-k3s%20%7C%20EKS%20%7C%20AKS%20%7C%20GKE-blue.svg?logo=kubernetes)](https://k3s.io)
[![Latest Release](https://img.shields.io/badge/Release-v2.4.0-brightgreen.svg)](https://github.com/anilkumar2713/KubeClusterSnap-Storefront/releases/tag/v2.4.0)
[![Edition](https://img.shields.io/badge/Edition-Community%20%26%20Enterprise-009688.svg)](#kubeclustersnap-editions)
[![License](https://img.shields.io/badge/License-Proprietary-red.svg)](#intellectual-property--licensing)

**KubeClusterSnap** is the all-in-one visual Internal Developer Platform (IDP) and real-time control plane built for fast-moving developers, engineering leads, and growing tech companies. It eliminates Kubernetes friction by turning complex container orchestration into a sleek, 1-click self-service dashboard with multi-cluster kubeconfig management, database volume state snapshotting, interactive container shell access, and real-time observability.

---

## 🚀 What's New in Release v2.4.0

* **🔌 Multi-Kubeconfig & Custom Cluster Connection Manager:** Connect KubeClusterSnap to any custom Kubernetes cluster (remote k3s, AWS EKS, Azure AKS, GCP GKE, Kind, Minikube) by uploading or pasting custom Kubeconfig YAML files directly from the UI or REST API.
* **⚡ 1-Click Cluster Context Switcher:** Seamlessly switch your entire dashboard observability & control plane context between connected clusters directly from the top Navbar dropdown.
* **📸 Database & Container State Cloning ("Snap" Feature):** 1-click volume snapshotting of container data paths (`/data`, `/var/lib/mysql`, etc.) and instant cloning into new, isolated preview environments pre-hydrated with your state.
* **Interactive In-Browser Container Terminal (`kubectl exec` over WebSockets):** Open a live TTY shell directly inside running pod containers from the web UI (`xterm.js`).
* **Hardened RBAC & Security Scoping:** Updated ClusterRole rules with explicit `pods/exec` and `pods/log` permissions for namespace-isolated execution.

Read full release details in our [GitHub Release Notes](https://github.com/anilkumar2713/KubeClusterSnap-Storefront/releases/tag/v2.4.0).

---

## What Problem Does KubeClusterSnap Solve?

### 1. Multi-Cluster Fragmentation & Kubeconfig Switching Hassle
* **The Problem:** Developers and DevOps engineers constantly juggle multiple terminal tabs, environment variables, and `kubectl config use-context` commands to manage dev, staging, and production clusters.
* **The Solution:** **Unified Multi-Cluster Manager.** Import custom `.kube/config` files or paste YAML text directly in the UI. Switch active cluster context with 1-click from the Navbar dropdown, automatically updating all telemetry, topology maps, container logs, and self-service catalog deployments.

### 2. Solves Database & Test State Isolation (The "Snap" Engine)
* **The Problem:** Developers spend hours generating mock data or trying to reproduce edge-case production bugs because test container environments start completely empty every time.
* **The Solution:** **1-Click Volume Snapshotting & State Cloning.** Developers can take an instant snapshot of a populated database or application volume (Redis, Postgres, MySQL) and clone it into a brand-new, isolated environment pre-hydrated with the exact same data.

### 3. Eliminates Local Kubernetes Onboarding Friction
* **The Problem:** Developers waste hours writing complex `kubectl` YAML manifests, creating namespaces manually, configuring services, and debugging port forwarding just to run local preview environments.
* **The Solution:** **1-Click Self-Service Catalog.** Developers can spin up isolated, multi-replica container workloads (NGINX, Redis, Node.js, or custom images) in seconds with automated namespace isolation and NodePort routing.

### 4. Eliminates CLI Dependency for Triage & Debugging
* **The Problem:** Debugging a failing container requires context-switching to a terminal window, running `kubectl get pods`, `kubectl exec -it <pod> -- sh`, or `kubectl logs -f`.
* **The Solution:** **In-Browser Terminal & Log Inspector.** Direct web-based container shell access (`>_ Shell`) and stdout/stderr log inspector (`>_ Logs`) directly from the control plane UI without leaving the browser.

### 5. Instant Visual Observability Without Heavy Setup
* **The Problem:** Understanding how microservices communicate or finding out why a pod crashed requires digging through raw text logs or setting up heavy, expensive monitoring stacks.
* **The Solution:** **Built-In Real-Time Control Plane.** Features interactive network topology DAG maps (ReactFlow), PromQL CPU/Memory telemetry graphs (ECharts), live WebSocket cluster event streams, and aggregate KPI gauges out-of-the-box.

---

## KubeClusterSnap Feature Matrix & Editions

| Feature | Community Edition (Free) | Enterprise Edition (Production Teams) |
| :--- | :---: | :---: |
| **Local k3s & Custom Kubeconfig Multi-Cluster Support** | ✅ Included | ✅ Included |
| **1-Click Cluster Context Switcher Dropdown** | ✅ Included | ✅ Included |
| **Standard 1-Click Service Catalog & Custom Container Provisioner** | ✅ Included | ✅ Included |
| **Volume State Snapshotting & State Cloning ("Snap" Feature)** | ✅ Included | ✅ Included |
| **Interactive In-Browser Container TTY Shell (`kubectl exec`)** | ✅ Included | ✅ Included |
| **Real-Time Telemetry & PromQL Metric Graphs** | ✅ Included | ✅ Included |
| **Visual Network Topology DAG Map (ReactFlow)** | ✅ Included | ✅ Included |
| **Live Cluster Event Drawer & Container Log Inspector** | ✅ Included | ✅ Included |
| **Production Multi-Cluster Mesh (AWS EKS / Azure AKS / GCP GKE)** | ❌ | ✅ Included |
| **Single Sign-On (SSO / OIDC) & Audit Logging** | ❌ | ✅ Included |
| **FinOps Auto-Sleep & TTL Garbage Collection**| ❌ | ✅ Included |
| **Custom PromQL Dashboard Builder** | ❌ | ✅ Included |
| **Production Helm Deployment & 24/7 SLA Support** | ❌ | ✅ Included |

---

## REST & WebSocket API Specification

| Method | Endpoint | Description |
| :--- | :--- | :--- |
| `GET` | `/api/v1/clusters` | List all registered cluster connections and active context |
| `POST` | `/api/v1/clusters` | Register a new cluster with Kubeconfig YAML text |
| `POST` | `/api/v1/clusters/switch/{cluster_id}` | Switch active cluster context for all operations |
| `DELETE` | `/api/v1/clusters/{cluster_id}` | Delete custom cluster connection |
| `POST` | `/api/v1/environments/launch` | Provision workload (supports custom specs, env vars, target namespace, and `clone_from_snap_id`) |
| `GET` | `/api/v1/environments` | List all active environments and workload statuses |
| `POST` | `/api/v1/environments/{name}/snap` | Capture volume snapshot tarball archive from running pod |
| `GET` | `/api/v1/snapshots` | List all available volume state snapshots |
| `GET` | `/api/v1/snapshots/download/{snap_id}` | Download volume snapshot archive tarball |
| `PATCH` | `/api/v1/environments/{name}/scale` | Scale deployment replica count (`{"replicas": N}`) |
| `POST` | `/api/v1/environments/{name}/restart` | Trigger zero-downtime rolling restart |
| `GET` | `/api/v1/environments/{name}/pods/{pod_name}/logs` | Fetch container stdout/stderr logs |
| `GET` | `/api/v1/environments/{name}/topology` | Fetch ReactFlow topology DAG nodes and edges |
| `DELETE` | `/api/v1/environments/{name}` | Initiate cascading namespace teardown |
| `WS` | `/ws/environments/{name}/pods/{pod_name}/shell` | Interactive WebSocket TTY container terminal (`kubectl exec`) |
| `WS` | `/ws/events` | Real-time WebSocket Kubernetes cluster event stream |

---

## Community Edition Quick Start & Lifecycle Guide (Helm)

### 1. Installation & Upgrades via Helm (`helm upgrade --install`)

Deploy or upgrade KubeClusterSnap in your Kubernetes cluster using our public Helm chart. The `--install` flag guarantees seamless initial installation AND future upgrades:

```bash
# Install or Upgrade KubeClusterSnap via Helm
helm upgrade --install kubeclustersnap ./charts/kubeclustersnap --create-namespace -n kubeclustersnap
```

Open **[http://localhost:30300](http://localhost:30300)** in your browser to access the visual developer dashboard!

---

### 2. Upgrading for Every New Release

Whenever a new version of KubeClusterSnap is released (e.g. `v2.4.0`), run `helm upgrade --install`:

```bash
# Upgrade your deployment to the latest version
helm upgrade --install kubeclustersnap ./charts/kubeclustersnap -n kubeclustersnap
```

---

## Intellectual Property & Licensing

**Copyright (c) 2026 Anil Gottapu. All Rights Reserved. Proprietary & Confidential.**

KubeClusterSnap software and its distribution artifacts are protected under proprietary copyright laws. Unauthorized copying, reverse engineering, redistribution, or commercial exploitation is strictly prohibited.
