# 🚀 Enterprise DevOps Homelab Platform on Proxmox

A multi-node Platform Engineering and DevOps homelab built on Proxmox VE, Kubernetes, GitOps, Infrastructure-as-Code, Monitoring, and Cloud-Native application deployment.
This environment simulates a production-style infrastructure platform with clustered virtualization, distributed Kubernetes workloads, automated deployments, service discovery, observability, and infrastructure management.

---

# 📌 Overview

The homelab is designed to provide hands-on experience with enterprise infrastructure operations, Kubernetes administration, GitOps workflows, CI/CD automation, monitoring, and platform engineering practices.

Core capabilities include:

* Multi-node Proxmox Cluster
* Kubernetes Cluster (kubeadm)
* GitOps with ArgoCD
* CI/CD with GitHub Actions
* Infrastructure as Code with Terraform
* Monitoring with Prometheus and Grafana
* Load Balancing with MetalLB
* Ingress Management with Traefik
* Internal DNS with AdGuard Home
* Secure Remote Access with Tailscale
* CMDB with Kubernetes Discovery
* Cloud-Native Application Hosting

The environment hosts multiple production-style applications and supporting infrastructure services while demonstrating real-world DevOps and Platform Engineering workflows.


---

# 🏗️ Current Architecture

> **Security Note:** Internal IP addresses and management endpoints are intentionally anonymized for public sharing.


```text
                           GitHub
                              │
                              ▼
                    GitHub Actions CI/CD
                              │
                              ▼
                         Docker Hub
                              │
                              ▼
                           ArgoCD
                              │
                              ▼
                     Kubernetes Cluster
                              │
      ┌───────────────────────┼───────────────────────┐
      ▼                       ▼                       ▼
 Portfolio App          Zen Ecommerce           MyRecipe
                              │
                              ▼
                            CMDB
                              │
                              ▼
                    Traefik Ingress Controller
                              │
                              ▼
                    MetalLB Load Balancer
                              │
                       <metallb-loadbalancer-ip>
```

## Virtualization Layer

```text
Proxmox Cluster (homelab)

PVE01 (<pve01-management-ip>)
├── mgmt01 (QDevice)
├── adguard-home
├── k8-master-homelab
└── devops-tools

PVE02 (<pve02-management-ip>)
├── k8-wk1-homelab
└── k8-wk2-homelab

External Quorum Witness
└── mgmt01 (Corosync QDevice)
```

---

# 🖥️ Infrastructure

| Component           | Details              |
| ------------------- | -------------------- |
| Hypervisor Platform | Proxmox VE Cluster   |
| Cluster Name        | homelab              |
| Node 1              | GMKTec Mini PC       |
| CPU                 | Intel Core i5-13500H |
| RAM                 | 32 GB                |
| Storage             | 1 TB NVMe SSD        |
| Node 2              | Beelink Mini PC      |
| CPU                 | AMD Ryzen 7 (8C/16T) |
| RAM                 | 24 GB                |
| Storage             | 512 GB NVMe SSD      |
| QDevice             | mgmt01               |
| Total RAM           | 56 GB                |
| Total Storage       | 1.5 TB SSD           |

## Kubernetes Topology

| Node              | Role          | Host  |
| ----------------- | ------------- | ----- |
| k8-master-homelab | Control Plane | PVE01 |
| k8-wk1-homelab    | Worker        | PVE02 |
| k8-wk2-homelab    | Worker        | PVE02 |

This design distributes Kubernetes workloads across multiple physical Proxmox hosts, reducing single points of failure and providing a more realistic production-style architecture.

---

# 🧩 VM Layout

| VM Name           | Network Reference | Purpose                  |
| ----------------- | -------------- | ------------------------ |
| Proxmox Host      | <pve01-management-ip> | Hypervisor               |
| mgmt01            | <mgmt01-ip> | kubectl / Ansible / Helm |
| k8-master-homelab | <k8-control-plane-ip> | Kubernetes Control Plane |
| k8-wk1-homelab    | <k8-worker-01-ip> | Kubernetes Worker        |
| k8-wk2-homelab    | <k8-worker-02-ip> | Kubernetes Worker        |
| devops-tools      | <devops-tools-ip> | Future tooling/services  |

---

# ☸️ Kubernetes Stack

## Core Components

* Kubernetes v1.30.x
* containerd
* kubeadm
* Calico CNI
* CoreDNS

## GitOps

* ArgoCD
* Auto-sync enabled
* Self-heal enabled

## Networking

* MetalLB
* Traefik Ingress Controller
* Internal DNS routing

## Monitoring

* Prometheus
* Grafana
* Alertmanager
* node-exporter
* kube-state-metrics

## Security

* cert-manager
* Internal Root CA
* Trusted HTTPS certificates
* Tailscale remote access

---

# 🔄 CI/CD Pipeline

GitHub Actions automatically:

1. Builds Docker image
2. Pushes image to Docker Hub
3. Updates Kubernetes manifests
4. Triggers ArgoCD sync
5. Deploys rolling updates to Kubernetes

---

# 🐳 Docker Image

```text
zaid/flask-app-portfolio
```

---

# 📂 Repository Structure

```text
.
├── .github/
│   └── workflows/
│       └── flask-app-portfolio.yml
│
├── flask_app_portfolio/
│   ├── Dockerfile
│   ├── app.py
│   ├── requirements.txt
│   ├── templates/
│   └── static/
│
├── k8s-apps/
│   └── flask_app_portfolio/
│       ├── deployment.yaml
│       └── service.yaml
│
├── ingress/
│   ├── flask-portfolio-ingress.yaml
│   ├── argocd-ingress.yaml
│   └── grafana-ingress.yaml
│
├── cert-manager/
│   ├── portfolio-certificate.yaml
│   ├── argocd-certificate.yaml
│   └── grafana-certificate.yaml
│
├── terraform/
│   ├── main.tf
│   ├── provider.tf
│   └── variables.tf
│
└── README.md
```

---

# 🌐 Current Service URLs

| Service         | URL                       |
| --------------- | ------------------------- |
| Flask Portfolio | https://portfolio.lab     |
| ArgoCD          | https://argocd.lab        |
| Grafana         | https://grafana.lab       |
| Proxmox VE      | https://<proxmox-management-url>:8006 |

---

# 🔐 Internal PKI

This homelab uses:

* cert-manager
* Internal Root CA
* Trusted TLS certificates

This removes browser certificate warnings for internal `.lab` domains.

---

# 🛠️ Key Technologies

| Category          | Technologies         |
| ----------------- | -------------------- |
| Virtualization    | Proxmox VE           |
| IaC               | Terraform            |
| Config Management | Ansible              |
| Container Runtime | containerd           |
| Orchestration     | Kubernetes           |
| GitOps            | ArgoCD               |
| CI/CD             | GitHub Actions       |
| Registry          | Docker Hub           |
| Monitoring        | Prometheus + Grafana |
| Ingress           | Traefik              |
| Load Balancing    | MetalLB              |
| Remote Access     | Tailscale            |
| TLS               | cert-manager         |

---

# 📈 Future Enhancements

Planned improvements:

* Longhorn distributed storage
* Loki centralized logging
* Velero Kubernetes backups
* Tailscale Kubernetes Operator
* ExternalDNS
* SSO with Keycloak/Authentik
* Harbor private registry
* Trivy security scanning
* Kubernetes HA control plane

---

# 📚 Key Learning Outcomes

This homelab provides practical experience in:

* Infrastructure-as-Code
* Kubernetes administration
* GitOps workflows
* CI/CD automation
* Cloud-native networking
* Kubernetes ingress
* TLS certificate management
* Monitoring & observability
* Production-style platform engineering


## 🎥 Homelab Walkthrough Video

https://youtu.be/OKY7_zPGjRg

---

# 👨‍💻 Author

**Zaid Lazim**

* Linux, Cloud & DevOps Enthusiast
* Platform Engineering Homelab Builder
* Kubernetes & Automation Learner

GitHub:

```text
https://github.com/zaidlaz
```

LinkedIn:

```text
https://www.linkedin.com/in/zaidlaz
```

---

# ⭐ Acknowledgements

This project was built as part of an ongoing hands-on DevOps and Platform Engineering learning journey using open-source technologies and enterprise-inspired architecture.
