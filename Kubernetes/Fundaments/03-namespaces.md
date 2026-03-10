# Kubernetes Namespaces

## What is a Namespace

Namespace is a logical partition inside a cluster used to organize and isolate resources.

Used for:

- environment separation
- team separation
- resource grouping

---

## Steps with Commands

### 1. List Namespace
```bash
kubectl get ns
kubectl get namespaces
```

---

### 2. Create File : `namespace.yml`
```yml
kind: Namespace
apiVersion: v1
metadata:
  name: my-namespace
```

### 3. Apply Namespace 

```bash
kubectl apply -f namespace.yml # by file
```
```bash
kubectl create namespace my-namespace # by command
```
### 4. Verify Namespace
```bash
kubectl get ns
```
- Expected Ouput
```
NAME                 STATUS   AGE
default              Active   8m39s
my-namespace         Active   5s
kube-node-lease      Active   8m39s
kube-public          Active   8m40s
kube-system          Active   8m40s
local-path-storage   Active   8m16s
```
---

## Pro Tip: Context Switching
Instead of typing `-n <namespace>` every time, change your current context:
```bash
kubectl config set-context --current --namespace=my-namespace
```
---

## Quick Revision

- `Namespace` = logical isolation
- `Default namespaces` exist already
- Create via `YML` or `command`

## Practical Notes Index

- [Kubernetes Theory](README.MD)
- [KIND Installation](./01-kind-installation.md)
- [Cluster Creation](./02-cluster-creation.md)
- [Namespaces](./03-namespaces.md)
- [Pods](./04-pods.md)