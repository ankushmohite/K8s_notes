## Project Flow.

First, the developer pushes the code to Git. Jenkins picks up the code and performs the CI process. It builds and compiles the code, runs the unit test cases, creates a Docker image, and pushes that image to the Docker registry.

We keep our Kubernetes manifest files separately in Git for each environment. When we update the manifest with the required image version, Argo CD detects the change and deploys it to the Kubernetes cluster. Kubernetes pulls the image from the Docker registry and runs the application inside the Pods.

we use ELK for centralized logging. So, the application logs are collected and stored in Elasticsearch, and we use Kibana to search and analyze the logs. This helps us troubleshoot application and deployment-related issues.

We use Prometheus and Grafana for monitoring the Kubernetes cluster and application metrics. Prometheus collects the metrics, and Grafana is used to visualize those metrics through dashboards.


# The Request Flow in Kubernetes.

When user try to visit your webpage, it first comes to the Load Balancer. The Load Balancer forwards the request to the Ingress. Ingress routes the request to the appropriate Service. The Service forwards the request to one of the available Pods, and the application inside the Pod processes the request and sends the response back.

User
 ↓
Load Balancer
 ↓
Ingress
 ↓
Service
 ↓
Pod
 ↓
Application



## DevOps Interview Q&A — ECGC Smile Project

# Kubernetes

## 1. What is Kubernetes?

Kubernetes is a container orchestration tool. We use it to deploy and manage applications running in containers.

## 2. What is a Pod?

A Pod is the smallest deployable unit in Kubernetes. Our application container runs inside a Pod.

## 3. What is a Deployment?

Deployment manages Pods and maintains the required number of replicas. It also helps with rolling updates.

## 4. What is a Service?

A Service provides a stable way to access Pods.

## 5. How do you troubleshoot a failed Pod?

First, I check the Pod status, then describe the Pod and check the logs.

```bash
kubectl get pods
kubectl describe pod
kubectl logs
```

## 6. What is CrashLoopBackOff?

It means the container is repeatedly starting and crashing.

## 7. What is ImagePullBackOff?

It means Kubernetes is unable to pull the Docker image from the registry.

## 8. Scenario: Pod is in ImagePullBackOff. What will you check?

I check the image name, tag, registry, credentials, and Pod events.

## 9. Scenario: Pod is in CrashLoopBackOff. What will you do?

I check the application logs and Pod events. Then I check configuration, environment variables, dependencies, and resource issues.

```bash
kubectl logs
kubectl describe pod
```

## 10. Scenario: Application is running, but users cannot access it. What will you check?

I check the Pod, Service, Ingress, and application logs.

```text
Pod → Service → Ingress → Application
```

## 11. How do you check Kubernetes resources?

```bash
kubectl get all
```

## 12. How do you check Pod events?

```bash
kubectl describe pod
```

## 13. How do you scale a Deployment?

```bash
kubectl scale deployment --replicas=3
```

It increases or decreases the number of Pods.




# GitOps and Argo CD Interview Questions

## 1. What is GitOps and how does it work?

GitOps is a deployment approach where Git is used as the single source of truth for Kubernetes configuration. Any change is first made in Git, and a tool like Argo CD detects the change and applies it to the Kubernetes cluster.

---

## 2. What is Argo CD and how does it work?

Argo CD is a GitOps continuous delivery tool for Kubernetes. It continuously monitors the Git repository and compares the configuration in Git with the Kubernetes cluster. If there is a difference, Argo CD can sync the changes and bring the cluster to the desired stat

---

## 3. Explain your Jenkins + Argo CD workflow.

This is very important because the interviewer may ask about your real project.

In our project, Jenkins is mainly used for CI. Developer pushes code to Git, Jenkins builds and tests the application and creates a Docker image. The image is pushed to the container registry. Then the Kubernetes manifest is updated with the new image version and committed to Git. Argo CD monitors the manifest repository and detects the change. It then syncs the manifest with the Kubernetes cluster and deploys the new version.

### Workflow

```text
Developer
   ↓
  Git
   ↓
Jenkins → Build/Test → Docker Image → Registry
   ↓
Manifest Update → Git
   ↓
Argo CD
   ↓
Kubernetes
