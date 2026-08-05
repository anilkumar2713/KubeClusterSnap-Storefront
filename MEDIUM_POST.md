# Stop Juggling `kubectl` Tabs: Meet KubeClusterSnap — The Real-Time Visual Kubernetes Control Plane & IDP

> **TL;DR:** We built **KubeClusterSnap** — an all-in-one visual Internal Developer Platform (IDP) and real-time Kubernetes control plane. It turns complex container management, image tag updates, pod debugging, DB querying, and volume snapshotting into a sleek, 1-click self-service web interface. Deploy it in 5 seconds without cloning the repository using a single `kubectl`, `helm`, or `docker compose` command!

---

## 🌩️ The Problem: The Modern Kubernetes Friction Tax

If you work with Kubernetes daily as a developer, DevOps engineer, or SRE, this workflow probably sounds familiar:

1. **Terminal Tab Sprawl**: You're constantly switching between 10 terminal tabs running `kubectl get pods -n dev`, `kubectl logs -f`, `kubectl exec -it`, and `kubectl port-forward`.
2. **Image Tag Update Overhead**: When testing a new container image build in staging, you have to write or manually edit deployment YAMLs or run verbose `kubectl set image deployment/frontend frontend=myrepo/frontend:v2.5.0` commands.
3. **Database Inspection Pain**: To query a Postgres or Redis instance running in Kubernetes, you're forced to establish temporary port-forwards, open external desktop tools (pgAdmin, RedisInsight), and manage credentials manually.
4. **Empty Test Containers**: Reproducing production edge cases in preview environments requires writing custom mock data scripts because fresh test containers start completely empty.

We asked ourselves: **Why isn't there a single, visual control plane that unifies workload debugging, image tag syncing, database interrogation, and volume state snapshotting?**

That's why we created **KubeClusterSnap**.

---

## ⚡ What is KubeClusterSnap?

**KubeClusterSnap** is a high-density, real-time Kubernetes observability and control suite. It brings developer self-service to your infrastructure, giving you complete visibility and control over your application workloads across any Kubernetes cluster (`k3s`, `AWS EKS`, `Azure AKS`, `GCP GKE`, `Minikube`, or `Kind`).

![KubeClusterSnap Overview Dashboard](https://raw.githubusercontent.com/anilkumar2713/KubeClusterSnap-Storefront/master/docs/overview_dashboard.png)

---

## 🌟 Key Capabilities & Features Highlight

### 1. 🚀 Deployment & Container Image Tag Sync Studio
- **Visual Image Tag Registry**: Audit every container image tag (`nginx:latest`, `node:18-alpine`, `redis:alpine`) deployed across all cluster namespaces in a single visual grid.
- **1-Click Image Tag Bumping**: Click any tag badge to update container image versions or SHA digests (`v2.4.0` → `v2.5.0`) interactively with automatic zero-downtime rolling updates.
- **Sync & Drift Reconciliation**: Monitors deployment sync states (**`Synced`** vs **`OutOfSync`**) with a 1-click **`Sync Now`** button to align cluster state without touching YAML files.

![Deployment & Image Sync Studio](https://raw.githubusercontent.com/anilkumar2713/KubeClusterSnap-Storefront/master/docs/deployments_sync_studio.png)

---

### 2. 📦 Cluster Resources & Pod Debugger
- **Pod Health Matrix**: Filter pods instantly by health state (**`All`**, **`Healthy Running`**, **`Crashing/Error`**, **`Pending`**) and namespace.
- **💻 In-Browser Terminal (`kubectl exec`)**: Open interactive full-screen xterm.js TTY terminal sessions inside any running container directly in your web browser over WebSockets.
- **🪵 Live Container Log Inspector**: Stream stdout/stderr logs with real-time keyword filtering.
- **💀 Terminate & Recreate**: 1-click pod deletion (`kubectl delete pod`) to test resilience or unfreeze stuck containers.

![Cluster Resources & Pod Debugger](https://raw.githubusercontent.com/anilkumar2713/KubeClusterSnap-Storefront/master/docs/cluster_resources_studio.png)

---

### 3. 🔌 Services, Ingress, Secrets & Tunnels
- **Services Registry**: Inspect `ClusterIP`, `NodePort`, and `LoadBalancer` services, Cluster IPs, target ports, and selector tags.
- **Ingress Routes Controller**: View exposed host domains, HTTP path maps (`/api` → `backend-svc`), and TLS certificate security status.
- **Secrets & ConfigMaps Security Store**: Inspect ConfigMaps and Secrets with secure **`Reveal Keys`** / **`Hide Keys`** toggle controls.
- **Port Forwards & Tunnels Studio**: Manage active local port-forwarding sessions (`http://localhost:30888`) and close tunnels with one click.

---

### 4. 🗄️ Built-in Visual Database Studio
- **Automated DB Auto-Discovery**: Automatically detects and connects to PostgreSQL, MySQL, Redis, and MongoDB instances across all namespaces.
- **SQL & NoSQL Query Console**: Execute queries, inspect collections, and browse Redis key spaces directly from the dashboard without external desktop apps or port-forwarding setup.

![Visual Database Studio](https://raw.githubusercontent.com/anilkumar2713/KubeClusterSnap-Storefront/master/docs/database_studio_preview.png)

---

### 5. 📸 Volume State Snapshotting & Cloning ("Snap" Engine)
- **1-Click Tarball Snapshotting**: Capture compressed volume snapshot archives from live container data directories (`/data`, `/var/lib/mysql`).
- **Pre-Hydrated Preview Environments**: Instantly launch isolated container environments populated with real volume snapshot data for instant bug reproduction.

---

## 🚀 1-Command Zero-Clone Deployment

You don't even need to clone the git repository to run KubeClusterSnap. Pick your preferred environment below and run a single command:

### Option A: Kubernetes Cluster Deployment (`kubectl`)
```bash
kubectl apply -f https://raw.githubusercontent.com/anilkumar2713/KubeClusterSnap/main/k8s/kubeclustersnap-all-in-one.yaml
```
- Open **`http://localhost:30333`** (or your NodePort/Ingress IP).

---

### Option B: Native Helm Package (`helm`)
```bash
helm install kubeclustersnap https://raw.githubusercontent.com/anilkumar2713/KubeClusterSnap/main/charts/kubeclustersnap-2.7.0.tgz --create-namespace -n kubeclustersnap-system
```

---

### Option C: Standalone Docker Desktop / VM (`docker compose`)
```bash
curl -sSL https://raw.githubusercontent.com/anilkumar2713/KubeClusterSnap/main/docker-compose.prod.yml | docker compose -f - up -d
```
- Open **`http://localhost:30333`**.

---

## 🤝 Join the Community & Get Started

KubeClusterSnap is open for developers and platform engineering teams looking for a faster, visual way to manage Kubernetes workloads.

- ⭐ **Star on GitHub**: [https://github.com/anilkumar2713/KubeClusterSnap](https://github.com/anilkumar2713/KubeClusterSnap)
- 🚀 **Storefront & Artifacts**: [https://github.com/anilkumar2713/KubeClusterSnap-Storefront](https://github.com/anilkumar2713/KubeClusterSnap-Storefront)

Try it out today and let us know what features you'd like to see next!

#Kubernetes #DevOps #CloudNative #Docker #PlatformEngineering #OpenSource #SoftwareEngineering
