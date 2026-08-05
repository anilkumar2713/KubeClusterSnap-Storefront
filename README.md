# KubeClusterSnap V2.7.0 — Visual Kubernetes Developer IDP & Real-Time Control Plane

[![Kubernetes](https://img.shields.io/badge/Kubernetes-k3s%20%7C%20EKS%20%7C%20AKS%20%7C%20GKE-blue.svg?logo=kubernetes)](https://k3s.io)
[![Docker Compose](https://img.shields.io/badge/Docker%20Compose-Supported-2496ED.svg?logo=docker)](https://docker.com)
[![FastAPI](https://img.shields.io/badge/FastAPI-v2.7.0-009688.svg?logo=fastapi)](https://fastapi.tiangolo.com)
[![Next.js](https://img.shields.io/badge/Next.js-14-black.svg?logo=next.js)](https://nextjs.org)

**KubeClusterSnap V2.7.0** is a visual Internal Developer Platform (IDP) and high-density, real-time Kubernetes control plane designed to provision, clone, observe, and control ephemeral application workloads across multiple Kubernetes clusters. It combines automated container provisioning with a full **Visual Database Studio**, **Cluster-Wide Namespace & Workload Troubleshooting**, interactive container TTY shell terminals, PromQL telemetry, network topology DAGs, and WebSocket event tailing.

Deploy natively in Kubernetes via **Helm Chart** or launch standalone via **Docker Compose** (`docker compose up -d`).

---

## ⚡ Zero-Clone Single-Command Installation Guide

Deploy KubeClusterSnap into any Kubernetes cluster or local Docker engine with a **single command** — no git clone required!

---

### Option A: One-Command `kubectl apply` (Kubernetes Cluster Deployment)
Deploys the full KubeClusterSnap backend, frontend, RBAC ServiceAccount, ClusterRoles, and NodePort services into any Kubernetes cluster (`k3s`, `EKS`, `AKS`, `GKE`, `Minikube`, `Kind`):

```bash
kubectl apply -f https://raw.githubusercontent.com/anilkumar2713/KubeClusterSnap/main/k8s/kubeclustersnap-all-in-one.yaml
```

- **Frontend Dashboard:** Open **[http://localhost:30333](http://localhost:30333)** (or your NodePort/Ingress IP)
- **Backend Control API:** **[http://localhost:30888](http://localhost:30888)**

---

### Option B: One-Command `helm install` (Helm Package Manager)
Install via Helm directly from the remote release chart package:

```bash
helm install kubeclustersnap https://raw.githubusercontent.com/anilkumar2713/KubeClusterSnap/main/charts/kubeclustersnap-2.7.0.tgz --create-namespace -n kubeclustersnap-system
```

---

### Option C: One-Command `docker compose` (Docker Desktop / Local / VM)
Launch KubeClusterSnap directly on any server or local laptop with Docker installed:

```bash
curl -sSL https://raw.githubusercontent.com/anilkumar2713/KubeClusterSnap/main/docker-compose.prod.yml | docker compose -f - up -d
```

- **Frontend Dashboard:** Open **[http://localhost:30333](http://localhost:30333)**
- **Backend Control API:** **[http://localhost:30888](http://localhost:30888)**

---

## 📖 Deep Component Architecture & Production Operational Manual

Each section below provides a detailed breakdown of the internal components, capabilities, and production troubleshooting workflows for every studio in KubeClusterSnap.

---

### 1. 🚀 Deployment & Container Image Tag Sync Studio (`Deployments & Sync`)

#### 🧩 Included Components & Architecture
- **`SyncHeaderBanner`**: Displays total cluster deployment count, out-of-sync drift indicators, and cluster sync health metrics.
- **`DeploymentTableCard`**: High-density grid detailing Deployment Name, Target Namespace, Active Container Image Tag Badge (`nginx:latest`, `node:18-alpine`), Replica Counts (`3/3`), Sync Status Badges (**`Synced`** vs **`OutOfSync`**), and control buttons (`Sync Now`, `Update Image`, `Scale`, `Restart`).
- **`UpdateImageModal`**: Interactive modal dialog for selecting or typing new container image tags (`v2.4.0` → `v2.5.0` or SHA digest) with real-time format validation.

#### 🛠️ What You Can Do
- **Audit Image Versions**: Instantly inspect every container image tag deployed across all cluster namespaces from a single screen.
- **1-Click Image Tag Bumping**: Change container image tags interactively in real time without writing or committing raw K8s Deployment YAML files.
- **Drift Reconciliation**: Reconcile image tag drift with a 1-click **`Sync Now`** button to pull updated manifests and align cluster state.
- **Workload Lifecycle Actions**: Trigger zero-downtime rolling restarts (`kubectl rollout restart`) and scale deployment replicas up or down on the fly.

#### 🏭 How to Use in Production
1. **CI/CD Rollout Verification**: After pushing a new container build from your CI/CD pipeline, open the **Deployment & Sync Studio** to verify that pods in staging or production have successfully updated to the new image tag.
2. **Emergency Production Hotfixes**: During critical production incidents, use the **`Update Image`** modal to immediately deploy a hotfix image tag or rollback to a previous container version in seconds.

---

### 2. 📦 Cluster Pods & Health Debugger (`Cluster Resources -> Pods`)

#### 🧩 Included Components & Architecture
- **`PodFilterBar`**: Namespace selector dropdown and health state filter pills (**`All Pods`**, **`Healthy Running`**, **`Crashing/Error`**, **`Pending`**).
- **`PodMatrixTable`**: Displays Pod Name, Namespace, IP address, Node hostname, Container Image Tag, Restarts count, Status badge (**`Running`**, **`CrashLoopBackOff`**, **`ImagePullBackOff`**), and action buttons (`Logs`, `Exec`, `Snap`, `Kill Pod`).
- **`LogViewerModal`**: Live streaming container log console with auto-scroll and keyword search filtering.
- **`XtermExecModal`**: In-browser full-screen xterm.js TTY terminal connected over WebSockets to `kubectl exec`.

#### 🛠️ What You Can Do
- **Isolate Failing Workloads**: Instantly filter for crashing or pending pods across all cluster namespaces using health pills.
- **Stream Live Container Logs**: Inspect stdout/stderr logs directly from container runtimes.
- **Interactive Container TTY Shell (`kubectl exec`)**: Open interactive shell sessions inside running pod containers for live debugging.
- **💀 Terminate & Force Recreate**: Single-click pod termination (`kubectl delete pod`) to test pod recovery times or clear frozen container states.

#### 🏭 How to Use in Production
1. **Incident Response & Triage**: When alerted to a service degradation, click the **`Crashing/Error`** filter pill to pinpoint failing pods instantly.
2. **Live Debugging**: Open the **`Logs`** window to read stderr tracebacks, then launch the **`Exec`** shell to inspect container environment variables, configuration files, and network connectivity inside the pod.

---

### 3. 🔌 Kubernetes Services Registry (`Cluster Resources -> Services`)

#### 🧩 Included Components & Architecture
- **`ServicesTable`**: Displays Service Name, Namespace, Service Type (**`ClusterIP`**, **`NodePort`**, **`LoadBalancer`**), Cluster IP address, External/Node Ports, and Target Ports.
- **`SelectorTagList`**: Visual pill badges displaying service selector key-value pairs (`app=backend`, `tier=api`).

#### 🛠️ What You Can Do
- **Inspect Service Routing**: Verify network target mappings and selector matches for all microservices across namespaces.
- **Port Mapping Audit**: Inspect assigned internal Cluster IPs, NodePort numbers (`30333`, `30888`), and container target ports.

#### 🏭 How to Use in Production
1. **Service Discovery Troubleshooting**: When ingress or API gateway calls return 502/503 Bad Gateway errors, inspect the **Services Registry** to verify that Service selector labels accurately match active Pod labels.

---

### 4. 🌐 Ingress Routes & Traffic Controller (`Cluster Resources -> Ingress Routes`)

#### 🧩 Included Components & Architecture
- **`IngressTable`**: Displays Ingress Name, Namespace, Class/Controller, Host Domain Rules, HTTP Path Maps (`/api` → `backend-svc:8000`), Target Backend Ports, and TLS Security status.

#### 🛠️ What You Can Do
- **Audit Domain Exposures**: Review all public and internal domain host rules (`api.company.com`, `dashboard.local`) across ingress controllers (`Traefik`, `NGINX`).
- **Path Routing Verification**: Map HTTP path routing rules (`/api/v1` → `backend-svc`, `/` → `frontend-svc`).
- **TLS Security Check**: Monitor TLS secret configuration and HTTPS security status.

#### 🏭 How to Use in Production
1. **Ingress Routing Audits**: Ensure new microservice routes are correctly exposed and verify that production SSL/TLS certificates are properly bound to host domains.

---

### 5. 🔐 Secrets & ConfigMaps Security Store (`Cluster Resources -> Secrets & ConfigMaps`)

#### 🧩 Included Components & Architecture
- **`ConfigSecretTable`**: Displays Name, Namespace, Resource Type (**`Secret`** vs **`ConfigMap`**), Data Key Count, Creation Date, and interactive **`Reveal Keys`** / **`Hide Keys`** toggle buttons.
- **`KeyInspectorModal`**: Masked key listing with 1-click reveal to safely inspect key-value pairs (`DATABASE_URL`, `API_KEY`).

#### 🛠️ What You Can Do
- **Cluster-Wide Configuration Audit**: Inspect all ConfigMaps and Secrets across every namespace from a centralized store.
- **Key-Value Verification**: View stored key names and decrypt secret values securely for troubleshooting.

#### 🏭 How to Use in Production
1. **Configuration Troubleshooting**: Diagnose application startup failures caused by missing environment variables or secret keys without needing cluster-admin terminal access.

---

### 6. 🔀 Port Forwards & Local Tunnels Studio (`Cluster Resources -> Port Forwards`)

#### 🧩 Included Components & Architecture
- **`TunnelManagerTable`**: Displays Active Session ID, Target Pod/Service, Namespace, Local Tunnel URL (`http://localhost:30888`), Target Container Port, Status (**`Active`**), and **`Stop Tunnel`** action button.

#### 🛠️ What You Can Do
- **Manage Active Local Tunnels**: Monitor and manage all active local port-forwarding sessions.
- **1-Click Stop Tunnel**: Terminate active port-forwarding sessions instantly from the UI without searching for background process PIDs.

#### 🏭 How to Use in Production
1. **Secure Private Service Access**: Access internal database admin ports, private redis caches, or internal telemetry APIs securely on your local machine without exposing ports publicly.

---

### 7. 🗄️ Visual Database Studio (`Databases Studio`)

#### 🧩 Included Components & Architecture
- **`DBDiscoveryBanner`**: Automatically detects PostgreSQL, MySQL, Redis, and MongoDB instances running anywhere in the cluster.
- **`QueryConsole`**: Built-in SQL and NoSQL query console for executing queries, inspecting collections, browsing Redis keys, and analyzing DB latency.

#### 🛠️ What You Can Do
- **Direct DB Interrogation**: Query databases directly from the browser UI without external desktop tools like pgAdmin or RedisInsight.

#### 🏭 How to Use in Production
1. **Database Diagnostics**: Verify database migration execution and inspect table state directly during deployment verification.

---

### 8. 📸 Volume State Snapshotting & Cloning (`Snap Engine`)

#### 🧩 Included Components & Architecture
- **`SnapshotManager`**: 1-click compressed tarball snapshot engine for container volume data paths (`/var/lib/mysql`, `/data`).
- **`EnvironmentHydrator`**: Provisions isolated preview workloads pre-hydrated with real volume data snapshots.

#### 🛠️ What You Can Do
- **Capture State Archives**: Take instant compressed volume snapshots from live running pod volumes.
- **Spin Up Preview Environments**: Instantiate duplicate container environments pre-hydrated with snapshot data for testing and bug reproduction.

#### 🏭 How to Use in Production
1. **Production Bug Reproduction**: Snapshot staging database volumes to reproduce complex edge-case bugs in completely isolated ephemeral test environments.

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
| `GET` | `/api/v1/environments/resources/services` | Fetch cluster-wide Kubernetes Services registry |
| `GET` | `/api/v1/environments/resources/ingresses` | Fetch cluster-wide Ingress routes and rules |
| `GET` | `/api/v1/environments/resources/configs` | Fetch cluster-wide ConfigMaps and Secrets key registry |
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
