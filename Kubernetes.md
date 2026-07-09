Kubernetes:

Kubernetes is a container orchestration tool used to automate deployment, scaling, and management of containerized applications.
Let's say we have a microservices application with 100 or 1000 containers. Management, scaling, and troubleshooting of all these multiple containers is very problematic and time-consuming, so we use Kubernetes.

Kubernetes is a tool that makes working with multiple containers very easy.

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/5bfe1ddd-6297-4c85-8e12-95b6cd5980cb" />







# Kubernetes cluster:

#################Master#############

## 1. ETCD:
ETCD is a database use to store the data, which will contains all information related to node, pods, Deployments, Services as a key:value.

If you create a Deployment with 3 replicas, ETCD stores that information.

## 2. API Server:
API Server receives all requests from users, such as Every command like kubectl get pods or kubectl apply -f deployment.yaml first goes to the API Server. An also communicate with worker node and other Kubernetes components like the Scheduler, Controller Manager, and Kubelet.

## 3. Controller Manager:
Controller Manager continuously monitor various component of cluster.

Example: Let's say we create a Deployment with 3 replicas. If one Pod crashes, The Controller Manager detects this and creates one more Pod.

## 4. Scheduler:
It's responsible for scheduling pods, it just decide which pods place on which node based on rank or resources on node.

## 5. Kubectl:
kubectl is a command line tool that allow user to communicate with cluster.

#################Worker###############

## 1. Kubelet
Kubelet is an agent that runs on every Worker Node. It's monitoring the status of pods an Communicate with the API Server.

## 2. Kube Proxy:
Kube Proxy manages networking inside the cluster.

############Labels & Selectors#############

## 1. Labels:

Labels are key-value pairs attached to Kubernetes objects like Pods, Deployments, and Services.

We use labels to identify and organize resources.

## 2. Selectors:

Selectors are used to find Pods based on their labels.


##1. Pod: Pod is the smallest deployable unit in Kubernetes in which the container are running.



## Deployment

Deployment is used to deploy and manage your applications. It creates and manages ReplicaSets and also supports rolling updates and rollbacks.

## ReplicaSet

ReplicaSet ensures that the required number of Pods are always running. If a Pod is deleted, a new Pod is automatically created to maintain the desired number of replicas. ReplicaSet also provides an advanced version of labels and selectors called **Set-Based Selectors**, which can match multiple values using different operators.

## ReplicationController

ReplicationController is the older version of ReplicaSet. It maintains the desired number of Pods but supports only simple label selectors (Equality-Based Selectors).
