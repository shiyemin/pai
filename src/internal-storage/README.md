## PAI Internal Storage

Internal Storage is designed to make database and other stateful applications available in PAI. It leverages `Persistent Volume` (PV) and `Persistent Volume Claim` (PVC) in k8s to decouple pods from the underlying storage technology. For example, the database pod can use the internal storage in the same manner, regardless the storage is based on local disk, NFS, Azure File, or any other file systems.

The default service configuration for internal storage is:

```yaml
internal-storage:
    type: hostPath
    localPath: /paiInternalStorage
    capacity: 10Gi
```

User can override these settings in `services-configuration.yaml`.

## Set up Internal Storage

### hostPath

The default setting will use the host path "/paiInternalStorage", and create `openpai-internal-pv` PV and `openpai-internal-pvc` PVC. To ensure the PV always locates on the master node, it leverages `node affinity` in k8s to select the targeted node.

### NFS

TBD

### Azure Disk, Azure File

TBD

## Use the Internal Storage

Any pod can use the Internal Storage by referring the `openpai-internal-pvc` PVC. For example:

```yaml
apiVersion: v1
kind: Pod
...
spec:
  containers:
  - image: <image-name>
    volumeMounts:
    - name: internal-storage
      mountPath: /data
  volumes:
  - name: internal-storage
    persistentVolumeClaim:
      claimName: openpai-internal-pvc
```

## Delete the Internal Storage

If the internal-storage service is stopped, the PV and PVC will be removed, but the data in the file system will not be deleted. User must clean the data manually.

