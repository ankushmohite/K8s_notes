## Requests_and_Limit.

Requests and Limits are used to control how much CPU and Memory (RAM) a Pod can use.

## Request:
It is the minimum CPU and Memory required by a Pod. Kubernetes uses this value to schedule the Pod on a node that has enough resources.

## Limit:
It is the maximum CPU and Memory a Pod is allowed to use.

#######################################################################################

If a Pod exceeds the CPU limit, then Kubernetes throttles (slows down).

If a Pod exceeds the Memory limit, then Kubernetes killed the container.

```yaml
resources:
  requests:
    memory: "64Mi"
    cpu: "250m"

  limits:
    memory: "128Mi"
    cpu: "500m"
```
