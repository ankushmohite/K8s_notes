# Kubernetes Issues

## 1. CrashLoopBackOff

### What is CrashLoopBackOff?

CrashLoopBackOff means pod starts successfully but crashes immediately, and Kubernetes continuously tries to restart it.

So, at that time we check a pod status, logs, and events using kubectl commands.

And also one of the reason for CrashLoopBackOff is like sometimes services need to  add some configurations. so, we communicate with developer team and add required changes. After adding the configuration, we restart the pod and verify the application.

---

## 2. ImagePullBackOff

### What is ImagePullBackOff?

when Kubernetes is unable to pull the container image from the registry.

sometimes we putted wrong image name or wrong image tags.

---

## 3. Pending Pods

### What are Pending Pods?

Pending state means the pod has been created but Kubernetes cannot schedule it on a worker node.

I usually check resource availability, node selectors, taints, tolerations, and storage-related issues.

---

## 4. Application Logging Issue

I work closely with developers to resolve application-related issues.

Sometimes ArgoCD does not show INFO logs.We check the logback-spring.xml file to verify INFO-level logging is configured. If it's missing, then we configure and restart the pod.


# 1. Diff. between Satic pod and dynamic pods?

Static pods manage by kubelet, such as:

* apiserver
* schedular
* controller-manager

dynamic pods are manage by kubernetes API such as:

* deployment
* replicaset
* statefulset

# 2.How to add new node in k8s?

frist, i generate a join command from the kubernetes master using kubeadm token create.

Then i prepare the new node by installing containerd, kubeadm & kubelet.

After that i execute the join command on the new node. then verify using kubectl get node.

```bash
yum install containerd -y
yum install kubeadm kubectl kubelet
```

# 3. Drain & Cordon.

When you drain a node, kubernetes safely remove all pods from a node.

```bash
kubectl drain node_name
```

when you cordan a node, kubernetes stop scheduling a new pods on that node but existing pods keep running.

```bash
kubectl cordon node_name
```
## how does FE know which BE to call?

Frontend communicates with backend using kubernetes Service name and port. Service forwards the API request to one of the backend Pods.

dnsConfig:
  searches:
    - ecgc.svc.cluster.local
    - ecgcbackenderp.svc.cluster.local

When FE sends a request:

http://erp-ecib-uw-be:11075/api/login

Kubernetes DNS sees:

erp-ecib-uw-be

and because of:

searches:

* ecgcbackenderp.svc.cluster.local

it automatically looks for:

erp-ecib-uw-be.ecgcbackenderp.svc.cluster.local

and reaches that backend service.






## What is RollingUpdate in Kubernetes?
## How do you achieve zero downtime deployment in Kubernetes?

In Kubernetes, we use RollingUpdate. Kubernetes starts to deploy new Pod with the new application version. Once the new Pod is ready, Kubernetes removes an old Pod. Kubernetes repeats this process until all old Pods are replaced.

maxUnavailable: 0
Simply means:
    Kubernetes keep required Pods available until a new Pod is ready.
	
	
maxSurge: 1
Simply means:
    Kubernetes is allowed to create 1 extra Pod during deployment.

maxUnavailable: 1 means:
    Kubernetes allowed 1 Pod unavailable during the update.
	
## Simple example:

Pod-1 → V1 ✅
Pod-2 → V1 ✅
Pod-3 → V1 ✅
Pod-4 → V2 ✅ Ready

Then Kubernetes can remove one old Pod:
Pod-1 → V1 ✅
Pod-2 → V1 ✅
Pod-4 → V2 ✅

It continues until:
Pod-4 → V2 ✅
Pod-5 → V2 ✅
Pod-6 → V2 ✅





# Pod is Working but Application is Not Working?

1. Verify Pod status by using `kubectl get pod` and check the restart count.
   After that, we can check the events of that Pod by using `kubectl describe pod`.

2. We can check application-level logs. Sometimes there might be a database connection failure or missing configuration  changes.

3. Verify the Service port, labels, and selectors. Also verify the Pod CPU and Memory utilization.

## how pod to pod and pod to service communication.

In Kubernetes, every Pod has its own IP address, so Pod-to-Pod communication will happen directly using Pod IPs through the network. But Pod IP is temporary.
pod IP will get change, if the pod restarted.
We use Kubernetes Services:

## Pod-to-Service Communication.

By using labels and selectors.









# 1. How to reduce the size of docker image?

Trying to use lightweight base images like alpine or distroless images.
Second appproch is create multistage Dockerfile, like first stage is to build the application and second stage is to copy the build into the second stage and run it with the help of runtime.

# 2. When you create a pod how does request flow in your k8s archecture?

When I create a Pod using `kubectl`, the request first goes to the **API Server**.

The API Server **authenticates and authorizes** the request and stores the Pod information in **etcd**.

Then **Scheduler selects a suitable worker node**. then Kubelet receives the Pod request and with the help of container runtime ( containerd, dockerd) will creates the Pod and its containers.
**In simple flow:**

`kubectl → API Server → etcd → Scheduler → Kubelet → Container Runtime → Pod`

# 3. Explain one  critical production issue you handel?

One critical production issue I handled was a **502 Bad Gateway error** after a recent deployment.

First, I checked the **ALB** and found that the target was unhealthy. Then I checked the **Ingress** configuration, and there was no issue.

After that, I checked the **pods**, and all pods were running healthy. The Ingress Controller was also working fine.

Then I checked the **Kubernetes Service** and found a **mismatch between the Service selector and Pod labels**. Because of this, the Service was not able to discover the pods.

I corrected the Service selector labels, and the Service started routing traffic to the pods. The ALB targets became healthy, and the **502 error was resolved**.

So, the root cause was a **label mismatch between the Service and Pods**.

and users were able to access the application successfully.

# 4. If some of the service will take high memory consumption.

Whenever memory of any pods get increases instantly, we just check whether it ts a memory leak or not. If required, we restart the service as a temporary solution and then look for the permanent fix.

so by setting with the developer we will check wheter it is any memory leak or any other issue with the application.

In one case, our **Java Spring Boot application** was consuming high memory because it was fetching the **entire database table** instead of fetching data in smaller batches.

So I worked with the **development team**, and they implemented **pagination** so that the application fetches data page by page.

After this change, the **memory consumption was reduced and the issue was resolved**.
