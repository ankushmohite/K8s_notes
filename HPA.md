# Kubernetes Horizontal Pod Autoscaler (HPA)

## What is HPA?

**HPA (Horizontal Pod Autoscaler)** automatically increases or decreases the number of pods in a Kubernetes Deployment based on resource utilization such as **CPU** or **Memory**.

For example, if CPU utilization exceeds **70%**, HPA automatically increases the number of pods. When utilization decreases, HPA removes the extra pods.

---

## How to Install HPA

First, ensure that **Metrics Server** is installed because HPA requires metrics data to make scaling decisions.

Then, configure **CPU and Memory Requests and Limits** in the Deployment manifest.

After that, create an HPA with:
- Minimum replica count
- Maximum replica count
- CPU and/or Memory utilization thresholds

---

## Important Notes

- One HPA can target only one Deployment.
- If we have 10 services, it depends on which services require autoscaling.
- HPA is usually enabled only for high-traffic or resource-sensitive services.

### Rule

1 Deployment = 1 HPA

Not every service needs HPA.

---

## How to Implement HPA

### Step 1: Verify Metrics Server

```bash
kubectl get pods -n kube-system | grep metrics-server
kubectl top nodes
kubectl top pods -A
```

### Step 2: Install Metrics Server

```bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

## What is Metrics Server?

Metrics Server is a Kubernetes component that collects CPU and Memory usage metrics from all nodes and pods in the cluster.

HPA uses these metrics to automatically scale applications based on resource consumption.

## Deployment Configuration

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
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

```bash
kubectl apply -f deployment.yaml
```

## Create HPA

apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: erp-hrd-emp-mgmt-be-hpa
  namespace: ecgcbackend

spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: erp-hrd-emp-mgmt-be

  minReplicas: 2
  maxReplicas: 10

  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 70

    - type: Resource
      resource:
        name: memory
        target:
          type: Utilization
          averageUtilization: 80
```

```bash
kubectl apply -f hpa.yaml
```

## Watch HPA Scaling

```bash
kubectl get hpa -w -n ecgcfrontenderp
kubectl get hpa
```

## Summary

- HPA automatically scales pods based on CPU or Memory utilization.
- Metrics Server is mandatory for HPA.
- Resource Requests and Limits must be configured in the Deployment.
- One HPA targets one Deployment.
- HPA is recommended for high-traffic and resource-sensitive applications.
- Not every service requires HPA.
