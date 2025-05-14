# Kubernetes Cluster Management with KOPS

This project documents the setup and management of a Kubernetes cluster using **KOPS** on an **Ubuntu 24.04 EC2 instance**. It includes the installation of required tools, explanation of Kubernetes distributions, and practical steps to create a Kubernetes cluster.

---

## 📌 Objectives

- Understand how clusters are managed in Kubernetes
- Learn about different Kubernetes distributions
- Install necessary CLI tools (Python3, AWS CLI, kubectl, kops)
- Set up a Kubernetes cluster using KOPS and AWS

---

## 🧰 Tools Installed

- **Python 3**
- **AWS CLI v2**
- **kubectl**
- **kops**

---

## 📦 Kubernetes Distributions

Kubernetes is an open-source container orchestration platform. Many cloud providers and vendors build their own distributions with added features:

| Distribution | Provider      | Description |
|--------------|---------------|-------------|
| Kubernetes   | Community     | Vanilla Kubernetes |
| OpenShift    | Red Hat       | Enterprise-ready, includes CI/CD and security |
| Rancher      | SUSE          | Multi-cluster management |
| Tanzu        | VMware        | Integrated with VMware products |
| EKS          | AWS           | Managed Kubernetes service |
| AKS          | Azure         | Azure Kubernetes Service |
| GKE          | Google Cloud  | Google Kubernetes Engine |
| DOKS         | DigitalOcean  | Developer-friendly Kubernetes |

---

## 🧪 Local Dev Environments (for testing)

These tools are not suitable for production but great for learning:

- `Minikube`
- `k3s`
- `k3d`
- `MicroK8s`
- `Kind` (Kubernetes in Docker)

---

## 🛠️ Cluster Management Tools

| Tool     | Purpose |
|----------|---------|
| **kops** | Full lifecycle management: create, upgrade, delete |
| **kubeadm** | Manual cluster bootstrap and config |

---

## 🔧 KOPS Practical: Cluster Creation Steps

### 1. Install Tools
```
sudo apt update && sudo apt install -y python3 python3-pip unzip
```
### Install AWS CLI
```
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip && sudo ./aws/install
```
### Install kubectl
```
curl -LO "https://dl.k8s.io/release/$(curl -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl && sudo mv kubectl /usr/local/bin/
```
### Install kops
```
curl -Lo kops https://github.com/kubernetes/kops/releases/latest/download/kops-linux-amd64
chmod +x kops && sudo mv kops /usr/local/bin/
```

### 2. Configure AWS CLI
```
aws configure
```
## 3. Create an S3 Bucket for KOPS state storage
```
aws s3api create-bucket \
  --bucket kops-storage \
  --region eu-north-1 \
  --create-bucket-configuration LocationConstraint=eu-north-1
```
### 4. Create a Kubernetes Cluster
```
export KOPS_CLUSTER_NAME=8scluster.k8s.local
export KOPS_STATE_STORE=s3://kops-storage
kops create cluster \
  --name=$KOPS_CLUSTER_NAME \
  --state=$KOPS_STATE_STORE \
  --zones=eu-north-1a \
  --node-count=1 \
  --node-size=t3.micro \
  --control-plane-size=t3.micro \
  --control-plane-volume-size=8 \
  --node-volume-size=8 \
  --yes
```
## 🧪 Since this is not a production environment, we're using .k8s.local as a domain. In real-world setups, you'd use a public domain managed via Route 53.
## ✅ Outcomes
- **Learned how Kubernetes clusters are managed**

- **Installed required tools and configured AWS**

- **Created a production-style Kubernetes cluster using kops**
