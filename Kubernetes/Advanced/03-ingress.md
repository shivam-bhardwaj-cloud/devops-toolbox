# Kubernetes Ingress — Path Based Routing 

Ingress is a Kubernetes resource that manages external HTTP/HTTPS access to Services inside the cluster.

- It works at Layer 7 (HTTP) and acts like a reverse proxy.

**Traffic Flow**
- `User` → `Ingress Controller` → `Service` → `Pods`

Ingress is only configuration.
It requires an **Ingress Controller** (e.g., NGINX Ingress Controller).

## Why Ingress Is Needed

Without Ingress:
- Each Service must be exposed separately
- Multiple NodePorts or LoadBalancers
- No central routing

With Ingress:
- Single entry point
- Path-based routing
- Host-based routing
- Production-style architecture

Example:

- `/nginx` → nginx-svc
- `/app` → app-svc

## Steps and Commands
### 1. Install NGINX Ingress Controller
```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
```
Verify controller:
```bash
kubectl get pods -n ingress-nginx
```
Make sure the ingress-nginx-controller pod is Running.
Wait until ingress-nginx-controller pod status is Running

### 2. Create Ingress YML File

- File: `ingress.yml`
```yml
apiVersion: networking.k8s.io/v1
kind: Ingress

metadata:
  name: demo-ingress
  namespace: demo-ns

  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
    # Removes /nginx or /app before forwarding to backend

spec:
  ingressClassName: nginx
  # Must match installed ingress controller

  rules:
  - http:
      paths:
      - path: /nginx
        pathType: Prefix
        backend:
          service:
            name: nginx-svc   # Service name
            port:
              number: 82      # Service port (NOT container port)

      - path: /app
        pathType: Prefix
        backend:
          service:
            name: app-svc
            port:
              number: 80
```
### 3. Apply Ingress
```bash
kubectl apply -f ingress.yml
```
- Verify:
```
kubectl get ingress -n demo-ns
```
### 4. Access Ingress (Local Cluster)
- Find controller service:
```bash
kubectl get svc -n ingress-nginx
```
### 5. Port Forward:
```bash
sudo -E kubectl port-forward svc/ingress-nginx-controller -n ingress-nginx 80:80 --address=0.0.0.0
```
Now test:
```
http://localhost/nginx
```
```
http://localhost/app
```
- `If you skip sudo on port 80, Linux will remind you who owns privileged ports.`

## How Path Routing Works

### Request:
```
http://localhost/nginx
```
Ingress:
- matches `/nginx`
- forwards to `nginx-svc` port `82`
- Service routes to `Pods`

### Request:
```
http://localhost/app
```
Ingress:
- matches `/app`
- forwards to `app-svc` port `80`

Routing depends on:
- correct Service name
- correct Service port
- correct labels
- correct namespace

`One mismatch and traffic quietly disappears.`

## Common Errors
- `Ingress Controller Not Installed` : Ingress resource exists but no routing happens.
- `Wrong ingressClassName`: Controller ignores the Ingress.
- `Service Port Mismatch`: Ingress forwards to wrong port.
- `Namespace Mismatch`: Ingress cannot see Services in another namespace.

- `No Endpoints`: Check
```bash
kubectl get endpoints -n demo-ns
```
## Study Order

0. [Workload Controller Theory](./00-workload-theory.md)
1. [Deployments](./01-deployments.md)
2. [Services](./02-services.md)
3. [Ingress](./03-ingress.md)

---

## Quick Revision

- Ingress = HTTP gateway
- Requires Ingress Controller
- Routes traffic using path or host
- Forwards to Services
- Works at Layer 7
- Cleaner than multiple NodePorts

## Official Reference
[Ingress Official Docs](https://kubernetes.io/docs/concepts/services-networking/ingress/)