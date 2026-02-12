# Kubernetes Fundamentals — Theory Overview

This file contains theory notes from the fundamentals introduction session before practical labs.  
Goal: fast revision without rewatching long videos.
---
## Kubernetes Architecture Diagram

![Kubernetes Architecture](../Assets/kubernetes-architecture.png)
---

# Why Kubernetes

Modern apps evolved like this:

Monolith → Microservices → Containers → Kubernetes

Containers solved packaging.  
Kubernetes solves **managing containers at scale**.

Kubernetes provides:

- orchestration
- scheduling
- auto healing
- auto scaling
- service discovery
- rolling updates
- desired state management

---

# Monolith vs Microservices

## Monolith
- Single big application
- One deployable unit
- Hard to scale parts independently
- Small change → full redeploy

## Microservices
- App split into small services
- Independent deployment
- Independent scaling
- Better fault isolation

Problem: many containers across many machines → needs orchestration → Kubernetes.

---

# Containers and Docker

Containers package:

- code
- runtime
- dependencies

Docker runs containers well on **one machine**.

Docker does NOT handle:

- multi-node scheduling
- failover
- scaling
- cluster networking

Kubernetes sits above containers and manages them across nodes.

---

# Kubernetes Cluster

A cluster = group of machines (nodes).

Terms used the same way:

- node
- server
- instance
- VM

Cluster has two parts:

- Control Plane (Master)
- Worker Nodes

---

# Control Plane (Master) Components

Control plane manages the cluster.

## API Server
- Entry point to cluster
- kubectl talks here
- All requests pass through it

## Scheduler
- Decides which node runs a Pod

## Controller Manager
- Watches desired vs actual state
- Fixes drift automatically

## etcd
- Key-value database
- Stores cluster state

---

# Worker Node Components

Worker nodes run application workloads.

## Kubelet
- Node agent
- Talks to API server
- Ensures containers are running

## kube-proxy
- Network traffic routing
- Service connectivity

## Container Runtime
- Runs containers
- containerd / Docker compatible runtimes

---

# Kubernetes Networking (High Level)

- Every Pod gets an IP
- Pods communicate over cluster network
- Services give stable access layer
- CNI plugins provide networking

Examples:

- Calico
- Weave
- others

---

# Core Kubernetes Behaviors

## Desired State Model

You declare desired state in YAML.  
Kubernetes keeps trying to match it.

Desired → enforced continuously.

---

## Auto Healing

If container / Pod crashes:

- Kubernetes recreates it
- No manual restart needed

---

## Auto Scaling

Workloads can scale:

- manually
- automatically (metrics based)

---

# Kubernetes Setup Options

Different ways to create clusters:

## Local / Learning

- kubeadm
- minikube
- KIND (Kubernetes in Docker) ✅ used in this course

## Managed Cloud

- AWS EKS
- Azure AKS
- GCP GKE

## Practice Platforms

- Killercoda
- Rancher (management layer)

---

# Kubernetes Short Name

Kubernetes → **K8s**

K + 8 letters + s

---

# Open Source & CNCF

- Kubernetes is open source
- Governed by CNCF
- Large ecosystem
- Vendor neutral

---

# Key Objects (High Level)

## Pod
Smallest deployable unit  
One or more containers

## Service
Stable network endpoint for Pods

## Namespace
Logical isolation inside cluster

## Volume
Persistent storage for Pods

## Ingress
External HTTP/HTTPS routing

---

# Mental Model

Think of cluster like:

- Control Plane = brain
- Workers = muscle
- Pods = running units
- YAML = instruction sheet
- Controllers = auto-repair bots

---

# Practical Notes Index

Jump to detailed files:

- [Kubernetes Theory](README.MD)
- [KIND Installation](./01-kind-installation.md)
- [Cluster Creation](./02-cluster-creation.md)
- [Namespaces](./03-namespaces.md)
- [Pods](./04-pods.md)

---

# Quick Revision

- Kubernetes = container orchestrator
- Cluster = control plane + workers
- API server = entry point
- Scheduler = placement
- Controllers = auto fix
- Kubelet = node agent
- Pods = smallest unit
- Desired state model rules everything