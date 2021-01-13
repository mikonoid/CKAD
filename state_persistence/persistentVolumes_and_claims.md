## PersistentVolumes and PersistentVolumeClaims

* Documentation

* https://kubernetes.io/docs/concepts/storage/persistent-volumes/
* https://kubernetes.io/docs/tasks/configure-pod-container/configure-persistent-volume-storage/


Create the PersistentVolume:

```
kind: PersistentVolume
apiVersion: v1
metadata:
  name: my-pv
spec:
  storageClassName: local-storage
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/mnt/data"

```

Create the PersistentVolumeClaim:

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  storageClassName: local-storage
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 512Mi

```

We can use 'kubectl' to check the status of existing PVs and PVCs:

```

kubectl get pv
kubectl get pvc

```

Create a pod to consume storage resources using a PVC:

```
kind: Pod
apiVersion: v1
metadata:
  name: my-pvc-pod
spec:
  containers:
  - name: busybox
    image: busybox
    command: ["/bin/sh", "-c", "while true; do sleep 3600; done"]
    volumeMounts:
    - mountPath: "/mnt/storage"
      name: my-storage
  volumes:
  - name: my-storage
    persistentVolumeClaim:
      claimName: my-pvc

```