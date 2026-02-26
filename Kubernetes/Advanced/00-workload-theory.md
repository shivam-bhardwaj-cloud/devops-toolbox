# Workload Controllers — Core Theory

This section explains how Kubernetes maintains application state using controllers and label selectors.

Practical YAML examples are in the next files.

## 1. Desired State Model

Kubernetes follows a declarative model.

You declare:
- number of replicas
- container image
- labels
- Pod template

Kubernetes ensures the actual state matches the declared state.

If drift occurs, controllers reconcile it automatically.

Example
```txt
Desired replicas: 3  
Running replicas: 2  
```
Controller creates 1 additional Pod.

No manual intervention.

## 2. Controllers

Controllers are control loops running in the control plane.

They:
- watch cluster state
- compare actual vs desired
- create or delete resources

Examples:

- Deployment controller
- ReplicaSet controller
- Job controller

Controllers enable self-healing.

## 3. Replica

Replica = identical Pod instance created from a template.

If replicas = N → N identical Pods run.

All replicas share:
- same image
- same configuration
- same labels

They are interchangeable units.

## 4. ReplicaSet

ReplicaSet maintains Pod count.

Responsibilities:
```txt
If Pods < desired → create Pods
If Pods > desired → delete Pods
```
> ReplicaSet does not manage rolling updates.

It only enforces replica count.

>Usually managed by a Deployment.

## 5. Deployment

Deployment is a higher-level controller.

Hierarchy:
```txt
Deployment
  → ReplicaSet
      → Pods
```
Responsibilities:
- manage ReplicaSets
- maintain replica count
- perform rolling updates
- support rollback
- update Pod template safely

Users typically create Deployments, not ReplicaSets.

## 6. Labels
Labels are key-value metadata used for selection and grouping.

Example:

```yml
labels:
  app: api
  tier: backend
```

Controllers and Services use labels to select Pods.

Incorrect labels break routing and scaling.

## Study Order
0. [Workload Controller Theory](./00-workload-theory.md)
1. [Deployments](./01-deployments.md)
2. [Services](./02-services.md)
3. [Ingress](./03-ingress.md)