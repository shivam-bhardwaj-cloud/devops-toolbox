# Kubernetes Persistent Storage — PV & PVC

Stateful applications (like **MySQL**, **PostgreSQL**) require data persistence.

- Pods are **ephemeral**.
- When a Pod dies → container filesystem is lost.

Persistent storage solves this.

## Why Persistent Volume Is Required

Without persistent storage:
- Pod restart = data loss
- Node failure = data loss
- Scaling down = data loss

Databases require storage independent of Pod lifecycle.

## Core Concepts
### 1. PersistentVolume (PV)
- Cluster-level storage resource
- Created by admin
- Represents actual storage
- Not namespaced

Think: Supply

### 2. PersistentVolumeClaim (PVC)

- Storage request made by user
- Namespaced
- Gets bound to a matching PV

Think: Demand

### 3. Binding Flow

- PVC requests:
- StorageClass
- AccessMode
- Storage size

If a PV matches → Kubernetes binds them.
```txt
PVC  →  PV
```
After binding:
- Pod can mount the PVC
- Data persists independently

## Steps and Commands
### 1. Check StorageClass
```bash
kubectl get storageclass
```
Example:
```txt
standard (default)
```
We will use `standard`.

### 2. Create PersistentVolume
File: `persistentVolume.yml`
```yml

apiVersion: v1
kind: PersistentVolume

metadata:
  name: mysql-pv

spec:
  storageClassName: standard # Must match PVC

  capacity:
    storage: 2Gi

  accessModes:
    - ReadWriteOnce

  persistentVolumeReclaimPolicy: Retain

  hostPath:
    path: /mnt/data # Physical path on worker node
```
- Apply:
```bash
kubectl apply -f persistentVolume.yml
```
- Verify:
```bash
kubectl get pv
```
Status should be:
```txt
Available
```
### 3. Create PersistentVolumeClaim

File: `persistentVolumeClaim.yml`
```yml
apiVersion: v1
kind: PersistentVolumeClaim

metadata:
  name: mysql-pvc
  namespace: mysql

spec:
  storageClassName: standard # Must match PV

  accessModes:
    - ReadWriteOnce

  resources:
    requests:
      storage: 2Gi
```

- Apply:
```bash
kubectl apply -f persistentVolumeClaim.yml
```
- Verify:
```bash
kubectl get pvc -n mysql
```
Expected:
```txt
STATUS: Bound
```
Check PV again:
```bash
kubectl get pv
```
Now:
```
STATUS: Bound
CLAIM: mysql/mysql-pvc
```
Binding successful.

### 4. Use PVC in Deployment

Inside `deployment.yml`:
```yml
# deployment.yml

apiVersion: apps/v1
kind: Deployment

metadata:
  name: mysql-deployment
  namespace: mysql

spec:
  replicas: 1

  selector:
    matchLabels:
      app: mysql

  template:
    metadata:
      labels:
        app: mysql

    spec:
      containers:
        - name: mysql-container
          image: mysql:8.0

          ports:
            - containerPort: 3306

          # Environment variables from Secret
          env:
            - name: MYSQL_ROOT_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: mysql-secret
                  key: MYSQL_ROOT_PASSWORD

          # Mount PVC inside container
          volumeMounts:
            - name: mysql-data
              mountPath: /var/lib/mysql
              # MySQL stores DB files here

      # Attach PVC to Pod
      volumes:
        - name: mysql-data
          persistentVolumeClaim:
            claimName: mysql-pvc
```

- Apply:
```bash
kubectl apply -f deployment.yml
```
- Check Pods:
```bash
kubectl get pods -n mysql
```
Pods should be Running.

### 5. Verify Data Persistence

Check inside worker node:
```bash
docker ps
docker exec -it <worker-container-id> bash
```
Navigate:
```bash
cd /mnt/data
ls
```
You should see:
- ibdata1
- mysql.ibd
- binlog files
- performance_schema

That confirms:

Pod is writing to PV.

## Important Concepts
### Access Modes

1. `ReadWriteOnce (RWO)` : Mounted by one node
2. `ReadOnlyMany (ROX)` : Multiple nodes, read-only
3. `ReadWriteMany (RWX)` : Multiple nodes read/write


## Reclaim Policy

I used:
```txt
Retain
```
Options:

- Retain → Data remains after PVC deletion
- Delete → Storage auto deleted
- Recycle → Deprecated

Retain is safest for production.

### Static vs Dynamic Provisioning

This example = Static provisioning.

Admin manually creates PV.

Dynamic provisioning:

- No PV manually created
- StorageClass automatically provisions volume

Production environments mostly use dynamic provisioning.

## Common Errors & Debug
### 1. PVC Stuck in Pending

Cause:
- No matching PV
- StorageClass mismatch
- Requested size > PV capacity

Check:
```bash
kubectl describe pvc mysql-pvc -n mysql
```
### 2. AccessMode Mismatch

- PVC requests RWX
- PV supports only RWO

Binding will fail.

### 3. StorageClass Mismatch

PVC:
```txt
storageClassName: standard
```
PV:
```txt
storageClassName: fast
```
No binding.

### 4. Namespace Confusion

- PVC is namespaced.
- PV is NOT namespaced.

PVC and Deployment must be in same namespace.

### 5. Data Not Persisting

If using `hostPath`:
- Path must exist on node
- Wrong path = no persistence

## Quick Recap

- PV = actual storage (cluster resource)
- PVC = storage request (namespaced)
- PVC binds to matching PV
- Pod mounts PVC
- Data survives Pod restart
- Reclaim policy controls cleanup
- StorageClass must match
- AccessMode must match

## Mental Model

Think like this:

- PV  = Warehouse
- PVC = Order request
- Pod = Customer

Kubernetes connects order to warehouse automatically.
