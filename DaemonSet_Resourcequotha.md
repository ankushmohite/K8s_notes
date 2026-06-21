DaemonSet ensure that a copy of a pod run on every node in cluster. it is commonly use for node-level service like collect logs, monitoring agent.

suppose we have 10 node in kubernetes cluster and i want node Exporter on every node for cluster monitoring, insteat of creating 10 separate pods manually, i create one DaemonSet, so Kubernetes automaticallyrun one Node Exporterpod on each node.
