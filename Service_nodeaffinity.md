## Kubernet Service ##

Service is a way to expose our pod within in a cluster or to outside world.
so, Service basically provides a stable IP address and DNS name to access Pods. because pod is dynamic in nature hence ip also dynamic.

cluster Ip: use for internal communication.

NodePort: Use for External communication.


##Node Affinity##

```yaml
affinity:
  nodeAffinity:
    requiredDuringSchedulingIgnoredDuringExecution:
      nodeSelectorTerms:
      - matchExpressions:
        - key: app
          operator: In
          values:
          - middleware
```

##nodeSelector##

```yaml
nodeSelector:
  app: middleware --> Node label.
```

requiredDuringSchedulingIgnoredDuringExecution:

requiredDuringScheduling = This rule is mandatory when Kubernetes schedules the pod.

IgnoredDuringExecution = Once the pod is running, Kubernetes won't move it even if the node label changes later.

nodeSelector: If you want to schedule pod on a specific node based on the specific label.

##### What are the diff. between nodeSelector and nodeAffinity #####

nodeSelector: We can schedule a Pod on single label.
Node Affinity: We can schedule a Pod on large or medium nodes or prevent pod on small nodes using In and NotIn.

## Schedule the Pod on either a large node or a medium node.

matchExpressions:
- key: size
  operator: In
  values:
  - large
  - medium

 ## Do not schedule the Pod on small nodes.

 matchExpressions:
- key: size
  operator: NotIn
  values:
  - small
