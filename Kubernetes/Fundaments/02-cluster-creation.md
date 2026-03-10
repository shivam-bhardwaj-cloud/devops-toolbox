# KIND Cluster Creation (Custom Config)

## What This Does

This configuration file creates a KIND Kubernetes cluster with:

- 1 control-plane node
- 3 worker nodes
- Fixed Kubernetes node image version
- Multi-node setup for realistic labs

Useful for testing scheduling, deployments, and scaling behavior.

---

## Steps with Commands

### 1. Create a file: `cluster.yml`

```bash
vim cluster.yml
```
cluster.yml
```yml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4

nodes:
- role: control-plane
  image: kindest/node:v1.30.0

- role: worker
  image: kindest/node:v1.30.0

- role: worker
  image: kindest/node:v1.30.0

- role: worker
  image: kindest/node:v1.30.0
```

### 2. Create Cluster
```bash
kind create cluster --config cluster.yml --name <name_of_cluster>
```

- Verify Cluster

```bash
kind get clusters
```

- Check Kubernetes nodes:

```bash
kubectl get nodes
```

### 3. Delete Cluster
```bash
kind delete cluster --name <cluster-name>
```

## Cluster Access & Context Commands

- Check cluster API info:
```bash
kubectl cluster-info
```
- List available contexts:
```bash
kubectl config get-contexts
```
- Switch context:
```bash
kubectl config use-context kind-kind
```

## Quick Revision


- `KIND cluster` can be customized using YML
- `control-plane` = manager node
- `worker` = runs workloads
- `Use --config` to create custom cluster

## Practical Notes Index

- [Kubernetes Theory](README.MD)
- [KIND Installation](./01-kind-installation.md)
- [Cluster Creation](./02-cluster-creation.md)
- [Namespaces](./03-namespaces.md)
- [Pods](./04-pods.md)