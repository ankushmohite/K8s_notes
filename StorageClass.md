## StorageClass:

StorageClass defines what type of storage will provide to the application.
For example, in an EKS cluster, I can create a StorageClass for AWS EBS gp3 volumes.
Then i will create PVC, ater that Kubernetes checks the StorageClass and dynamically creates the required storage.
So, we don't need to manually create a PV and EBS volume for every application.

## What is EBS CSI Provisioner?
EBS CSI provisioner is the component that allows Kubernetes to communicate with AWS EBS.

#########################################################################

Step:1 --> Install Amazon EBS CSI Driver.

AWS Console
   ↓
EKS
   ↓
Clusters
   ↓
Select Cluster
   ↓
Add-ons
   ↓
Get more add-ons
   ↓
Amazon EBS CSI Driver

Step:2 --> Create one StorageClass.

apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: ebs-gp3

provisioner: ebs.csi.aws.com

parameters:
  type: gp3
  encrypted: "true"

reclaimPolicy: Retain

volumeBindingMode: WaitForFirstConsumer

allowVolumeExpansion: true

Step:2 --> Create PVC for Payment Service.

apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: payment-pvc
  namespace: ecgcbackend

spec:
  accessModes:
    - ReadWriteOnce

  storageClassName: ebs-gp3

  resources:
    requests:
      storage: 5Gi
