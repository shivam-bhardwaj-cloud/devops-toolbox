# Helm on Kubernetes
## What is Helm

Helm is the package manager for Kubernetes.

Deploying applications in Kubernetes usually requires multiple YAML files such as:
- Deployment
- Service
- ConfigMap
- Secrets
- Ingress

Managing these files manually becomes complex.

Helm solves this by packaging Kubernetes resources into Helm Charts, allowing applications to be installed with a single command.

Example:
```bash
helm install my-app bitnami/nginx
```
### Helm Chart
A package that contains Kubernetes manifests.

### Release

A running instance of a Helm chart in the cluster.

### Helm Repository

A location where Helm charts are stored.

## Find Helm Charts

Most Helm charts are available on Artifact Hub.
```txt
https://artifacthub.io
```
Steps:
- Open ArtifactHub
- Search for an application (nginx, redis, jenkins, prometheus)
- Copy the repository URL
- Install the chart using Helm

## Steps with Commands

### 1. Install Helm
```bash
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
```
Give permission
```bash
chmod 700 get_helm.sh
```
Run installer
```bash
./get_helm.sh
```

Verify installation
```bash
helm version
```
### 2. Add Helm Repository

Example (Jenkins repository)

```bash
helm repo add jenkins https://charts.jenkins.io
```
Check repositories

```bash
helm repo list
```
Example output
```txt
NAME      URL
jenkins   https://charts.jenkins.io
```
### 3. Update Repositories
```bash
helm repo update
```
### 4. Search Charts
```bash
helm search repo <keyword>
```
Example
```bash
helm search repo jenkins
```
### 5. Install Application

General syntax

```bash
helm install <release-name> <chart>
```

Example
```bash
helm install my-app jenkins/jenkins
```
Verify deployment

```bash
kubectl get pods
```
### 6. Check Services
```bash
kubectl get svc
```
### 7. Access Application
Port forward service
```bash
kubectl port-forward svc/<service-name> 8080:8080 --address=0.0.0.0
```
Open browser
```txt
http://localhost:8080
```
### 8. View Secrets (Credentials)

List secrets

```bash
kubectl get secrets
```
Decode secret
```bash
kubectl get secret <secret-name> \
-o jsonpath="{.data.password}" | base64 --decode
```
## Useful Helm Commands
### Install Charts from OCI Registry
Helm can also install charts directly from container registries.

Example
```bash
helm install my-app oci://registry-1.docker.io/bitnamicharts/nginx
```
### Remove Deployment
```bash
helm uninstall <release-name>
```
Example
```bash
helm uninstall my-app
```

### List repositories
```bash
helm repo list
```
List installed releases
```bash
helm list
```
Upgrade release
```bash
helm upgrade <release> <chart>
```
Rollback release
```bash
helm rollback <release> <revision>
```

## Common Errors
### 1. helm: command not found
Helm is not installed.

Check:
```bash
helm version
```
If not installed, run:
```bash
./get_helm.sh
```
### 2. Error: repo not found

The Helm repository was not added.

Fix:
```bash
helm repo add <repo-name> <repo-url>
```
Then update:
```bash
helm repo update
```
### 3. Error: cannot re-use a name that is still in use

A release with the same name already exists.

Check releases:
```bash
helm list
```
Remove the release:
```bash
helm uninstall <release-name>
```
Or install with a different name.

### 4. Pods stuck in Pending

Usually caused by:
- insufficient cluster resources
- storage issues
- node scheduling problems

Check pod details:
```bash
kubectl describe pod <pod-name>
```
### 5. Application not accessible

Service might not be exposed.

Check services:
```bash
kubectl get svc
```
Use port forwarding:
```bash
kubectl port-forward svc/<service-name> 8080:8080
```


## Quick Summary
Helm simplifies application deployment in Kubernetes.

Basic workflow:
```txt
Find Chart → Add Repository → Install Chart → Verify Deployment
```
Typical flow:
```bash
helm repo add <repo-name> <repo-url>
helm repo update
helm search repo <keyword>
helm install <release-name> <chart>
kubectl get pods
```
Helm concepts:
- Chart → Package containing Kubernetes manifests
- Release → Installed instance of a chart
- Repository → Storage location for charts

Helm helps with:
- Faster application deployment
- Managing complex Kubernetes YAML
- Easy upgrades and rollbacks