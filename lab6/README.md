# lab6

Q1) What is a volume in Kubernetes, and how does it differ from a container's storage?

A volume in Kubernetes is a directory accessible to containers in a Pod, used to store data. Unlike container storage, which is ephemeral and tied to the container's lifecycle, a volume's lifecycle is tied to the Pod, allowing data to persist across container restarts.

Q2) What are the different types of volumes available in Kubernetes? Describe at least three types and their use cases.

1. **emptyDir**: A temporary directory that shares data between containers in a Pod. Data is lost when the Pod is removed.
2. **hostPath**: Mounts a file or directory from the host node’s filesystem into a Pod. Useful for accessing host-specific files.
3. **PersistentVolume (PV)**: A storage resource in the cluster that is independent of any individual Pod. Used with PersistentVolumeClaims (PVCs) to provide durable storage.

Q3) How do PersistentVolumes (PVs) and PersistentVolumeClaims (PVCs) work together in Kubernetes? Explain their relationship and purpose.

PVs are storage resources provisioned by an administrator, while PVCs are requests for storage by users. A PVC binds to a PV that matches its requirements (e.g., size, access modes). This decouples storage provisioning from usage, allowing for dynamic and flexible storage management.

Q4) Create a Pod with an emptyDir volume:
 Write a YAML definition for a Pod that uses an emptyDir volume to share data between two containers within the Pod. Deploy the Pod and verify that the data is shared.

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: emptydir-pod
spec:
    containers:
    - name: container1
        image: busybox
        command: ["/bin/sh", "-c", "sleep 3600"]
        volumeMounts:
        - mountPath: /shared-data
            name: shared-data
    - name: container2
        image: busybox
        command: ["/bin/sh", "-c", "sleep 3600"]
        volumeMounts:
        - mountPath: /shared-data
            name: shared-data
    volumes:
    - name: shared-data
        emptyDir: {}
```

Q5) Set up a Pod with a hostPath volume:
 Define a Pod that mounts a hostPath volume to access files from the host node’s file system. Deploy the Pod and verify that it can read/write to the specified directory on the host.

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: hostpath-pod
spec:
    containers:
    - name: container
        image: busybox
        command: ["/bin/sh", "-c", "sleep 3600"]
        volumeMounts:
        - mountPath: /host-data
            name: host-data
    volumes:
    - name: host-data
        hostPath:
            path: /path/on/host
```

Q6) Deploy a PersistentVolume (PV) and PersistentVolumeClaim (PVC):
 Create a YAML file to define a PersistentVolume of 5Gi with ReadWriteOnce access mode. Then, create a PersistentVolumeClaim requesting 2Gi of storage from this PV. Deploy both resources and verify the PVC is bound to the PV.

```yaml
# persistent-volume.yaml
apiVersion: v1
kind: PersistentVolume
metadata:
    name: pv-example
spec:
    capacity:
        storage: 5Gi
    accessModes:
        - ReadWriteOnce
    hostPath:
        path: /mnt/data

# persistent-volume-claim.yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
    name: pvc-example
spec:
    accessModes:
        - ReadWriteOnce
    resources:
        requests:
            storage: 2Gi
```

Q7) Create a Pod that uses a PVC:
 Write a YAML definition for a Pod that uses the PVC created in Exercise 3. Mount the PVC to a specific path inside the container and test that the storage is accessible.

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: pvc-pod
spec:
    containers:
    - name: container
        image: busybox
        command: ["/bin/sh", "-c", "sleep 3600"]
        volumeMounts:
        - mountPath: /data
            name: pvc-storage
    volumes:
    - name: pvc-storage
        persistentVolumeClaim:
            claimName: pvc-example
```

Q8) Dynamic Provisioning of Persistent Volumes:
 Create a StorageClass that uses a dynamic provisioner (e.g., AWS EBS, GCE Persistent Disk, or NFS). Deploy a PVC that requests storage dynamically using this StorageClass. Verify that the storage is dynamically provisioned.

```yaml
# storage-class.yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
    name: standard
provisioner: kubernetes.io/aws-ebs
parameters:
    type: gp2

# dynamic-pvc.yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
    name: dynamic-pvc
spec:
    storageClassName: standard
    accessModes:
        - ReadWriteOnce
    resources:
        requests:
            storage: 1Gi
```

Q9) Use a configMap as a Volume:
 Create a ConfigMap with some configuration data. Write a Pod YAML definition that mounts this ConfigMap as a volume and verify the data is correctly mounted and accessible inside the container.

```yaml
# configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
    name: config-example
data:
    config-key: config-value

# pod-configmap.yaml
apiVersion: v1
kind: Pod
metadata:
    name: configmap-pod
spec:
    containers:
    - name: container
        image: busybox
        command: ["/bin/sh", "-c", "sleep 3600"]
        volumeMounts:
        - mountPath: /etc/config
            name: config-volume
    volumes:
    - name: config-volume
        configMap:
            name: config-example
```

Q10) Create a Pod with a secret as a Volume:
 Define a Kubernetes Secret containing sensitive data. Create a Pod that mounts this secret as a volume and verify the data is correctly mounted and accessible inside the container in a secure way.

```yaml
# secret.yaml
apiVersion: v1
kind: Secret
metadata:
    name: secret-example
type: Opaque
data:
    secret-key: c2VjcmV0LXZhbHVl

# pod-secret.yaml
apiVersion: v1
kind: Pod
metadata:
    name: secret-pod
spec:
    containers:
    - name: container
        image: busybox
        command: ["/bin/sh", "-c", "sleep 3600"]
        volumeMounts:
        - mountPath: /etc/secret
            name: secret-volume
            readOnly: true
    volumes:
    - name: secret-volume
        secret:
            secretName: secret-example
```

Q11) Set up a Pod with a gitRepo volume:
 Write a YAML definition for a Pod that uses a gitRepo volume to clone a Git repository into the container. Verify that the repository's contents are available inside the container.

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: gitrepo-pod
spec:
    containers:
    - name: container
        image: busybox
        command: ["/bin/sh", "-c", "sleep 3600"]
        volumeMounts:
        - mountPath: /repo
            name: git-volume
    volumes:
    - name: git-volume
        gitRepo:
            repository: "https://github.com/example/repo.git"
            directory: "."
```

Q12) Resize a Persistent Volume Claim (PVC):
 Create a PVC and bind it to a Pod. After deployment, resize the PVC to request more storage (assuming the underlying storage provider supports resizing). Verify that the PVC has been resized successfully.

```yaml
# initial-pvc.yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
    name: resizable-pvc
spec:
    accessModes:
        - ReadWriteOnce
    resources:
        requests:
            storage: 1Gi

# resized-pvc.yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
    name: resizable-pvc
spec:
    accessModes:
        - ReadWriteOnce
    resources:
        requests:
            storage: 2Gi
```

Q13) Use subPath for mounting volumes:
 Create a Pod with a single volume and use the subPath feature to mount different subdirectories of that volume to different paths within a container. Verify that each path in the container corresponds to the correct subdirectory on the volume.

```yaml
apiVersion: v1
kind: Pod
metadata:
    name: subpath-pod
spec:
    containers:
    - name: container
        image: busybox
        command: ["/bin/sh", "-c", "sleep 3600"]
        volumeMounts:
        - mountPath: /data/subdir1
            name: volume
            subPath: subdir1
        - mountPath: /data/subdir2
            name: volume
            subPath: subdir2
    volumes:
    - name: volume
        emptyDir: {}
```
