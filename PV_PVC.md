## PV & PVC.

In Kubernetes, PV (Persistent Volume) and PVC (Persistent Volume Claim) are used to store application data permanently, Even if a Pod restarted, the data stored in the PV remains safe.

Without using PV/PVC, Pod stores its data inside the container. If the Pod recreated the data will be loss, because it is a temporary storage.

With PV/PVC, the data will be stored outside the Pod on a separate storage.

## Persistent Volume:

PV is the actual storage available to Kubernetes.

## Persistent Volume Claim:

PVC is a request for that storage made by a Pod.

## admin-setting-pv.yaml

```yaml
apiVersion: v1

kind: PersistentVolume

metadata:

  name: admin-setting-pv

spec:

  capacity:

    storage: 5Gi

  accessModes:

    - ReadWriteOnce

  persistentVolumeReclaimPolicy: Retain

  hostPath:

    path: /data/admin-setting
```

## admin-setting-pvc.yaml

```yaml
apiVersion: v1

kind: PersistentVolumeClaim

metadata:

  name: admin-setting-pvc

  namespace: ecgcbackend

spec:

  accessModes:

    - ReadWriteOnce

  resources:

    requests:

      storage: 5Gi
```
