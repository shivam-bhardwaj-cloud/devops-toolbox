# Kubernetes Resource Monitoring (Metrics Server)

Kubernetes does not expose CPU or memory usage by default.

Commands like:
```bash
kubectl top pod
kubectl top node
```
require a component called **Metrics Server**.

Metrics Server collects resource metrics from nodes and exposes them through the Kubernetes API.

These metrics are used by:

- kubectl top
- Horizontal Pod Autoscaler (HPA)
- Kubernetes dashboards

## Metrics Flow
`Node` → `Kubelet` → `Metrics Server` → `Kubernetes API` → `kubectl top / HPA`

Without metrics-server you will see:

> error: Metrics API not available

## Steps with Commands
### 1. Verify Metrics Not Available

Run:
```bash
kubectl top pod
```
Expected:
```txt
error: Metrics API not available
```
This means metrics-server is not installed.

### Step 2 Install Metrics Server

Install official components:
```bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```
Expected output:
```txt
serviceaccount/metrics-server created
clusterrole.metrics-server created
deployment.apps/metrics-server created
service.metrics-server created
```

### 3 Edit Metrics Server Deployment
- Local clusters (KIND / Minikube) require TLS bypass.

Edit deployment:
```bash
kubectl -n kube-system edit deployment metrics-server
```
Find container args and add:
```yml
args:
  - --kubelet-insecure-tls
  - --kubelet-preferred-address-types=InternalIP,Hostname,ExternalIP
```
Example deployment section:
```yml
containers:
- name: metrics-server
  image: registry.k8s.io/metrics-server/metrics-server:v0.7.2

  args:
    - --kubelet-insecure-tls
    - --kubelet-preferred-address-types=InternalIP,Hostname,ExternalIP
    - --cert-dir=/tmp
    - --secure-port=10250
    - --metric-resolution=15s
```
Save and exit.

### 4. Restart Metrics Server

Restart deployment:
```bash
kubectl -n kube-system rollout restart deployment metrics-server
```
Verify pods:
```bash
kubectl get pods -n kube-system
```
### 5. Verify Metrics

Run:
```bash
kubectl top pod
```
or
```bash
kubectl top pod -n online-shop
```
Example output
```txt
NAME                                      CPU   MEMORY
my-app-deployment-5f77594898-jplsm   0m    9Mi
my-app-deployment-5f77594898-ngphq   0m    3Mi
```
Now the cluster is collecting resource metrics.

## Common Errors
### 1. Metrics API not available
- Metrics server not installed.

Fix:
```bash
kubectl apply -f components.yaml
```
### 2. Metrics server CrashLoopBackOff
- Usually TLS problem.

Fix:
```bash
--kubelet-insecure-tls
```
### 3. kubectl top returns nothing
- Metrics server still starting.

Wait:
```bash
kubectl get pods -n kube-system
```