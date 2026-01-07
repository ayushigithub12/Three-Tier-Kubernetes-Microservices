# Cloud Native Todo App

A simple Todo application built to understand containerized and Kubernetes-based deployments.

## Tech Stack
- React (Frontend)
- Node.js & Express (Backend)
- MongoDB (Database)
- Docker
- Kubernetes
- NGINX Ingress

## Features
- Microservice-style deployment using Kubernetes
- Backend and database communication via ClusterIP services
- Configuration and credentials managed using Kubernetes Secrets
- Liveness and readiness probes for health monitoring
- Ingress routing for frontend and backend APIs

## Run Locally on Kubernetes
```bash
kubectl create namespace workshop
kubectl apply -f k8s/ -n workshop
```

## Project Status
- Application successfully deployed on Kubernetes
- Frontend → Backend → MongoDB flow working correctly

## Future Enhancements
- Infrastructure provisioning using Terraform
- Helm chart implementation
- Cloud deployment on AWS (EKS)

## Author
# Ayushi Sharma
