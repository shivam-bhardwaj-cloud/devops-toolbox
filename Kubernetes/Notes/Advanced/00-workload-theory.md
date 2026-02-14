# Theory (Deployments, ReplicaSets, Labels)

These notes capture the theory explained before Deployment practical labs.

Focus: how Kubernetes maintains the desired number of Pods using controllers and label selectors.

This is concept-first material. Practical YAML comes in the next files.

---

## Desired State Model

Kubernetes works on a **desired state** system.

You declare in YAML:

- how many replicas you want
- which container image to run
- which labels to assign
- how Pods should look

Kubernetes continuously compares:

desired state vs actual state

If they differ, controllers take action to fix it.

### Example

Desired replicas = 3  
Currently running = 2  

Controller automatically creates 1 more Pod.

No manual repair needed.

---

## Controllers in Kubernetes

Controllers are background control loops inside the control plane.

They:

- watch cluster state
- compare against desired state
- create / delete resources to fix drift

Examples of controllers:

- Deployment controller
- ReplicaSet controller
- Job controller
- Node controller

Controllers are the reason Kubernetes is self-healing.

---

## Replica Concept (Clone Model)

Replica = identical copy of a Pod.

If replicas = N → Kubernetes runs N identical Pods.

Each replica has:

- same container image
- same configuration
- same labels
- same template

They are interchangeable units.

Think of replicas as cloned instances from one Pod template.

---

## ReplicaSet — Pod Count Enforcer

ReplicaSet is a controller whose only job is:

Maintain the correct number of Pods.

If Pods < desired → create new Pods  
If Pods > desired → delete extra Pods

ReplicaSet does not handle updates smartly.  
It only maintains count.

You usually do not create ReplicaSets directly.  
Deployments manage them.

---

## Deployment — Higher Level Controller

Deployment sits above ReplicaSet.

Hierarchy:

Deployment  
→ ReplicaSet  
→ Pods

Deployment responsibilities:

- manages ReplicaSets
- controls replica count
- performs rolling updates
- supports rollback
- updates Pod templates safely

Users create Deployments, not ReplicaSets, in most cases.

---

## Labels — Object Identification System

Labels are key-value metadata attached to Kubernetes objects.

Example:

```yaml
labels:
  app: nginx
  tier: frontend
```

## Study Order

0. [Workload Controller Theory](./00-workload-theory.md)
1. [Deployments](./01-deployments.md)
