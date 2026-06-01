# DevOps Course Assignment 3: Kubernetes Orchestration & Full DevOps Pipeline

**Course:** DevOps and Cloud Computing Foundation  
**Student Name:** Noor Bibi  
**Roll Number:** f2022-737  
**Submission Date:** June 2026  

---

## 1. Introduction & Objectives
Briefly describe what this assignment accomplishes: containerizing a 3-tier microservice application (Nginx, Flask API, and MySQL) and orchestrating it on a local Kubernetes cluster (Minikube) with a robust GitHub Actions CI/CD pipeline.

---

## 2. System Architecture Diagram

```
                       ┌─────────────────────────┐
                       │    External Client      │
                       └─────────────────────────┘
                                    │
                                    │ (NodePort: 30080)
                                    ▼
                       ┌─────────────────────────┐
                       │   Nginx Service         │
                       │   (Reverse Proxy)       │
                       └─────────────────────────┘
                                    │
                                    │ (Internal ClusterIP: 5000)
                                    ▼
                       ┌─────────────────────────┐
                       │   Flask API Service     │
                       │   (Python Backend)      │
                       └─────────────────────────┘
                                    │
                                    │ (Internal ClusterIP: 3306)
                                    ▼
┌─────────────────┐    ┌─────────────────────────┐
│ Persistent      │───▶│   MySQL Service         │
│ Volume (PV/PVC) │    │   (Relational DB)       │
└─────────────────┘    └─────────────────────────┘
```

*Explain in 2-3 sentences the routing flow between services shown above.*

---

## Part A: Git & Version Control (10 Marks)

### A1. Git Configurations and Repository Setup
*Run the command to check your local Git settings:*
```bash
git config --list
```

### A2. Git History Log
> **Screenshot Required:** Show the output of `git log --oneline --graph --all` with at least 10 meaningful commits across your branching strategy.
> 
> *[Insert Git Log Screenshot Here]*

### A3. Git Branches Listing
> **Screenshot Required:** Show the output of `git branch -a` displaying your `main`, `develop`, and feature branches.
> 
> *[Insert Git Branches Screenshot Here]*

### A4. GitHub Remote Repository Dashboard
> **Screenshot Required:** Show your repository page on GitHub displaying your commit count, branches, and `.gitignore`.
> 
> *[Insert GitHub Repository Screenshot Here]*

---

## Part B: Containerization with Docker (20 Marks)

### B1. Running Docker Containers
> **Screenshot Required:** Show the output of `docker compose ps` (or `docker ps`) indicating that the `nginx`, `flask-api`, and `mysql` containers are running successfully and healthy.
> 
> *[Insert Running Containers Screenshot Here]*

### B2. Docker Custom Bridge Network
> **Screenshot Required:** Show the output of `docker network inspect assignment3-net` showing the containers connected to the bridge network.
> 
> *[Insert Network Inspection Screenshot Here]*

### B3. Volume Persistence Test
Describe how you tested volume persistence by stopping and recreating the containers without losing database items.
> **Screenshot Required:** Show the output of `curl http://localhost/api/items` BEFORE and AFTER running `docker compose down` and `docker compose up -d` to prove the named volume `mysql-data` preserves your data.
> 
> *[Insert Docker Persistence Screenshot Here]*

---

## Part C: CI/CD with GitHub Actions (15 Marks)

### C1. GitHub Actions Execution Dashboard
> **Screenshot Required:** Show the Actions tab in your GitHub repository indicating a green successful run of the `CI/CD Pipeline` workflow.
> 
> *[Insert GitHub Actions Run Screenshot Here]*

### C2. DockerHub Repository
> **Screenshot Required:** Show your repositories dashboard on DockerHub containing both the `flask-api` and `nginx-proxy` images pushed by the CI/CD pipeline.
> 
> *[Insert DockerHub Images Screenshot Here]*

---

## Part D: Kubernetes Orchestration on Minikube (55 Marks)

### D1. Namespace & Active Pods
> **Screenshot Required:** Show the outputs of:
> 1. `kubectl get ns` (showing `assignment3` namespace)
> 2. `kubectl get deployments -n assignment3`
> 3. `kubectl get pods -n assignment3` (showing Nginx, Flask API, and MySQL pods running and ready)
> 
> *[Insert Active K8s Resources Screenshot Here]*

### D2. Services & Networking
> **Screenshot Required:** Show the output of `kubectl get svc -n assignment3` showing Nginx as `NodePort`, and Flask API & MySQL as `ClusterIP`.
> 
> *[Insert K8s Services Screenshot Here]*

**Explain:** What is the difference between ClusterIP, NodePort, and LoadBalancer service types? Why did you select each for these services?
*Answer:*
- **ClusterIP:** ...
- **NodePort:** ...
- **LoadBalancer:** ...

### D3. Persistent Storage (PV & PVC)
> **Screenshot Required:** Show the output of `kubectl get pv,pvc -n assignment3` verifying that both are in `Bound` status.
> 
> *[Insert PV & PVC Bound Screenshot Here]*

### D4. ConfigMaps & Secrets
> **Screenshot Required:** Show the output of `kubectl get configmaps,secrets -n assignment3` and describe how your pods consume them.
> 
> *[Insert ConfigMaps & Secrets Screenshot Here]*

### D5. Scaling, Rolling Updates & Rollback

#### 1. Horizontal Scaling to 3 Replicas
> **Screenshot Required:** Show the output of scaling your Flask API to 3 replicas with `kubectl get pods -n assignment3 -l app=flask-api` showing 3 active pods.
> 
> *[Insert Scaled Replicas Screenshot Here]*

#### 2. Rolling Update
> **Screenshot Required:** Show the status output of your rolling update using `kubectl rollout status deployment/flask-api -n assignment3`.
> 
> *[Insert Rolling Update Screenshot Here]*

#### 3. Rollback
> **Screenshot Required:** Show the rollback output and the active image description using `kubectl describe deployment flask-api -n assignment3 | grep Image`.
> 
> *[Insert Rollback Screenshot Here]*

### D6. End-to-End Verification

#### 1. Access and CRUD Operations
> **Screenshot Required:** Show the terminal output curl commands accessing the application at `http://$(minikube ip):30080/health` and creating/listing items at `/api/items`.
> 
> *[Insert E2E Verification Screenshot Here]*

#### 2. Self-Healing Demonstration
Describe the self-healing test where a Flask pod was deleted and automatically rescheduled.
> **Screenshot Required:** Output showing a pod deletion command followed by `kubectl get pods` demonstrating K8s instantly spinning up a replacement pod.
> 
> *[Insert Self-Healing Screenshot Here]*

#### 3. K8s Data Persistence
Describe the database persistence test under Kubernetes.
> **Screenshot Required:** Show that previously added items are still retrieved by `/api/items` even after the MySQL pod was deleted and auto-recreated.
> 
> *[Insert K8s Storage Persistence Screenshot Here]*
