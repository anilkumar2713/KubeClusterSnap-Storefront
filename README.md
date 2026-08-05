# KubeClusterSnap — Visual Kubernetes Developer IDP & Real-Time Control Plane

[![Kubernetes](https://img.shields.io/badge/Kubernetes-k3s%20%7C%20EKS%20%7C%20AKS%20%7C%20GKE-blue.svg?logo=kubernetes)](https://k3s.io)
[![Docker Compose](https://img.shields.io/badge/Docker%20Compose-Supported-2496ED.svg?logo=docker)](https://docker.com)
[![Latest Release](https://img.shields.io/badge/Release-v2.7.0-brightgreen.svg)](https://github.com/anilkumar2713/KubeClusterSnap-Storefront/releases/tag/v2.7.0)
[![Edition](https://img.shields.io/badge/Edition-Community%20%26%20Enterprise-009688.svg)](#kubeclustersnap-editions)
[![License](https://img.shields.io/badge/License-Proprietary-red.svg)](#intellectual-property--licensing)

**KubeClusterSnap** is the all-in-one visual Internal Developer Platform (IDP) and real-time control plane built for fast-moving developers, engineering leads, and growing tech companies. It turns complex container orchestration into a sleek, 1-click self-service dashboard with multi-cluster kubeconfig management, database volume state snapshotting, interactive container TTY shell access, and real-time observability.

Now available with **Zero-Clone Installation**: Deploy natively inside Kubernetes via **`kubectl apply`** or **`helm install`**, or launch instantly anywhere via **`docker compose`** without cloning the repo!

---

## 🚀 What's New in Release v2.7.0

* **🚀 Deployment & Image Tag Sync Studio**: Monitor every container image tag (`nginx:latest`, `redis:alpine`, `node:18-alpine`) across all namespaces with 1-click interactive image tag updates and real-time sync state drift reconciliation.
* **📦 Cluster Resources & Pods Control Center**: Aggregated 5-in-1 control center featuring dedicated sub-tabs for **Pods** (health matrix, logs, xterm exec shell, terminate pod), **Services** (ClusterIP/NodePort/LoadBalancer), **Ingress Routes** (host rules & backend targets), **Secrets & ConfigMaps** (with Reveal/Hide toggle), and **Port Forwards & Tunnels**.
* **⚡ Single-Command Zero-Clone Deployments**: Deploy natively in any cluster or Docker engine with 1-command `kubectl apply`, `helm install`, or `docker compose` without cloning the repository!
* **🗄️ Visual Database Studio**: Auto-detect, connect, and query PostgreSQL, MySQL, Redis, and MongoDB instances cluster-wide without leaving the dashboard.
* **📸 Volume State Snapshotting**: Capture 1-click compressed tarball snapshots of live container data volumes and clone them into pre-hydrated preview environments.

---

## Quick Start & Deployment Options (Zero-Clone Installation)

Deploy KubeClusterSnap into any Kubernetes cluster or local Docker engine with a **single command** without cloning the repo:

### Option A: One-Command `kubectl apply` (Zero-Clone for Kubernetes)
```bash
kubectl apply -f https://raw.githubusercontent.com/anilkumar2713/KubeClusterSnap/main/k8s/kubeclustersnap-all-in-one.yaml
```
- **Frontend Dashboard:** Open **[http://localhost:30333](http://localhost:30333)** (or your NodePort/Ingress IP)
- **Backend Control API:** **[http://localhost:30888](http://localhost:30888)**

---

### Option B: One-Command `helm install` (Zero-Clone for Helm Users)
```bash
helm install kubeclustersnap https://raw.githubusercontent.com/anilkumar2713/KubeClusterSnap/main/charts/kubeclustersnap-2.7.0.tgz --create-namespace -n kubeclustersnap-system
```

---

### Option C: One-Command `docker compose` (Zero-Clone for Docker / Local / VM)
```bash
curl -sSL https://raw.githubusercontent.com/anilkumar2713/KubeClusterSnap/main/docker-compose.prod.yml | docker compose -f - up -d
```
- **Frontend Dashboard:** Open **[http://localhost:30333](http://localhost:30333)**
- **Backend Control API:** **[http://localhost:30888](http://localhost:30888)**

---

## What Problem Does KubeClusterSnap Solve?

### 1. Zero-Friction Deployment Choices (`kubectl`, `helm`, or `docker compose`)
* **The Problem:** Non-Kubernetes users or teams testing control plane software don't want to set up Helm or in-cluster CRDs just to run a dashboard UI.
* **The Solution:** **Zero-Clone Installation.** Deploy natively via `kubectl apply` or `helm` inside Kubernetes, or run standalone via Docker Compose (`docker compose up -d`) with mounted host kubeconfig access.

### 2. Multi-Cluster Fragmentation & Kubeconfig Switching Hassle
* **The Problem:** Developers and DevOps engineers constantly juggle multiple terminal tabs, environment variables, and `kubectl config use-context` commands to manage dev, staging, and production clusters.
* **The Solution:** **Unified Multi-Cluster Manager.** Import custom `.kube/config` files or paste YAML text directly in the UI. Switch active cluster context with 1-click from the Navbar dropdown.

### 3. Solves Database & Test State Isolation (The "Snap" Engine)
* **The Problem:** Developers spend hours generating mock data or trying to reproduce edge-case production bugs because test container environments start completely empty every time.
* **The Solution:** **1-Click Volume Snapshotting & State Cloning.** Developers can take an instant snapshot of a populated database or application volume (Redis, Postgres, MySQL) and clone it into a brand-new, isolated environment pre-hydrated with the exact same data.

### 4. Database Interrogation Friction (Visual Database Studio)
* **The Problem:** Developers are forced to rely on external desktop tools like pgAdmin, MongoDB Compass, or RedisInsight, requiring tedious `kubectl port-forward` commands and separate credential management just to run a simple SQL query.
* **The Solution:** **Built-in Visual Database Studio.** Instantly auto-detect, connect, and query PostgreSQL, MySQL, MongoDB, or Redis instances running anywhere in your cluster right from the web UI without leaving the dashboard or exposing ports.

### 5. Cluster-Wide Troubleshooting Overhead (Cluster Resources Studio)
* **The Problem:** When an incident happens, SREs and Developers have to run a series of complex `kubectl get pods -A | grep name` commands to find failing workloads, then manually kill or restart them.
* **The Solution:** **Cluster Resources & Pods Studio.** Aggregates Pods, Services, Ingresses, Secrets, ConfigMaps, and Port-Forwards across your entire cluster into a single high-speed UI. You can kill pods (`kubectl delete pod`), trigger zero-downtime rolling restarts, view live container logs, or open an interactive terminal shell instantly from one screen.

---

## KubeClusterSnap Feature Matrix & Editions

| Feature | Community Edition (Free) | Enterprise Edition (Production Teams) |
| :--- | :---: | :---: |
| **Deployment & Image Tag Sync Studio (Interactive Image Updates & Force Sync)** | ✅ Included | ✅ Included |
| **Cluster Resources Studio (Pods, Services, Ingress, Secrets, Port-Forwards)** | ✅ Included | ✅ Included |
| **1-Command Zero-Clone Deployments (`kubectl`, `helm`, `docker compose`)** | ✅ Included | ✅ Included |
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

## Intellectual Property & Licensing

**Copyright (c) 2026 Anil Gottapu. All Rights Reserved. Proprietary & Confidential.**

KubeClusterSnap software and its distribution artifacts are protected under proprietary copyright laws. Unauthorized copying, reverse engineering, redistribution, or commercial exploitation is strictly prohibited.
