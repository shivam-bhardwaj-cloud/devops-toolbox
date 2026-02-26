# Secrets & ConfigMaps

In this section we will:
- Create a **Secret** for sensitive data
- Create a **ConfigMap** for application configuration
- Inject both into a Deployment
- Debug common startup failures
> All resources must exist in the same namespace.

## Secret — Store Sensitive Data

Secrets are used for:
- Passwords
- Tokens
- Credentials
- Kubernetes stores Secret data in `base64` format.

### 1. Generate Base64 Value
```bash
echo -n "Str0ngPass!" | base64
```
Example output:
```
U3RyMG5nUGFzcyE=
```
---
### 2. Create Secret
file : `db-secret.yml`
```yml
apiVersion: v1
kind: Secret

metadata:
  name: database-secret

type: Opaque

data:
  DB_ROOT_PASSWORD: U3RyMG5nUGFzcyE=
```
- Apply:
```bash
kubectl apply -f db-secret.yml
```
- Verify:
```bash
kubectl get secrets
```
---
## ConfigMap — Store Non-Sensitive Config

Used for:
- Environment variables
- App configuration
- Feature flags

### 1. Create ConfigMap
file : `app-config.yml`
```yml
apiVersion: v1
kind: ConfigMap

metadata:
  name: database-config

data:
  DB_NAME: productiondb
```
- Apply:
```bash
kubectl apply -f app-config.yml
```
- Verify:
```bash
kubectl get configmap
```
## Deployment Using Secret & ConfigMap

file : `database-deployment.yml`
```yml
apiVersion: apps/v1
kind: Deployment

metadata:
  name: database-deployment

spec:
  replicas: 1

  selector:
    matchLabels:
      app: database

  template:
    metadata:
      labels:
        app: database

    spec:
      containers:
      - name: mysql-container
        image: mysql:latest
        ports:
        - containerPort: 3306

        env:
        - name: MYSQL_ROOT_PASSWORD
          valueFrom:
            secretKeyRef:
              name: database-secret
              key: DB_ROOT_PASSWORD

        - name: MYSQL_DATABASE
          valueFrom:
            configMapKeyRef:
              name: database-config
              key: DB_NAME
```
> Make sure Secret and ConfigMap are created before applying the Deployment.
- Apply:
```bash
kubectl apply -f database-deployment.yml
```
- Check Pod:
```bash
kubectl get pods
```

## Common Errors & Debug
### 1. Secret Key Mismatch

Error : `Database is uninitialized and password option is not specified`

Fix Checklist

- Secret name correct?
- Key name correct?
- Base64 encoded properly?
- Same namespace?

Check logs:
```bash
kubectl logs <pod-name>
```
### 2. Secret Not Found

Error : `secret "database-secret" not found`

Check:
```bash
kubectl get secrets
```
### 3. ConfigMap Key Wrong
Error : `configmap "database-config" not found`

Check:
```bash
kubectl describe configmap database-config
```
### 4. Pod `CrashLoopBackOff`

Debug:
```bash
kubectl describe pod <pod-name>
kubectl logs <pod-name>
```
If Secret was created after Deployment:
```bash
kubectl rollout restart deployment database-deployment
```
## Summary

- Secret is base64 encoded, not encrypted by default.
- Never commit real credentials to Git.
- In **production** use `external secret managers`.
- Secret & Deployment must be in same namespace.

### Quick Recap

- Secret → sensitive data
- ConfigMap → non-sensitive config
- Inject using valueFrom
- Debug using logs and describe
- Namespace consistency is critical