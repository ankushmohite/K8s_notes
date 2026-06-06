# Kubernetes Horizontal Pod Autoscaler (HPA)

## What is HPA?

HPA (Horizontal Pod Autoscaler) is a Kubernetes feature that automatically increases or decreases the number of pod replicas based on resource utilization such as CPU or Memory.

### Example
- If CPU utilization exceeds **70%**, HPA automatically creates more pods.
- When utilization decreases, HPA removes extra pods.
- This helps maintain application performance and optimize resource usage.

---

## Why Do We Use HPA?

- Handles increased user traffic automatically.
- Improves application availability.
- Optimizes infrastructure costs.
- Eliminates manual scaling efforts.

---

## What is Metrics Server?

Metrics Server is a Kubernetes component that collects CPU and Memory usage metrics from all nodes and pods in the cluster.

HPA depends on Metrics Server to make scaling decisions.

### Verify Metrics Server

```bash
kubectl get pods -n kube-system | grep metrics-server
```

```bash
kubectl top nodes
kubectl top pods -A
```

### Install Metrics Server

```bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

---

## Prerequisites for HPA

Before creating HPA, configure CPU and Memory Requests/Limits in the Deployment manifest.

### Deployment Example

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 2
  template:
    spec:
      containers:
      - name: my-app
        image: nginx
        resources:
          requests:
            cpu: "100m"
            memory: "128Mi"
          limits:
            cpu: "500m"
            memory: "512Mi"
```

Apply the deployment:

```bash
kubectl apply -f deployment.yaml
```

---

## HPA Configuration Example

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: my-app-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: my-app

  minReplicas: 2
  maxReplicas: 10

  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 70
```

Apply HPA:

```bash
kubectl apply -f hpa.yaml
```

---

## Monitor HPA

```bash
kubectl get hpa -w
```

Or for a specific namespace:

```bash
kubectl get hpa -w -n ecgcfrontenderp
```

---

## Can One HPA Manage Multiple Deployments?

No.

One HPA can target only one Deployment (or StatefulSet).

### Rule

```text
1 Deployment = 1 HPA
```

If there are 10 microservices, create HPA only for services that require autoscaling. Not every service needs HPA.

---

## Real-Time Interview Answer

"HPA stands for Horizontal Pod Autoscaler. It automatically increases or decreases the number of pods based on CPU or Memory utilization. Before implementing HPA, we ensure Metrics Server is installed because HPA uses Metrics Server to collect resource metrics. We configure CPU and Memory requests and limits in the Deployment file, then create an HPA by defining minimum and maximum replicas along with utilization thresholds. For example, if CPU usage goes above 70%, Kubernetes automatically creates additional pods, and when the load decreases, it removes the extra pods. This helps maintain application performance and avoids manual scaling."
