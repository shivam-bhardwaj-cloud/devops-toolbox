# Kubernetes Service

A Service in Kubernetes provides a stable network endpoint to access Pods.

Service uses **labels + selectors** to find target Pods.

Pods are dynamic:
- they restart
- IPs change
- replicas scale up/down

Service gives:
- fixed virtual IP
- stable DNS name
- load balancing across Pods

Service connects to Pods using **labels + selectors**

---

## Why Service Is Needed

Without Service:
- Pods are reachable only by Pod IP
- Pod IP changes on restart
- no stable access

With Service:
- single stable endpoint
- traffic auto-distributed to matching Pods
- works with Deployments and scaling

Flow:

Client → Service → Pod (via label selector)

---
## Service YAML Example

File: `service.yml`
```yml
apiVersion: v1                 # Core API group
kind: Service                  # Object type = Service

metadata:
  name: nginx-svc              # Service name
  namespace: nginx-ns          # Target namespace

spec:
  selector:
    app: nginx                 # Must match Pod labels

  ports:
  - protocol: TCP
    port: 82                   # Service port (cluster side)
    targetPort: 80             # Container port inside Pod

  type: ClusterIP              # Default service type
```

## Important Fields
- `selector` → matches Pod labels
- `port` → Service port
- `targetPort` → container port
- `type` → exposure method

Selector must match Pod label exactly or Service routes to nothing and silently judges you.

## Create Service
```bash
kubectl apply -f service.yml
```
## Verify Service
```bash
kubectl get svc -n <namespace>
```
## Check endpoints (which Pods are linked):
```bash
kubectl get endpoints -n <namespace>
```

## Port Forward (Quick Local Test)
```bash
kubectl port-forward svc/nginx-svc -n <namespace> 8080:82
```

## Then open:

http://localhost:8080


If you try binding low ports like 80 or 82 and Linux slaps your hand with permission denied, use `sudo` or pick a `higher port`. Kernel rules, not mine.