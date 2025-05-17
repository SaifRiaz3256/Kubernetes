# 📦 Kubernetes Pod Example

This repository demonstrates how to define and deploy a **Kubernetes Pod** using a YAML configuration file. It also includes step-by-step instructions on setting up your Kubernetes environment using `kubectl` and `Minikube`.

---

## 📘 What Is a Kubernetes Pod?

A **Pod** is the smallest and simplest unit in the Kubernetes object model. It represents a single instance of a running process in your cluster.

### Key Characteristics:

- A Pod can run **one or more containers**.
- Containers in a Pod share:
  - **Network** (localhost communication)
  - **Storage volumes**
- Pods are designed to run closely related containers (e.g., app + sidecar).
- Each Pod gets its **own IP address** in the cluster.
- Pods are typically defined in **YAML files** and managed using `kubectl`.

---
## 🔄 From `docker run` to Kubernetes YAML

Traditionally, with Docker you would run a container using:
```
docker run -d -p 80:80 nginx:1.14.2
But in Kubernetes, you define this configuration in a YAML file, which is reusable, version-controlled, and more declarative.
```
But in Kubernetes you will have to write all the reuquirments in yaml file.Check out pods.yml for preiveiw.
You can customize this:

Change image: nginx:1.14.2 to any other image (e.g., node:18, python:3.11, or your own)

Change containerPort: 80 to match the port your application runs on

Save this as pod.yaml.
## 🛠️ Prerequisites

### ✅ Install `kubectl` (Kubernetes CLI)
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
kubectl version --client
```
### ✅ Install Minikube (Local Kubernetes Cluster)
```
curl -LO https://github.com/kubernetes/minikube/releases/latest/download/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64
Note: Alternatives include Kind, MicroK8s, and k3s.
```
### ✅ Install Docker (Container Runtime)
```
sudo apt update
sudo apt install -y docker.io
sudo usermod -aG docker $USER
newgrp docker
```
## 🚀 Getting Started
### 1. Start Minikube
```
minikube start
```
### 2. Clone this repository
```
git clone https://github.com/your-username/k8s-pod-example.git
cd k8s-pod-example
```

### 3. Deploy the Pod
```
kubectl create -f pod.yaml
```
### 4. Verify the Pod is running
```
kubectl get pods
kubectl get pods -o wide
```
### 5. Inspect or debug the Pod
```
kubectl describe pod nginx
```
### 6. Access Minikube shell 
```
minikube ssh
```
## 🌐 Accessing the Pod

Pods in Minikube are isolated inside the VM. To test the application:

Get the Pod’s IP
```
kubectl get pod nginx -o wide
```

SSH into Minikube
```
minikube ssh
```

- **Curl the Pod’s IP**


- **If your pod is running nginx, you’ll see the default Nginx welcome page HTML**

## 🧠 Additional Notes
- **Pods are ephemeral—once deleted, they are gone. Use higher-level objects like Deployments for persistence and scaling.**
- **You can define multiple containers in a Pod for sidecar patterns (e.g., a logger or proxy container).**
- **For exposing Pods to the internet or across the cluster, you typically use a Service object.**
