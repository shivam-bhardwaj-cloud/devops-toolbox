## What is Horizontal Pod Autoscaler (HPA)

Horizontal Pod Autoscaler automatically adjusts the **number of pod replicas**
based on resource usage metrics.

HPA monitors metrics collected by the **Metrics Server** and scales the
application horizontally.

Common metrics used for scaling:

- **CPU Utilization**
- **Memory (RAM) Utilization**
- Custom metrics (advanced setups)

### Example Scaling Logic

Target CPU utilization: `50%`

Scenario:
- If pod CPU usage rises to **80%** → Kubernetes **increases replicas**
- If CPU usage drops below the target → Kubernetes **reduces replicas**

This ensures the application:

- Handles increased traffic during high load
- Saves resources when demand decreases

## HPA Architecture
`Traffic` → `Deployment` → `Pods` → `Metrics Server` → `HPA Controller` → `Scale Pods`

## Steps with Commands
### 1. Create Deployment
File : `apache-deployment.yml`
```yml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: apache-deployment

spec:
  replicas: 1

  selector:
    matchLabels:
      app: apache

  template:
    metadata:
      labels:
        app: apache

    spec:
      containers:
      - name: apache
        image: httpd:2.4

        ports:
        - containerPort: 80

        resources:
          requests:
            cpu: 100m
            memory: 128Mi
```
Apply:
```bash
kubectl apply -f apache-deployment.yaml
```
### 2. Expose Service
```bash
kubectl expose deployment apache-deployment --type=NodePort --port=80
```
### 3. Create HPA

File : `apache-hpa.yml`
```yml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler

metadata:
  name: apache-hpa

spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: apache-deployment

  minReplicas: 1
  maxReplicas: 10

  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 50
```
Apply:
```bash
kubectl apply -f apache-hpa.yaml
```
### 4. Check HPA
```bash
kubectl get hpa
```
Example:
```txt
NAME        REFERENCE                    TARGETS   MIN   MAX   REPLICAS
apache-hpa  Deployment/apache-deployment 10%/50%   1     10    1
```
### 5. Generate Load
```bash
kubectl run load-generator \
--image=busybox \
-- /bin/sh -c "while true; do wget -q -O- http://apache-deployment; done"
```
Watch scaling:
```bash
kubectl get hpa -w
```
Pods will increase automatically.

## Quick Recap

Metrics Server
- Collects CPU and memory
- Enables kubectl top

HPA
- Uses metrics server
- Automatically scales pods
- Based on CPU usage