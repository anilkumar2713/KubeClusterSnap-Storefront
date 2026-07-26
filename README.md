# KubeLaunch V2 — Grafana-Style Observability & Ephemeral Developer IDP

[![Kubernetes](https://img.shields.io/badge/Kubernetes-k3s-blue.svg?logo=kubernetes)](https://k3s.io)
[![FastAPI](https://img.shields.io/badge/FastAPI-v2.1.0-009688.svg?logo=fastapi)](https://fastapi.tiangolo.com)
[![Next.js](https://img.shields.io/badge/Next.js-14-black.svg?logo=next.js)](https://nextjs.org)
[![Prometheus](https://img.shields.io/badge/Prometheus-PromQL-E6522C.svg?logo=prometheus)](https://prometheus.io)

**KubeLaunch V2** is a visual Internal Developer Platform (IDP) and high-density, real-time Kubernetes control plane designed to provision and observe ephemeral application workloads on local `k3s` clusters. It combines automated container provisioning with Grafana/Lens-style telemetry, PromQL metrics, interactive network topology DAGs, WebSocket event tailing, and workload control plane actions.

---

## Architecture Overview

```text
┌─────────────────────────────────────────────────────────────────────────────────┐
│                           Next.js 14 Frontend UI                                │
│  ┌──────────────────────┐  ┌──────────────────────┐  ┌──────────────────────┐  │
│  │ Top KPIs & Gauges    │  │ ECharts PromQL Panel │  │ ReactFlow DAG Map    │  │
│  └──────────────────────┘  └──────────────────────┘  └──────────────────────┘  │
│  ┌───────────────────────────────────────────────────────────────────────────┐  │
│  │ Workload Control Grid (Scale | Restart | Log Inspector | Teardown)        │  │
│  └───────────────────────────────────────────────────────────────────────────┘  │
└───────────────────────────────────────┬─────────────────────────────────────────┘
                                        │
                         REST API & WebSockets (/ws/events)
                                        │
┌───────────────────────────────────────▼─────────────────────────────────────────┐
│                        FastAPI Control Plane Backend                            │
│                 (Runs in dedicated 'kubelaunch' namespace)                      │
│                                       │                                         │
│   ┌───────────────────────────┐       │       ┌─────────────────────────────┐   │
│   │ Official Python K8s SDK   │ ──────┴──────>│ Prometheus PromQL Engine    │   │
│   └─────────────┬─────────────┘               └──────────────┬──────────────┘   │
└─────────────────┼────────────────────────────────────────────┼──────────────────┘
                  │                                            │
                  ▼                                            ▼
┌─────────────────────────────────────────────────────────────────────────────────┐
│                                 k3s Cluster                                     │
│  ┌─────────────────────┐  ┌──────────────────────┐  ┌────────────────────────┐  │
│  │ Namespace:          │  │ Namespace:           │  │ Telemetry:             │  │
│  │ kubelaunch          │  │ env-<name>           │  │ Prometheus             │  │
│  │ (SA, Role, Backend) │  │ (Deployment, Pods)   │  │ (http://prometheus:9090│  │
│  └─────────────────────┘  └──────────────────────┘  └────────────────────────┘  │
└─────────────────────────────────────────────────────────────────────────────────┘
```

---

## Key Features

### 1. Developer Service Catalog & Custom Workload Provisioner
- **1-Click Catalog Tiles:** Pre-configured templates for NGINX (`nginx:latest`), Redis (`redis:alpine`), and Node.js (`node:18-alpine`).
- **Custom Container Provisioner:** Deploy **any** container image from Docker Hub, GitHub Container Registry (GHCR), or local registries.
- **Custom Specifications & Env Vars:**
  - Custom target namespace override (e.g. `default`, `kubelaunch`, or auto `env-<name>`).
  - Environment variable key-value pair editor (`env`).
  - Container CPU and Memory resource requests and limits (`resources`).

### 2. High-Density Grafana-Style Observability
- **Top Row KPI Gauges:** Active environments count, global CPU cores consumed, total memory (MB) working set, and pod restart rate per hour.
- **ECharts PromQL Telemetry Panel:** Multi-line time-series graphs tracking CPU and memory usage over time.
- **Graceful Degradation:** If Prometheus is absent or unreachable, the panel displays a clean "No Telemetry Available" fallback while all control plane operations remain 100% functional.
- **Interactive ReactFlow Topology Graph:** Visual DAG illustrating network traffic flow from Ingress ➔ Service NodePort ➔ Deployment ➔ Pods.

### 3. Workload Control Plane & Action Grid
- **Replica Scaling:** Dynamically scale deployment replica counts (`PATCH /api/v1/environments/{name}/scale`).
- **Zero-Downtime Rolling Restarts:** Trigger rolling restarts via pod template `restartedAt` annotations (`POST /api/v1/environments/{name}/restart`).
- **Container Log Inspector:** Modal viewer for stdout/stderr container logs (`GET /api/v1/environments/{name}/pods/{pod_name}/logs`).
- **One-Click Teardowns:** Cascading deletion of namespaces and underlying child resources (`DELETE /api/v1/environments/{name}`).

### 4. WebSocket Live Event Streaming
- Side drawer tailing real-time cluster events over WebSockets (`ws://localhost:30800/ws/events`), equivalent to `kubectl get events --watch`.

---

## Project Structure

```text
KubeLaunch/
├── backend/
│   ├── app/
│   │   ├── main.py            # FastAPI entrypoint, CORS & router registration
│   │   ├── schemas.py         # Pydantic V2 models (Scale, Logs, Topology, Metrics)
│   │   ├── k8s_client.py      # Kubernetes Python SDK driver (Provision, Scale, Restart, Logs)
│   │   ├── prometheus.py      # PromQL query client & fallback handler
│   │   └── routers/
│   │       ├── environments.py# Control plane endpoints
│   │       ├── metrics.py     # PromQL telemetry endpoints
│   │       └── events.py      # WebSocket /ws/events stream
│   ├── Dockerfile
│   └── requirements.txt
├── frontend/
│   ├── src/
│   │   ├── app/
│   │   │   ├── layout.tsx
│   │   │   ├── page.tsx       # Main Observability Dashboard
│   │   │   └── globals.css
│   │   ├── components/
│   │   │   ├── Navbar.tsx           # Header & Live Events toggle
│   │   │   ├── KPIGaugeRow.tsx      # Top row aggregate KPIs
│   │   │   ├── TelemetryCharts.tsx  # ECharts multi-line PromQL charts
│   │   │   ├── TopologyMap.tsx      # ReactFlow DAG topology graph
│   │   │   ├── ActionCenterGrid.tsx # Workload control action grid
│   │   │   ├── EventLogDrawer.tsx   # WebSocket live event log side drawer
│   │   │   ├── PodLogModal.tsx      # Container stdout/stderr log inspector
│   │   │   ├── CatalogCard.tsx      # Service Catalog tiles
│   │   │   └── LaunchModal.tsx      # Workload launch modal (Specs & Env Vars)
│   │   └── lib/
│   │       └── api.ts         # REST API SDK
│   ├── package.json
│   ├── tailwind.config.js
│   └── tsconfig.json
├── k8s/
│   ├── rbac.yaml              # Namespace, ServiceAccount, ClusterRole, ClusterRoleBinding
│   └── backend-deployment.yaml# Backend pod & NodePort service (Port 30800)
├── info.md                    # Executive summary & quick reference
└── README.md
```

---

## Quick Start Guide

### Option 1: 1-Command Installation via Helm (Recommended)

Install KubeLaunch into any Kubernetes cluster in a single command using our built-in Helm chart:

```bash
# 1. Install KubeLaunch Helm Chart
helm install kubelaunch ./charts/kubelaunch
```

This automatically provisions:
* `Namespace`: `kubelaunch`
* `ServiceAccount` & RBAC (`kubelaunch-sa`, `kubelaunch-role`)
* `Backend`: NodePort `30800` (`http://localhost:30800`)
* `Frontend`: NodePort `30300` (`http://localhost:30300`)

---

### Option 2: Installation via kubectl

```bash
kubectl apply -f k8s/rbac.yaml
kubectl apply -f k8s/backend-deployment.yaml
```

Verify status in cluster:
```bash
kubectl get all -n kubelaunch
```

---

### 2. Start the Frontend Dashboard

```bash
cd frontend
npm install
npm run dev
```

Open **[http://localhost:3000](http://localhost:3000)** in your browser.

---

### 3. Local Backend Development (Optional)

If running the Python backend locally outside the cluster:

```bash
cd backend
python -m venv venv
# On Windows PowerShell:
.\venv\Scripts\Activate.ps1
# On Linux/macOS:
source venv/bin/activate

pip install -r requirements.txt
uvicorn app.main:app --reload --port 8000
```

FastAPI interactive OpenAPI docs will be available at [http://localhost:8000/docs](http://localhost:8000/docs).

---

## REST & WebSocket API Specification

| Method | Endpoint | Description |
| :--- | :--- | :--- |
| `POST` | `/api/v1/environments/launch` | Provision workload (supports custom specs, env vars, target namespace) |
| `GET` | `/api/v1/environments` | List all active environments and workload statuses |
| `PATCH` | `/api/v1/environments/{name}/scale` | Scale replica count (`{"replicas": N}`) |
| `POST` | `/api/v1/environments/{name}/restart` | Trigger rolling deployment restart |
| `GET` | `/api/v1/environments/{name}/pods/{pod_name}/logs` | Fetch container stdout/stderr logs |
| `GET` | `/api/v1/environments/{name}/topology` | Fetch ReactFlow topology DAG nodes and edges |
| `DELETE` | `/api/v1/environments/{name}` | Initiate cascading namespace teardown |
| `GET` | `/api/v1/metrics/summary` | Global aggregate metrics (CPU, Memory, Restarts) |
| `GET` | `/api/v1/metrics/timeseries` | PromQL time-series dataset for ECharts |
| `WS` | `/ws/events` | Real-time WebSocket Kubernetes cluster event stream |

## KubeClusterSnap Editions

| Feature | Community Edition (Free Forever) | Enterprise Edition (Production Teams) |
| :--- | :---: | :---: |
| **Local k3s Single Cluster Support** | ✅ Included | ✅ Included |
| **Standard Service Catalog Templates** | ✅ Included | ✅ Included |
| **PromQL Telemetry & ECharts Panel** | ✅ Included | ✅ Included |
| **ReactFlow Topology DAG** | ✅ Included | ✅ Included |
| **WebSocket Real-Time Event Stream** | ✅ Included | ✅ Included |
| **Multi-Cluster Support (AWS EKS / Azure AKS)** | ❌ | ✅ Included |
| **Single Sign-On (SSO) & Audit Logging** | ❌ | ✅ Included |
| **FinOps Auto-Sleep & TTL Garbage Collection**| ❌ | ✅ Included |
| **Custom PromQL Dashboard Builder** | ❌ | ✅ Included |
| **24/7 Dedicated SLA & Consultation** | ❌ | ✅ Included |

### Enterprise License Key (BYOL) Activation

To unlock Enterprise features, paste your cryptographic License Key into your `values.yaml`:

```yaml
licenseKey: "KCS-ENT-YOURORGANIZATION-HASH"
```

Then run `helm upgrade`:
```bash
helm upgrade kubeclustersnap ./charts/kubeclustersnap
```

---

## Enterprise Sales & Licensing Contact

To purchase an Enterprise License Key or request a full-scale cloud deployment consultation, contact:
* **Email:** [anilkumargottapu9@gmail.com](mailto:anilkumargottapu9@gmail.com)
* **Website:** [https://github.com/anilkumar2713/KubeClusterSnap](https://github.com/anilkumar2713/KubeClusterSnap)

---

## Intellectual Property & Licensing

**Copyright (c) 2026 Anil Gottapu. All Rights Reserved. Proprietary & Confidential.**

This repository and its underlying control plane software are strictly proprietary. Unauthorized copying, distribution, reverse engineering, or commercial exploitation via any medium is strictly prohibited. See [LICENSE](file:///d:/ideas/KubeLaunch/LICENSE) for details.
