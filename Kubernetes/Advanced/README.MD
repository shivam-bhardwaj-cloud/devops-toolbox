# Kubernetes Advanced — Study Index

This folder contains advanced Kubernetes topics after fundamentals.  
Focus here is on behavior, configuration, scaling, networking, and production-style features.

Use these notes for revision, lab work, and interview preparation.

---

## Study Order

0. [Workload Controller Theory](./00-workload-theory.md)
1. [Deployments](./01-deployments.md)
2. [Services](./02-services.md)
3. [Ingress](./03-ingress.md)

---

## How To Use These Notes

- Each file = one advanced topic
- Includes YAML + commands + verification
- Includes common mistakes and debug tips
- Ends with quick revision + official references
- Pair notes with lab YAML files in `/labs`

---

## Quick Revision — Advanced Scope

- Deployments manage Pods
- ReplicaSets maintain count
- Labels connect objects
- Selectors choose Pods
- Desired state is enforced
- Controllers auto-heal
- Rolling updates via Deployment
