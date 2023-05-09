# Kubernetes Storage

## Persistent Volumes (PVs)

Persistent Volumes (PVs) are used to provide long-term storage for data in Kubernetes. PVs abstract the details of the underlying storage system, such as NFS, iSCSI, or cloud-based storage like AWS EBS, GCE Persistent Disks, or Azure Disk Storage.

PVs are created by cluster administrators and have a lifecycle independent of any individual Pod that uses them. A PV is a cluster-wide resource, and its size and access modes are defined when it's created. Here's an example of a PV configuration:

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: my-pv
spec:
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  storageClassName: my-storage-class
  nfs:
    path: /path/to/my/nfs-share
    server: nfs-server.example.com
```

## Persistent Volume Claims (PVCs)

Persistent Volume Claims (PVCs) are requests for storage resources made by Pods. A PVC specifies the desired storage size and access mode, and the system matches it with an available PV. When a PVC is bound to a PV, the PV's storage becomes available to the Pod.

PVCs are namespaced resources, and their lifecycle is tied to the namespace they belong to. Here's an example of a PVC configuration:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
  storageClassName: my-storage-class
```

## Storage Classes

Storage Classes are used to define different categories of storage within a cluster. They provide a way for administrators to define the default behavior for dynamically-provisioned PVs, such as the type of storage to use, the reclaim policy, and other storage-specific settings.

Here's an example of a StorageClass configuration:

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: my-storage-class
provisioner: kubernetes.io/aws-ebs
parameters:
  type: gp2
  fsType: ext4
reclaimPolicy: Delete
```

In this example, the StorageClass uses the AWS EBS provisioner and specifies the storage type as `gp2` and the file system as `ext4`. When a PVC requests this StorageClass, a new PV will be dynamically provisioned using these settings.

## StatefulSets for stateful applications

StatefulSets are used to manage stateful applications that require stable network identities and persistent storage. Unlike Deployments, which manage stateless applications, StatefulSets ensure that Pods are created with a unique and stable hostname, such as `web-0`, `web-1`, and so on.

When using a StatefulSet, you typically define a PVC template, which creates a new PVC for each Pod in the StatefulSet. Here's an example of a StatefulSet configuration:

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: web
spec:
  serviceName: "web-service"
  replicas: 3
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
      - name: web-container
        image: my-web-image
        volumeMounts:
        - name: web-data
          mountPath: /var/www/html
  volumeClaimTemplates:
  - metadata:
      name: web-data
    spec:
      accessModes: ["ReadWriteOnce"]
      storageClassName: my-storage-class
      resources:
        requests:
          storage: 1Gi
```

In this example, the StatefulSet creates three replicas of the `web` application. Each replica will have a unique and stable hostname, such as `web-0`, `web-1`, and `web-2`. The PVC template `web-data` is used to create a unique PVC for each replica, which is then mounted at `/var/www/html` in the container.