#DaemonSet

DaemonSet ensure that a copy of a pod run on every node in cluster. it is commonly use for node-level service like collect logs, monitoring agent.

suppose we have 10 node in kubernetes cluster and i want node Exporter on every node for cluster monitoring, insteat of creating 10 separate pods manually, we will create one DaemonSet, so Kubernetes automatically run one Node Exporter pod on each node.



# Resource Quota

Resource Quota is a Kubernetes object used to limit how much CPU, Memory, Storage, Pods, and other resources can be used within a namespace.

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: dev-quota
  namespace: development
spec:
  hard:
    requests.cpu: "4"
    requests.memory: 8Gi
    limits.cpu: "8"
    limits.memory: 16Gi
    pods: "20"
```
