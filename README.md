# KubeClusterSnap — Visual Kubernetes Developer IDP & Real-Time Control Plane

[![Kubernetes](https://img.shields.io/badge/Kubernetes-k3s%20%7C%20EKS%20%7C%20AKS-blue.svg?logo=kubernetes)](https://k3s.io)
[![Latest Release](https://img.shields.io/badge/Release-v2.2.0-brightgreen.svg)](https://github.com/anilkumar2713/KubeClusterSnap-Storefront/releases/tag/v2.2.0)
[![Edition](https://img.shields.io/badge/Edition-Community%20%26%20Enterprise-009688.svg)](#kubeclustersnap-editions)
[![License](https://img.shields.io/badge/License-Proprietary-red.svg)](#intellectual-property--licensing)

**KubeClusterSnap** is the all-in-one visual Internal Developer Platform (IDP) and real-time control plane built for fast-moving developers, engineering leads, and growing tech companies. It eliminates Kubernetes friction by turning complex container orchestration into a sleek, 1-click self-service dashboard.

---

## 🚀 What's New in Release v2.2.0

* **Interactive In-Browser Container Terminal (`kubectl exec` over WebSockets):** Open a live TTY shell directly inside running pod containers from the web UI (`xterm.js`).
* **Multi-Namespace Helm Flexibility:** Seamlessly deploy KubeClusterSnap into any custom namespace (`testing`, `dev`, `prod`) using `.Release.Namespace`.
* **Security Hardening:** Mandatory API key authentication middleware (`X-KubeClusterSnap-API-Key`) and RFC 1123 custom namespace sanitization.

Read full release details in our [GitHub Release Notes](https://github.com/anilkumar2713/KubeClusterSnap-Storefront/releases/tag/v2.2.0).

---

## What Problem Does KubeClusterSnap Solve?

### 1. Eliminates Local Kubernetes Onboarding Friction
* **The Problem:** Developers waste hours writing complex `kubectl` manifests, creating namespaces manually, configuring services, and debugging port forwarding just to run local preview environments.
* **The Solution:** **1-Click Ephemeral Environments.** Developers can spin up isolated, multi-replica container workloads (NGINX, Redis, Node.js, microservices) in seconds with automated namespace isolation.

### 2. Instant Visual Observability Without Heavy Setup
* **The Problem:** Understanding how microservices communicate or finding out why a pod crashed requires digging through raw text logs or setting up heavy, expensive monitoring stacks.
* **The Solution:** **Built-In Real-Time Control Plane.** Features interactive network topology DAG maps, PromQL CPU/Memory telemetry graphs, live WebSocket cluster event streams, and container stdout/stderr log tailing out-of-the-box.

### 3. Developer Self-Service Control
* **The Problem:** Developers rely on DevOps engineers for basic operations like scaling workloads, restarting deployments, or cleaning up test resources.
* **The Solution:** **Self-Service Workload Control.** Developers can scale replicas, trigger zero-downtime rolling restarts, inspect live pod logs, run container terminals, and perform 1-click namespace teardowns safely.

---

## KubeClusterSnap Editions

| Feature | Community Edition (Free) | Enterprise Edition (Production Teams) |
| :--- | :---: | :---: |
| **Local k3s Single Cluster Support** | ✅ Included | ✅ Included |
| **Standard 1-Click Service Catalog** | ✅ Included | ✅ Included |
| **Real-Time Telemetry & Metric Graphs** | ✅ Included | ✅ Included |
| **Visual Network Topology Map** | ✅ Included | ✅ Included |
| **Live Cluster Event Drawer & Pod Logs** | ✅ Included | ✅ Included |
| **Interactive In-Browser Container Shell (xterm.js)** | ✅ Included | ✅ Included |
| **Multi-Cluster Support (AWS EKS / Azure AKS / GCP GKE)** | ❌ | ✅ Included |
| **Single Sign-On (SSO / OIDC) & Audit Logging** | ❌ | ✅ Included |
| **FinOps Auto-Sleep & TTL Garbage Collection**| ❌ | ✅ Included |
| **Custom PromQL Dashboard Builder** | ❌ | ✅ Included |
| **Production Helm Deployment & 24/7 SLA Support** | ❌ | ✅ Included |

---

## Community Edition Quick Start & Lifecycle Guide (Helm)

### 1. Initial Installation via Helm

Deploy KubeClusterSnap to your Kubernetes cluster in a single command using our public Helm chart:

```bash
# Install KubeClusterSnap Community Edition via Helm
helm install kubeclustersnap ./charts/kubeclustersnap --create-namespace -n kubeclustersnap
```

Open **[http://localhost:30300](http://localhost:30300)** in your browser to access the visual developer dashboard!

---

### 2. Upgrading to Every New Release (`helm upgrade`)

Whenever a new version of KubeClusterSnap is released (e.g. `v2.2.0`), you can upgrade your cluster deployment seamlessly without downtime:

```bash
# Upgrade your existing Helm release to the latest version
helm upgrade kubeclustersnap ./charts/kubeclustersnap -n kubeclustersnap
```

#### Upgrading in Custom Namespaces (`dev`, `testing`, `prod`)

If you installed KubeClusterSnap in a custom namespace, specify `-n <namespace>` during upgrade:

```bash
# Upgrade release in custom namespace (e.g., 'dev' or 'testing')
helm upgrade kubeclustersnap ./charts/kubeclustersnap -n testing
```

---

### 3. Upgrading from Community to Enterprise (BYOL Key)

To unlock Enterprise features on your existing deployment without reinstalling, pass your cryptographic License Key during `helm upgrade`:

```bash
# Unlock Enterprise Edition on existing deployment
helm upgrade kubeclustersnap ./charts/kubeclustersnap \
  -n kubeclustersnap \
  --set licenseKey="KCS-ENT-YOURORGANIZATION-HASH"
```

---

### 4. Uninstalling / Cleanup

To uninstall KubeClusterSnap and remove control plane resources:

```bash
# Uninstall KubeClusterSnap release
helm uninstall kubeclustersnap -n kubeclustersnap
```

---

## Need Full-Scale Production Deployment or Enterprise Features?

If your engineering team is scaling up and needs:
* **Production Multi-Cluster Support** (AWS EKS, Azure AKS, Google Cloud GKE)
* **Enterprise Single Sign-On (SSO)** & Role-Based Access Controls
* **FinOps Cost Optimization** (Auto-sleep for idle preview environments)
* **Custom Enterprise Helm Charts** tailored to your cloud infrastructure
* **24/7 Dedicated SLA & Architecture Consultation**

### Contact Sales & Enterprise Engineering

Get in touch directly with our founder to unlock Enterprise licensing or request a custom cloud deployment architecture setup:

* 📧 **Email:** [anilkumargottapu9@gmail.com](mailto:anilkumargottapu9@gmail.com)
* 🌐 **Public Storefront:** [https://github.com/anilkumar2713/KubeClusterSnap-Storefront](https://github.com/anilkumar2713/KubeClusterSnap-Storefront)

---

## Intellectual Property & Licensing

**Copyright (c) 2026 Anil Gottapu. All Rights Reserved. Proprietary & Confidential.**

KubeClusterSnap software and its distribution artifacts are protected under proprietary copyright laws. Unauthorized copying, reverse engineering, redistribution, or commercial exploitation is strictly prohibited.
