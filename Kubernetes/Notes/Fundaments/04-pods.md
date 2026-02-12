# Kubernetes Pod (Basic YML + Commands)

## What is a Pod

Pod is the smallest deployable unit in Kubernetes.  
It wraps one or more containers and provides:

- shared network
- shared storage
- single IP
- same lifecycle

Usually Pods are not created directly in production. Deployments typically manage Pods. For learning and debugging, creating Pods directly is useful.

---

## Pod YML Example

File: `pod.yml`

```yml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80
```
## Create Pod
```bash
kubectl apply -f pod.yml
```
- Create Pod in Namespace
```bash
kubectl apply -f pod.yml -n my-namespace
```
## Get Pods
```bash
kubectl get pods
kubectl get pods -n <namespace>
kubectl get pods -o wide
kubectl get pods --show-labels
kubectl get pods -A # All pods
```
## Describe Pod (Important for Debugging)
- Shows events, status, node placement, and container details.
```bash
kubectl describe pod nginx
```
## Debugging
- View Logs
```bash
kubectl logs nginx
```
- Follow Logs (live)
```bash
kubectl logs -f nginx
```
- Exec Into Container
```bash
kubectl exec -it nginx -- sh # Using Shell
```
```bash
kubectl exec -it nginx -- bash # Using Bash
```
## Expose Pod as Service (Quick Test)
```bash
kubectl expose pod nginx --type=NodePort --port=80
```
```bash
kubectl get svc
```
## Delete Pod
```bash
kubectl delete pod nginx # Using Pod 
```
```bash 
kubectl delete -f pod.yml # Using File
```
## Common Errors
- `ImagePullBackOff`: Wrong image name or registry access issue

- `CrashLoopBackOff`: Container keeps starting and failing
- `Pending`:Scheduler cannot place pod (resource or node issue)

## Quick Revision
- `Pod` = smallest deployable Kubernetes unit
- Usually created by `Deployments`
- Create with `kubectl apply`
- Inspect with `describe`
- Debug with `logs` and `exec`
- Remove with `kubectl delete`