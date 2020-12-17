## Container Logging

* Documentation
* https://kubernetes.io/docs/concepts/cluster-administration/logging/

A sample pod that generates log output every second:

```
apiVersion: v1
kind: Pod
metadata:
  name: log-counter
spec:
  containers:
  - name: count
    image: busybox
    args: [/bin/sh, -c, 'i=0; while true; do echo "$i: $(date)"; i=$((i+1)); sleep 1; done']

```

Get the container's logs:

```
kubectl logs log-counter
```

For a multi-container pod, specify which container to get logs for using the -c flag:

```
kubectl logs <pod name> -c <container name>
```

Save container logs to a file:

```
kubectl logs logs-counter > counter.log
```