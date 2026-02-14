# Kubernetes Deployment

Deployment is a higher-level Kubernetes controller used to manage Pods and ReplicaSets.

It provides:

- replica management
- rolling updates
- rollback support
- scaling
- self-healing

You normally create Deployments, not Pods or ReplicaSets directly.

---

## What Deployment Does

Deployment ensures:

- desired number of Pods are running
- Pods are recreated if deleted
- updates happen gradually (rolling update)
- previous versions can be restored

Control chain:

Deployment → ReplicaSet → Pods

---

## Deployment YAML Example

File: `deployment.yml`
```yml
apiVersion: apps/v1          # API group for Deployment resources
kind: Deployment             # Object type = Deployment controller

metadata:
  name: nginx-deployment     # Name of the Deployment
  namespace: demo-ns         # Namespace where it will be created
  labels:
    app: nginx               # Label attached to the Deployment object

spec:
  replicas: 3                # Desired number of Pod replicas

  selector:                  # Selector used to find Pods managed by this Deployment
    matchLabels:
      app: nginx             # Must match template.labels exactly

  template:                  # Pod template (blueprint for Pods)
    metadata:
      labels:
        app: nginx           # Labels applied to created Pods (must match selector)

    spec:
      containers:            # List of containers inside each Pod
      - name: nginx          # Container name (logical name inside Pod)
        image: nginx:latest  # Container image to run
        ports:
        - containerPort: 80  # Port exposed by container inside Pod
```

## Important Fields
1. `apiVersion: apps/v1` → required API group for Deployments
2. `kind:` Deployment → object type
3. `metadata.name` → deployment name
4. `metadata.namespace` → target namespace
5. `spec.replicas` → number of Pod copies
6. `spec.selector` → matches Pods managed by Deployment
7. `spec.template` → Pod blueprint used for replicas


## Create Deployment
- Validate only (no creation):
```bash
kubectl apply -f deployment.yml --dry-run=client
```
- Create Deployment:
```bash
kubectl apply -f deployment.yml
```
- Create in namespace explicitly:
```bash
kubectl apply -f deployment.yml -n demo-ns
```
## Verify Deployment
- List Deployments:
```bash
kubectl get deployments -n demo-ns
```

## Check ReplicaSets:
```bash
kubectl get rs -n demo-ns
```

## Scale Deployment
- Increase replicas:
```bash
kubectl scale deployment <deployment-name> -n <namespace> --replicas=10
```
- Decrease replicas:
```bash
kubectl scale deployment <deployment-name> -n <namespace> --replicas=1
```

Verify:

kubectl get pods -n <namespace>

## Update Image (Rolling Update)
- Change container image:
```bash
kubectl set image deployment/<deployment-name> nginx=nginx:1.25 -n <namespace>
```
## Kubernetes performs rolling update automatically.

- Watch rollout progress:
```bash
kubectl rollout status deployment/<deployment-name> -n <namespace>
```
- View rollout history:
```bash
kubectl rollout history deployment/<deployment-name> -n <namespace>
```

- Rollback to previous version:
```bash
kubectl rollout undo deployment/<deployment-name> -n <namespace>
```

## Delete Deployment
- Delete by name:
```bash
kubectl delete deployment <deployment-name> -n <namespace>
```
- Delete by file:
```bash
kubectl delete -f deployment.yml
```

## Common Mistakes

### Selector and Label Mismatch

- If these do not match exactly:
  - `spec.selector.matchLabels`
  - `spec.template.metadata.labels`
- Deployment will NOT manage the Pods correctly
- Pods may be created but not controlled
- Scaling and updates will break

Example (must match):

```yml
selector:
  matchLabels:
    app: nginx

template:
  metadata:
    labels:
      app: nginx
```
---

## Study Order

0. [Workload Controller Theory](./00-workload-theory.md)
1. [Deployments](./01-deployments.md)

---

## Official References

[Deployment Concept](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)