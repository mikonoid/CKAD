## Volumes

* Documentation
* https://kubernetes.io/docs/concepts/storage/volumes/


This pod mounts a simple emptyDir volume, my-volume, to the container at the path /tmp/storage.

```

apiVersion: v1
kind: Pod
metadata:
  name: volume-pod
spec:
  containers:
  - image: busybox
    name: busybox
    command: ["/bin/sh", "-c", "while true; do sleep 3600; done"]
    volumeMounts:
    - mountPath: /tmp/storage
      name: my-volume
  volumes:
  - name: my-volume
    emptyDir: {}

```