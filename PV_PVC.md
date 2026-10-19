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



---

apiVersion: v1

kind: Service

metadata:

  name: admin-setting-be

  labels:

    app: admin-setting-be-app

spec:

  selector:

    app: admin-setting-be-app

    tier: middleware

  type: NodePort

  ports:

    - port: 8091

      targetPort: 8091

      nodePort: 30554

 

---

apiVersion: apps/v1

kind: Deployment

metadata:

  name: admin-setting-be

  namespace: ecgcbackend

  labels:

    app: admin-setting-be-app

 

spec:

  replicas: 2

 

  selector:

    matchLabels:

      app: admin-setting-be-app

      tier: middleware

 

#  strategy:

#    type: RollingUpdate

#    rollingUpdate:

#      maxSurge: 1

#      maxUnavailable: 0

#  revisionHistoryLimit: 0

 

  template:

    metadata:

      name: admin-setting-be

      labels:

        app: admin-setting-be-app

        tier: middleware

 

    spec:

 

      affinity:

        nodeAffinity:

          requiredDuringSchedulingIgnoredDuringExecution:

            nodeSelectorTerms:

            - matchExpressions:

              - key: app

                operator: In

                values:

                - middleware

 

      volumes:

      - name: admin-setting-storage

        persistentVolumeClaim:

          claimName: admin-setting-pvc

 

      containers:

      - image: docker-neia-prod.ecgc.in:443/admin-setting-be.jar:R1.0.0_293

        name: admin-setting-be-container

 

        volumeMounts:

        - name: admin-setting-storage

          mountPath: /D:/neia_logs/admin/be

 

        # resources:

        #   requests:

        #     cpu: "100m"

        #     memory: "256Mi"

        #   limits:

        #     cpu: "500m"

        #     memory: "512Mi"

 

        env:

        - name: SPRING_PROFILES_ACTIVE

          value: "prod"

 

        - name: TZ

          value: Asia/Kolkata

 

#      nodeSelector:

#        size: medium

 

      dnsConfig:

        searches:

        - ecgc.svc.cluster.local

        - ecgcbackend.svc.cluster.local

        - ecgcbackendbiz.svc.cluster.local

 

 
