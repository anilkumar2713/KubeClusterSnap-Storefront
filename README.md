# KubeClusterSnap — Visual Kubernetes Developer IDP & Real-Time Control Plane

[![Kubernetes](https://img.shields.io/badge/Kubernetes-k3s%20%7C%20EKS%20%7C%20AKS%20%7C%20GKE-blue.svg?logo=kubernetes)](https://k3s.io)
[![Docker Compose](https://img.shields.io/badge/Docker%20Compose-Supported-2496ED.svg?logo=docker)](https://docker.com)
[![Latest Release](https://img.shields.io/badge/Release-v2.5.0-brightgreen.svg)](https://github.com/anilkumar2713/KubeClusterSnap-Storefront/releases/tag/v2.5.0)
[![Edition](https://img.shields.io/badge/Edition-Community%20%26%20Enterprise-009688.svg)](#kubeclustersnap-editions)
[![License](https://img.shields.io/badge/License-Proprietary-red.svg)](#intellectual-property--licensing)

**KubeClusterSnap** is the all-in-one visual Internal Developer Platform (IDP) and real-time control plane built for fast-moving developers, engineering leads, and growing tech companies. It turns complex container orchestration into a sleek, 1-click self-service dashboard with multi-cluster kubeconfig management, database volume state snapshotting, interactive container TTY shell access, and real-time observability.

Now available with **Dual Distribution Options**: Deploy natively inside Kubernetes via **Helm Chart** or launch instantly anywhere via **Docker Compose**!

---

## 🚀 What's New in Release v2.5.0

* **🐳 Standalone Docker Compose Support (`docker compose up -d`):** Customers and developers can now launch KubeClusterSnap in seconds using `docker compose up -d` without needing Helm or installing control plane manifests directly into Kubernetes!
* **🔌 Multi-Kubeconfig & Custom Cluster Connection Manager:** Connect KubeClusterSnap to any custom Kubernetes cluster (remote k3s, AWS EKS, Azure AKS, GCP GKE, Kind, Minikube) by uploading or pasting custom Kubeconfig YAML files directly from the UI or REST API.
* **⚡ 1-Click Cluster Context Switcher:** Seamlessly switch your entire dashboard observability & control plane context between connected clusters directly from the top Navbar dropdown.
* **📸 Database & Container State Cloning ("Snap" Feature):** 1-click volume snapshotting of container data paths (`/data`, `/var/lib/mysql`, etc.) and instant cloning into new, isolated preview environments pre-hydrated with your state.
* **Interactive In-Browser Container Terminal (`kubectl exec` over WebSockets):** Open a live TTY shell directly inside running pod containers from the web UI (`xterm.js`).

Read full release details in our [GitHub Release Notes](https://github.com/anilkumar2713/KubeClusterSnap-Storefront/releases/tag/v2.5.0).

---

## Quick Start & Deployment Options

Customers can choose either deployment method based on their infrastructure preferences:

### Option A: Standalone Docker Compose (`docker compose up -d`)

Ideal for local Docker Desktop users, developers, or standalone Linux VMs/EC2 instances:

```bash
# 1. Download docker-compose.yml
curl -sSL https://raw.githubusercontent.com/anilkumar2713/KubeClusterSnap-Storefront/master/docker-compose.yml -o docker-compose.yml

# 2. Launch KubeClusterSnap Control Plane
docker compose up -d
```

- **Frontend Dashboard:** Open **[http://localhost:30333](http://localhost:30333)** (or `http://localhost:30300`)
- **Backend Control API:** **[http://localhost:30888](http://localhost:30888)**

> **Note:** Docker Compose automatically mounts your host `${HOME}/.kube/config` into the control plane container so you can control your local or remote clusters out of the box!

---

### Option B: Native Kubernetes Installation via Helm (`helm upgrade --install`)

Ideal for cluster-native deployment inside Kubernetes:

```bash
# Install or Upgrade KubeClusterSnap via Helm
helm upgrade --install kubeclustersnap ./charts/kubeclustersnap --create-namespace -n kubeclustersnap
```

- **Frontend Dashboard:** Open **[http://localhost:30300](http://localhost:30300)**
- **Backend Control API:** **[http://localhost:30800](http://localhost:30800)**

---

## What Problem Does KubeClusterSnap Solve?

### 1. Zero-Friction Deployment Choices (Helm OR Docker Compose)
* **The Problem:** Non-Kubernetes users or teams testing control plane software don't want to set up Helm or in-cluster CRDs just to run a dashboard UI.
* **The Solution:** **Dual Distribution Options.** Deploy natively via Helm inside Kubernetes, or run standalone via Docker Compose (`docker compose up -d`) with mounted host kubeconfig access.

### 2. Multi-Cluster Fragmentation & Kubeconfig Switching Hassle
* **The Problem:** Developers and DevOps engineers constantly juggle multiple terminal tabs, environment variables, and `kubectl config use-context` commands to manage dev, staging, and production clusters.
* **The Solution:** **Unified Multi-Cluster Manager.** Import custom `.kube/config` files or paste YAML text directly in the UI. Switch active cluster context with 1-click from the Navbar dropdown.

### 3. Solves Database & Test State Isolation (The "Snap" Engine)
* **The Problem:** Developers spend hours generating mock data or trying to reproduce edge-case production bugs because test container environments start completely empty every time.
* **The Solution:** **1-Click Volume Snapshotting & State Cloning.** Developers can take an instant snapshot of a populated database or application volume (Redis, Postgres, MySQL) and clone it into a brand-new, isolated environment pre-hydrated with the exact same data.

---

## KubeClusterSnap Feature Matrix & Editions

| Feature | Community Edition (Free) | Enterprise Edition (Production Teams) |
| :--- | :---: | :---: |
| **Standalone Docker Compose Support (`docker compose up -d`)** | ✅ Included | ✅ Included |
| **Helm Chart Deployment (`helm upgrade --install`)** | ✅ Included | ✅ Included |
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
| **24/7 Dedicated SLA & Architecture Consultation** | ❌ | ✅ Included |

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

## Intellectual Property & Licensing

**Copyright (c) 2026 Anil Gottapu. All Rights Reserved. Proprietary & Confidential.**

KubeClusterSnap software and its distribution artifacts are protected under proprietary copyright laws. Unauthorized copying, reverse engineering, redistribution, or commercial exploitation is strictly prohibited.
