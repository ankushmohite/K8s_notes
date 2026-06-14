# Kubernetes Issues

## 1. CrashLoopBackOff

### What is CrashLoopBackOff?

CrashLoopBackOff means the container starts successfully but crashes immediately, and Kubernetes continuously tries to restart it.

So, that time My approach is to check pod status, logs, and events using kubectl commands.

And also one of the reason for CrashLoopBackOff is like sometimes services need to change some configurations. so, we communicate with developer team and add required changes. After updating the configuration, we restart the pod and verify the application.

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

so sometimes ArgoCD does not show service INFO logs, so, at that time we check logback-spring.xml file for INFO Level logs entry putted or not.

If not, then we will add and then restart pod.
