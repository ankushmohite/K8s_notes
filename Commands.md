```md
kubectl get pods -A | grep reporting

kubectl get node

kubectl get node -o wide

kubectl get ns

kubectl get pods -n kube-system

kubectl get deployment -n ecgcfrontend

kubectl get deployment -n ecgcfrontend neia-ecgc-fe -o yaml

kubectl get deployment -n ecgcbackend neia-dms-be -o yaml | grep dms

## How to restart the pods.

kubectl edit deployment neia-ecgc-fe -n ecgcfrontend

kubectl rollout restart deployment neia-ecgc-fe -n ecgcfrontend

kubectl logs -n ecgcbackend neia-dms-be-996d89df9-5kmhz

kubectl get svc,endpoints -n ecgcbackend

kubectl describe pod neia-dms-be-996d89df9-5kmhz -n ecgcbackend

kubectl get node -o wide | grep

kubectl describe node mdcuatnkek8s02
```
