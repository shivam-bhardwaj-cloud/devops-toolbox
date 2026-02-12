# Kubernetes Namespaces

## What is a Namespace

Namespace is a logical partition inside a cluster used to organize and isolate resources.

Used for:

- environment separation
- team separation
- resource grouping

---

## List Namespaces

```bash
kubectl get ns
kubectl get namespaces
```

---

## Create Namespace (YML)

```yml
kind: Namespace
apiVersion: v1
metadata:
  name: my-namespace
```

Apply:

```bash
kubectl apply -f namespace.yml # by file
```
```bash
kubectl create namespace my-namespace # by command
```
Verify:

```bash
kubectl get ns
```

---

## Get Services

```bash
kubectl get service
kubectl get svc
```

## Pro Tip: Context Switching
Instead of typing `-n <namespace>` every time, change your current context:
```bash
kubectl config set-context --current --namespace=my-namespace
```
---

## Quick Revision

- `Namespace` = logical isolation
- `Default namespaces` exist already
- Create via YML or command

# Practical Notes Index

Jump to detailed files:

- [Kubernetes Theory](README.MD)
- [KIND Installation](./01-kind-installation.md)
- [Cluster Creation](./02-cluster-creation.md)
- [Namespaces](./03-namespaces.md)
- [Pods](./04-pods.md)