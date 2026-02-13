# KIND + kubectl Installation (Linux)

## What is KIND
KIND (Kubernetes IN Docker) lets you run a local Kubernetes cluster using Docker containers. Useful for learning, testing, and labs without cloud setup.

---

## Requirements
- Linux machine
- Docker installed
- sudo access
- Internet connection

---

## Step 1 — Update Packages

```bash
sudo apt-get update -y
```
## Step 2 — Install Docker
```bash
sudo apt-get install docker.io -y
sudo systemctl enable docker
sudo systemctl start docker
```
Add user to docker group:
```bash
sudo usermod -aG docker $USER
newgrp docker
```
Verify:
```bash
docker ps
```
## Step 3 — Check System Architecture
```bash
uname -m
```
- `x86_64` → AMD64
- `aarch64` → ARM64
## Step 4 — Install KIND
- For AMD64
```bash
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.31.0/kind-linux-amd64
```
- ARM64
```bash
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.31.0/kind-linux-arm64
```

Install:
```bash
chmod +x kind
sudo mv kind /usr/local/bin/
```

Verify:
```bash
kind --version
```

## Step 5 — Install kubectl
- For AMD64
```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
```
- For ARM64
```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/arm64/kubectl"
```

Install:
```bash
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
```
Verify:
```bash
kubectl version
```

## Common Errors
- `Docker permission denied`: → run `newgrp docker` or relogin
- `kubectl not found`: → check `/usr/local/bin` in PATH

# Practical Notes Index

Jump to detailed files:

- [Kubernetes Theory](README.MD)
- [KIND Installation](./01-kind-installation.md)
- [Cluster Creation](./02-cluster-creation.md)
- [Namespaces](./03-namespaces.md)
- [Pods](./04-pods.md)

---
---

## Official References (Version-Sensitive)

Use these official docs if commands, versions, or download links change.

- [KIND Installation Guide](https://kind.sigs.k8s.io/docs/user/quick-start/#installing-from-release-binaries)

- [Kubectl Installation (Linux)](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/)

- [KIND Cluster Configuration](https://kind.sigs.k8s.io/docs/user/configuration/)

- [Pods Configuration](https://kubernetes.io/docs/concepts/workloads/pods/)

- [Namespace Configuration](https://kubernetes.io/docs/tutorials/cluster-management/namespaces-walkthrough/)