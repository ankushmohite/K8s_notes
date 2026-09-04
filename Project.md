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
🔹 Kubernetes
1. What is Kubernetes?

Kubernetes is a container orchestration tool. We use it to deploy and manage applications running in containers.

2. What is a Pod?

A Pod is the smallest deployable unit in Kubernetes. Our application container runs inside a Pod.

3. What is a Deployment?

Deployment manages Pods and maintains the required number of replicas. It also helps with rolling updates.

4. What is a Service?

A Service provides a stable way to access Pods.

7. How do you troubleshoot a failed Pod?

First, I check the Pod status, then describe the Pod and check the logs.

kubectl get pods
kubectl describe pod <pod-name>
kubectl logs <pod-name>
8. What is CrashLoopBackOff?

It means the container is repeatedly starting and crashing.

9. What is ImagePullBackOff?

It means Kubernetes is unable to pull the Docker image from the registry.

10. Scenario: Pod is in ImagePullBackOff. What will you check?

I check the image name, tag, registry, credentials, and Pod events.

11. Scenario: Pod is in CrashLoopBackOff. What will you do?

I check the application logs and Pod events. Then I check configuration, environment variables, dependencies, and resource issues.

kubectl logs <pod-name>
kubectl describe pod <pod-name>
12. Scenario: Application is running, but users cannot access it. What will you check?

I check the Pod, Service, Ingress, and application logs.

Pod → Service → Ingress → Application
13. How do you check Kubernetes resources?
kubectl get all
14. How do you check Pod events?
kubectl describe pod <pod-name>
15. How do you scale a Deployment?
kubectl scale deployment <deployment-name> --replicas=3

It increases or decreases the number of Pods.
