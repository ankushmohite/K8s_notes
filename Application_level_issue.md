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

Sometimes ArgoCD does not show INFO logs.We check the logback-spring.xml file to verify if INFO-level logging is configured. If it's missing, we add the entry and restart the pod.


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

When you drain a node, kubernetessafely remove all pods from a node.

```bash
kubectl drain node_name
```

when you cordan a node, kubernetes stop scheduling a new pods on that node but existing pods keep running.

```bash
kubectl cordon node_name
```

