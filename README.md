# Society Management System – Kubernetes Deployment

A containerized Society Management System deployed on a self-managed Kubernetes cluster hosted on AWS EC2.

This project demonstrates containerization, image management, Kubernetes orchestration, service discovery, persistent storage, secrets management, and application deployment.

## Architecture

```text
                 Internet
                    |
                    v
             AWS EC2 Public IP
                    |
                    v
              NodePort :30080
                    |
                    v
              Frontend Service
                    |
                    v
             Frontend Nginx Pod
                    |
                    | API Proxy
                    v
              Backend Service
                    |
                    v
              Backend Pod
                    |
                    v
             MongoDB Service
                    |
                    v
              MongoDB Pod
                    |
                    v
             Persistent Volume
                    |
                    v
                AWS EBS
```

## Technology Stack

| Technology | Purpose |
|---|---|
| AWS EC2 | Kubernetes infrastructure |
| Ubuntu 24.04 | Operating system |
| Kubernetes v1.30 | Container orchestration |
| containerd | Container runtime |
| Docker | Image building |
| Docker Hub | Container image registry |
| Weave Net | Pod networking |
| CoreDNS | Cluster DNS |
| AWS EBS CSI Driver | Persistent storage |
| MongoDB | Database |
| Nginx | Frontend and reverse proxy |

## Kubernetes Cluster

The cluster consists of two nodes:

| Node | Role |
|---|---|
| Master | Control Plane |
| Worker | Application workloads |

## Container Images

- Frontend: `manandock/society-management-system-frontend`
- Backend: `manandock/society-management-system-backend`

## Project Structure

```text
society-management-k8s/
│
├── backend/
│   ├── deployment.yaml
│   └── service.yaml
│
├── database/
│   ├── secret.yaml
│   ├── deployment.yaml
│   ├── service.yaml
│   └── pvc.yaml
│
├── frontend/
│   ├── deployment.yaml
│   └── service.yaml
│
├── namespace.yaml
└── README.md
```

*Adjust this structure to match the actual files in the repository.*

## Features Implemented

- Deployed a multi-container application using Kubernetes.
- Created a dedicated Kubernetes namespace.
- Configured Deployments and Services.
- Used Kubernetes Secrets for database credentials.
- Configured internal service discovery using Kubernetes DNS.
- Deployed MongoDB using StatefulSet.
- Configured persistent storage using PersistentVolumeClaims.
- Integrated AWS EBS through the EBS CSI Driver.
- Exposed the frontend using NodePort.
- Configured Nginx as a reverse proxy for backend API requests.
- Used Docker Hub to distribute container images.

## Useful Kubernetes Commands

Check cluster nodes:

```bash
kubectl get nodes -o wide
```

Check application resources:

```bash
kubectl get all -n society
```

Check persistent storage:

```bash
kubectl get pv,pvc -n society
```

Check application logs:

```bash
kubectl logs deployment/backend -n society
```

Restart a deployment:

```bash
kubectl rollout restart deployment/backend -n society
```

Check rollout status:

```bash
kubectl rollout status deployment/backend -n society
```

## Deployment

Apply the Kubernetes manifests:

```bash
kubectl apply -f namespace.yaml
kubectl apply -f database/
kubectl apply -f backend/
kubectl apply -f frontend/
```

Verify the deployment:

```bash
kubectl get pods,svc,deployments -n society
```

Access the application:

```text
http://<EC2-PUBLIC-IP>:30080
```

Ensure the AWS Security Group allows the required inbound traffic.

## Challenges and Troubleshooting

During deployment, several issues were investigated:

1. Kubernetes DNS resolution between backend and MongoDB.
2. MongoDB Service endpoint verification.
3. CoreDNS troubleshooting.
4. Worker node resource constraints and swap configuration.
5. Kubelet connectivity and log retrieval issues.
6. Frontend Nginx reverse proxy configuration.
7. Backend database connection configuration.

These troubleshooting exercises helped understand how Kubernetes networking, DNS, storage, and application-level connectivity work together.

## What I Learned

- Kubernetes cluster architecture and node management.
- Deployments, Pods, Services, and Namespaces.
- ClusterIP, NodePort, and headless Services.
- Kubernetes DNS and service discovery.
- Secrets and environment variable injection.
- Stateful workloads and persistent volumes.
- AWS EBS CSI integration.
- Container image management with Docker Hub.
- Application troubleshooting using kubectl.
- Nginx reverse proxy configuration.

## Security Notes

- Never commit actual database passwords, API keys, or other secrets.
- Keep sensitive configuration in Kubernetes Secrets or an appropriate external secret manager.
- Restrict public access through AWS Security Groups.
- Rotate any credentials accidentally exposed in logs or Git history.
- Use TLS and appropriate authentication for production deployments.

---

**Project:** Society Management System – Kubernetes Deployment  
**Platform:** AWS EC2  
**Focus:** DevOps | Docker | Kubernetes | AWS
