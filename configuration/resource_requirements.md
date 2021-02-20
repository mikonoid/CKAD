## Resource Requirements

Documentation

* https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/#resource-requests-and-limits-of-pod-and-container
* https://kubernetes.io/docs/tasks/administer-cluster/manage-resources/memory-default-namespace/
* https://sysdig.com/blog/kubernetes-limits-requests/

Specify resource requests and resource limits in the container spec like this:

```
apiVersion: v1
kind: Pod
metadata:
  name: my-resource-pod
spec:
  containers:
  - name: myapp-container
    image: busybox
    command: ['sh', '-c', 'echo Hello Kubernetes! && sleep 3600']
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "500m"
    
```
