# KIND Cluster Creation (Custom Config)

## What This Does

This configuration file creates a KIND Kubernetes cluster with:

- 1 control-plane node
- 3 worker nodes
- Fixed Kubernetes node image version
- Multi-node setup for realistic labs

Useful for testing scheduling, deployments, and scaling behavior.

---

## Create Config File

Create a file:

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

## Create Cluster
```bash
kind create cluster --config cluster.yml --name <name_of_cluster>
```

## Verify Cluster

Check KIND clusters:

```bash
kind get clusters
```

Check Kubernetes nodes:

```bash
kubectl get nodes
```

## Delete Cluster
```bash
kind delete cluster
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