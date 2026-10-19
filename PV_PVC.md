## PV & PVC.

In Kubernetes, PV (Persistent Volume) and PVC (Persistent Volume Claim) is used to store application data permanently, Even if a Pod restarted, the data remains safe. the data will be stored outside the Pod.

Without using PV/PVC, Pod stores its data inside the container. If the Pod recreated the data will be loss, because it is a temporary storage.


## Persistent Volume:

PV is the actual storage available to Kubernetes.

## Persistent Volume Claim:

PVC is a request for that storage made by a Pod.

## Policy:

1.Retain:
Retain means if PVC is deleted, our actual storage and data remain safe.
persistentVolumeReclaimPolicy: Retain

2.Delete
Delete means if PVC is deleted, Kubernetes also deletes the PV and underlying storage.
persistentVolumeReclaimPolicy: Delete

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



