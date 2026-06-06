# Kubernetes HPA (Horizontal Pod Autoscaler) – Interview Notes

## What is HPA?

HPA (Horizontal Pod Autoscaler) is a Kubernetes feature that automatically increases or decreases the number of pods based on CPU, Memory, or custom metrics.

### Example
- If CPU utilization exceeds 70%, HPA automatically creates additional pods.
- When utilization decreases, HPA removes unnecessary pods.
- This helps maintain application performance and optimize resource utilization.

---

## How Did You Implement HPA in AWS EKS?

1. Installed Metrics Server.
2. Configured CPU and Memory requests and limits in the Deployment.
3. Created an HPA resource with minReplicas, maxReplicas, and utilization thresholds.
4. Verified scaling using:

```bash
kubectl get hpa
kubectl describe hpa
kubectl top pods
```

---

## Short Interview Answer (1 Minute)

HPA is used to automatically scale pods in Kubernetes based on CPU, Memory, or custom metrics.

In EKS, I installed Metrics Server, configured resource requests and limits in the Deployment, and created an HPA manifest with minReplicas, maxReplicas, and utilization thresholds.

When application load increased, HPA automatically created more pods, and when the load decreased, it scaled them down.

---

## Why Do We Need Resource Requests?

HPA calculates CPU and Memory utilization based on the resource requests defined in the Deployment.

Without resource requests, Kubernetes cannot accurately calculate utilization percentages, and HPA may not work correctly.

---

## Real-Time Production Example

Normally the application runs with 2 pods.

During peak traffic:
- CPU utilization exceeds 70%
- HPA scales from 2 pods to 4 or 5 pods

When traffic decreases:
- HPA scales back to 2 pods

Benefits:
- High Availability
- Better Performance
- Efficient Resource Utilization

---

## Important Interview Point

- HPA scales Pods.
- Karpenter or Cluster Autoscaler scales Nodes.
- Metrics Server is required for HPA.
