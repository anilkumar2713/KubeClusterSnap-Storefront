# KubeClusterSnap V2 — Ephemeral Environment & Observability Platform

## Executive Summary

**KubeClusterSnap V2** is a visual Internal Developer Platform (IDP) and real-time Kubernetes control plane designed for local `k3s` clusters. It combines automated ephemeral environment provisioning with high-density Grafana/Lens-style observability, telemetry graphs, network topology visualization, live event streaming, and workload management.

---

## What Problem Does KubeClusterSnap Solve?

### 1. Developer Onboarding & Local Environment Friction
* **Traditional Pain Point:** Developers waste hours writing complex `kubectl` manifests, creating namespaces manually, configuring services, and debugging port forwarding to run isolated preview environments.
* **KubeClusterSnap Solution:** Provides a **1-click Service Catalog**. Developers can spin up an isolated, multi-replica environment (e.g., NGINX, Redis, custom microservices) in seconds with automated namespace isolation (`env-<name>`).

### 2. Lack of Visibility in Local Kubernetes Clusters
* **Traditional Pain Point:** Understanding how microservices communicate (Ingress ➔ Service ➔ Deployment ➔ Pods) requires memorizing CLI commands or digging through raw text logs.
* **KubeClusterSnap Solution:** Generates an interactive **ReactFlow Topology Graph** showing live resource relationships and health states in real time.

### 3. Monitoring & Telemetry Overhead
* **Traditional Pain Point:** Setting up or checking Prometheus metrics, CPU/memory limits, and pod restart rates requires navigating separate dashboards or running heavy monitoring stacks.
* **KubeClusterSnap Solution:** Built-in **ECharts PromQL Telemetry Panel** displaying multi-line CPU and memory consumption graphs with **graceful degradation** (if Prometheus is absent, all control functions remain 100% operational).

### 4. Real-Time Debugging & Event Streaming
* **Traditional Pain Point:** Finding out why a pod failed (e.g., `CrashLoopBackOff`, OOMKilled) requires continuous terminal polling.
* **KubeClusterSnap Solution:** Features a **WebSocket Live Event Drawer** tailing `kubectl get events --watch` directly into the UI alongside a **Pod Container Log Inspector**.

---

## Core Capabilities & Features

| Capability | Description |
| :--- | :--- |
| **1-Click Provisioning** | Instantly deploys `V1Namespace`, `V1Deployment`, and `V1Service` via the official Kubernetes Python SDK. |
| **Workload Control Plane** | Modifies replica counts (`PATCH /scale`), triggers zero-downtime rolling restarts (`POST /restart`), and inspects container stdout/stderr logs. |
| **Topology DAG** | Interactive node-graph rendering ingress entry points, service node ports, deployment readiness, and pod IPs. |
| **Live WebSocket Events** | Streams real-time warnings, pod additions, and cluster lifecycle events over WebSockets (`/ws/events`). |
| **One-Click Teardown** | Completely deletes `env-<name>` namespaces, cascading the cleanup of all underlying resources. |
| **Strict In-Cluster RBAC** | Scoped via dedicated `kubeclustersnap` namespace with minimal `ClusterRole` permissions. |

---

## How to Access & Use KubeClusterSnap

### 1. Web Dashboard (Frontend UI)
* **URL:** [http://localhost:3000](http://localhost:3000)
* **What you can do:**
  * Click **"Launch NGINX"** on the Service Catalog to spin up a new environment.
  * Adjust replicas using the **+ / -** controls in the Workload Action Grid and click **Apply Scale**.
  * Click **"Logs"** to inspect live container output.
  * Click **"Live Events"** in the top navigation bar to open the WebSocket event drawer.
  * Click **"Teardown"** to clean up an environment.

### 2. Backend REST API & OpenAPI Documentation
* **Swagger Interactive Docs:** [http://localhost:8000/docs](http://localhost:8000/docs) (or [http://localhost:30800/docs](http://localhost:30800/docs) if using in-cluster NodePort)
* **Health Endpoint:** `GET http://localhost:8000/healthz`
* **WebSocket Endpoint:** `ws://localhost:8000/ws/events`

### 3. Kubernetes Namespace & CLI Inspection
All platform control plane components run in the dedicated `kubeclustersnap` namespace:
```powershell
# Inspect KubeClusterSnap backend pod & service in cluster
kubectl get all -n kubeclustersnap

# Inspect active ephemeral developer environments
kubectl get ns
```

---

## Technology Stack

* **Frontend:** React, Next.js (App Router), Tailwind CSS, ECharts (`echarts-for-react`), ReactFlow (`@xyflow/react`), Lucide Icons.
* **Backend:** Python 3, FastAPI, Official Kubernetes Python Client (`kubernetes`), `httpx`, `websockets`, Pydantic V2.
* **Target Environment:** Local `k3s` / Kubernetes cluster.
