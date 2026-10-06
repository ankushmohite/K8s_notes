## Readiness Probe

Readiness checks whether the application is ready to receive traffic.

If ready Kubernetes sends traffic to the pod otherwise stops sending traffic to the pod.

If Ready → Kubernetes sends traffic to the pod.

If Not Ready → Kubernetes stops sending traffic to the pod.

initialDelaySeconds: 30
periodSeconds: 10
failureThreshold: 3


Kubernetes waits 30 seconds, then checks the application every 10 seconds.
If the readiness endpoint fails 3 consecutive times, the pod becomes NotReady and the Service stops sending traffic to it.


## Liveness Probe

Liveness checks whether the application is still running properly.

If alive Kubernetes keeps the pod running otherwise restarts the container.

If Alive → Kubernetes keeps the pod running.

If Not Alive → Kubernetes restarts the container.

initialDelaySeconds: 60
periodSeconds: 20
failureThreshold: 3

Kubernetes waits 60 seconds, then checks every 20 seconds.
If the liveness endpoint fails 3 times, Kubernetes restarts the container.

Liveness probe checks whether the application is still running and healthy. If the liveness probe fails repeatedly, Kubernetes restarts the container.

# -------------------------
          # READINESS PROBE
          # -------------------------
          readinessProbe:
            httpGet:
              path: /actuator/health/readiness
              port: 11065
              scheme: HTTP

            initialDelaySeconds: 30
            periodSeconds: 10
            timeoutSeconds: 5
            failureThreshold: 3
            successThreshold: 1

          # -------------------------
          # LIVENESS PROBE
          # -------------------------
          livenessProbe:
            httpGet:
              path: /actuator/health/liveness
              port: 11065
              scheme: HTTP

            initialDelaySeconds: 60
            periodSeconds: 20
            timeoutSeconds: 5
            failureThreshold: 3
            successThreshold: 1
