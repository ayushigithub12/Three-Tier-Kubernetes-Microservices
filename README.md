<div align="center">

<br/>

```
 ██╗  ██╗ █████╗ ███████╗
 ██║ ██╔╝██╔══██╗██╔════╝
 █████╔╝ ╚█████╔╝███████╗
 ██╔═██╗ ██╔══██╗╚════██║
 ██║  ██╗╚█████╔╝███████║
 ╚═╝  ╚═╝ ╚════╝ ╚══════╝
  Three-Tier Microservices on Kubernetes
```

**A full-stack application deployed the cloud-native way — containerized, orchestrated, and production-aware**

<br/>

[![React](https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)](https://reactjs.org/)
[![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)](https://nodejs.org/)
[![Express](https://img.shields.io/badge/Express-000000?style=for-the-badge&logo=express&logoColor=white)](https://expressjs.com/)
[![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=for-the-badge&logo=mongodb&logoColor=white)](https://www.mongodb.com/)
[![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)](https://kubernetes.io/)
[![NGINX](https://img.shields.io/badge/NGINX_Ingress-009639?style=for-the-badge&logo=nginx&logoColor=white)](https://kubernetes.github.io/ingress-nginx/)

<br/>

> *"Not just building an app — deploying it the way real infrastructure teams do."*

<br/>

</div>

---

## 🧭 Table of Contents

- [What This Project Demonstrates](#-what-this-project-demonstrates)
- [Three-Tier Architecture](#-three-tier-architecture)
- [Kubernetes Architecture](#-kubernetes-architecture)
- [Kubernetes Concepts Applied](#-kubernetes-concepts-applied)
- [Request Flow — End to End](#-request-flow--end-to-end)
- [Project Structure](#-project-structure)
- [Running on Kubernetes](#-running-on-kubernetes)
- [Future Roadmap](#-future-roadmap)

---

## 💡 What This Project Demonstrates

This is a **cloud-native deployment showcase** — a three-tier web application that goes beyond just writing code and focuses on *how* applications actually run in modern infrastructure.

| Skill | What it proves |
|---|---|
| 🐳 Containerization | Each tier packaged as its own Docker image |
| ☸️ Kubernetes Orchestration | Deployments, Services, Namespace isolation |
| 🔒 Secrets Management | Credentials handled via K8s Secrets, not hardcoded |
| 🏥 Health Monitoring | Liveness + readiness probes for self-healing pods |
| 🌐 Ingress Routing | NGINX-based traffic routing for frontend & API |
| 🔗 Service Networking | ClusterIP for internal communication between tiers |
| 📦 Namespace Isolation | Clean environment scoping via `workshop` namespace |

---

## 🏗️ Three-Tier Architecture

The application is split into three independent, containerized layers — each with its own responsibility, its own Kubernetes deployment, and its own service.

```
┌─────────────────────────────────────────────────────────────────┐
│                        TIER 1: FRONTEND                          │
│                                                                  │
│   React App  ──►  Docker Image  ──►  K8s Deployment             │
│                                      (frontend pods)            │
│                   Served via NGINX Ingress                       │
└──────────────────────────────┬──────────────────────────────────┘
                               │ HTTP (ClusterIP Service)
┌──────────────────────────────▼──────────────────────────────────┐
│                        TIER 2: BACKEND                           │
│                                                                  │
│   Node.js + Express  ──►  Docker Image  ──►  K8s Deployment     │
│                                              (backend pods)     │
│                   REST API — exposed via ClusterIP               │
└──────────────────────────────┬──────────────────────────────────┘
                               │ MongoDB driver (ClusterIP Service)
┌──────────────────────────────▼──────────────────────────────────┐
│                        TIER 3: DATABASE                          │
│                                                                  │
│   MongoDB  ──►  Docker Image  ──►  K8s Deployment               │
│                                    (database pod)               │
│             Credentials via K8s Secrets (never hardcoded)        │
└─────────────────────────────────────────────────────────────────┘
```

---

## ☸️ Kubernetes Architecture

```
                    ┌──────────────────────────────────────────┐
                    │            KUBERNETES CLUSTER             │
                    │                                          │
                    │   ┌──────────────────────────────────┐  │
                    │   │      Namespace: workshop          │  │
                    │   │                                   │  │
  External          │   │  ┌─────────────────────────────┐ │  │
  Traffic           │   │  │     NGINX Ingress Controller │ │  │
  ──────────────────┼───┼──►  Routes: /        → frontend │ │  │
                    │   │  │           /api/*   → backend  │ │  │
                    │   │  └──────┬──────────────┬────────┘ │  │
                    │   │         │              │           │  │
                    │   │  ┌──────▼──────┐ ┌────▼────────┐ │  │
                    │   │  │  Frontend   │ │   Backend   │ │  │
                    │   │  │  Deployment │ │  Deployment │ │  │
                    │   │  │  (React)    │ │ (Node/Expr) │ │  │
                    │   │  │             │ │             │ │  │
                    │   │  │ ┌─────────┐ │ │ ┌─────────┐│ │  │
                    │   │  │ │  Pod 1  │ │ │ │  Pod 1  ││ │  │
                    │   │  │ │  Pod 2  │ │ │ │  Pod 2  ││ │  │
                    │   │  │ └─────────┘ │ │ └─────────┘│ │  │
                    │   │  │  ClusterIP  │ │  ClusterIP │ │  │
                    │   │  └─────────────┘ └──────┬─────┘ │  │
                    │   │                          │       │  │
                    │   │              ┌───────────▼─────┐ │  │
                    │   │              │ MongoDB Deployment│ │  │
                    │   │              │  (Database Pod)   │ │  │
                    │   │              │                   │ │  │
                    │   │              │  🔒 K8s Secret    │ │  │
                    │   │              │  (credentials)    │ │  │
                    │   │              │   ClusterIP Svc   │ │  │
                    │   │              └───────────────────┘ │  │
                    │   └──────────────────────────────────┘  │
                    └──────────────────────────────────────────┘
```

---

## ☁️ Kubernetes Concepts Applied

### 🚀 Deployments
Each tier runs as a separate Kubernetes `Deployment`. This gives:
- Declarative replica management
- Rolling updates with zero downtime
- Automatic pod restarts on failure

### 🔗 Services (ClusterIP)
Internal communication between tiers uses **ClusterIP services** — stable DNS names that route traffic to the right pods without exposing anything outside the cluster.

```
frontend  →  backend-service:3000   (ClusterIP)
backend   →  mongo-service:27017    (ClusterIP)
```

### 🌐 NGINX Ingress
A single external entry point routes traffic based on path:

```
/          →  frontend service   (React UI)
/api/*     →  backend service    (REST API)
```

### 🔒 Kubernetes Secrets
MongoDB credentials are stored as base64-encoded **Kubernetes Secrets** — injected as environment variables into the backend pod at runtime.

```
# What you DON'T see in the code:
MONGO_USER=admin
MONGO_PASSWORD=supersecret

# What you DO see — clean, safe, and correct:
env:
  - name: MONGO_USER
    valueFrom:
      secretKeyRef:
        name: mongo-secret
        key: username
```

### 🏥 Liveness & Readiness Probes
Every deployment includes health probes so Kubernetes can self-heal without manual intervention:

```
livenessProbe:   Is the container still running?  → Restart if not
readinessProbe:  Is the container ready for traffic? → Route only when yes
```

This means:
- No traffic reaches a pod that hasn't finished starting
- Unhealthy pods are automatically replaced
- Rolling deploys wait for new pods to be ready before removing old ones

---

## 🔀 Request Flow — End to End

Here's what happens when a user opens the app:

```
Browser
  │
  │── GET /  ───────────────────────────────────────────────────►
  │                                                    NGINX Ingress
  │                                                    (routes to frontend)
  │◄── React App (HTML/JS/CSS) ────────────────────────────────
  │
  │  [User submits a form / triggers API call]
  │
  │── POST /api/items ─────────────────────────────────────────►
  │                                                    NGINX Ingress
  │                                                    (routes to backend)
  │                                              Node.js + Express
  │                                              (validates, processes)
  │                                              │
  │                                              │── MongoDB query ──►
  │                                              │              MongoDB
  │                                              │◄── result ─────────
  │                                              │
  │◄── JSON response ──────────────────────────────────────────
```

---

## 📁 Project Structure

```
Three-Tier-Kubernetes-Microservices/
│
├── app/                          # Application source code
│   ├── frontend/                 # React application
│   │   ├── src/
│   │   ├── public/
│   │   └── Dockerfile            # Frontend container image
│   │
│   └── backend/                  # Node.js + Express API
│       ├── index.js
│       ├── routes/
│       └── Dockerfile            # Backend container image
│
├── k8s_manifests/                # All Kubernetes configuration
│   ├── frontend-deployment.yaml  # React deployment + ClusterIP
│   ├── backend-deployment.yaml   # Node.js deployment + ClusterIP
│   ├── mongo-deployment.yaml     # MongoDB deployment + ClusterIP
│   ├── mongo-secret.yaml         # 🔒 DB credentials (K8s Secret)
│   └── ingress.yaml              # NGINX Ingress routing rules
│
└── README.md
```

---

## 🚀 Running on Kubernetes

### Prerequisites

- A running Kubernetes cluster (local: [Minikube](https://minikube.sigs.k8s.io/) / [kind](https://kind.sigs.k8s.io/))
- `kubectl` configured and connected to your cluster
- NGINX Ingress Controller installed

```bash
# For Minikube — enable ingress addon
minikube addons enable ingress
```

### Deploy everything

```bash
# 1. Create an isolated namespace
kubectl create namespace workshop

# 2. Apply all manifests
kubectl apply -f k8s_manifests/ -n workshop
```

### Verify everything is running

```bash
# Check pods are healthy
kubectl get pods -n workshop

# Expected output:
# NAME                        READY   STATUS    RESTARTS   AGE
# frontend-xxx                1/1     Running   0          1m
# backend-xxx                 1/1     Running   0          1m
# mongo-xxx                   1/1     Running   0          1m

# Check services
kubectl get svc -n workshop

# Check ingress
kubectl get ingress -n workshop
```

### Access the app (Minikube)

```bash
minikube ip
# Visit http://<minikube-ip> in your browser
```

### Tear down

```bash
kubectl delete namespace workshop
```

---

## 🗺️ Future Roadmap

- [ ] **Terraform** — provision the cluster infrastructure as code (AWS EKS)
- [ ] **Helm Chart** — package all K8s manifests into a reusable, versioned chart
- [ ] **AWS EKS deployment** — move from local cluster to managed Kubernetes on AWS
- [ ] **Horizontal Pod Autoscaler (HPA)** — scale pods based on CPU/memory load
- [ ] **Persistent Volumes** — ensure MongoDB data survives pod restarts
- [ ] **CI/CD Pipeline** — GitHub Actions to build, push images, and deploy on merge

---

## 👩‍💻 About This Project

This project was built to move past theory and actually **deploy a full-stack app the cloud-native way** — understanding every layer from Dockerfiles to Kubernetes manifests to Ingress routing.

The goal was not just "make it work" but to understand *why* each piece exists: why ClusterIP instead of NodePort, why Secrets instead of env vars in the YAML, why readiness probes before liveness probes.

---

<div align="center">

**Built with a focus on understanding infrastructure, not just writing code.**

⭐ If this helped you learn or think about K8s differently, drop a star!

</div>
