## Understanding Multi-Container Pods

Documentation
* https://kubernetes.io/docs/concepts/cluster-administration/logging/#using-a-sidecar-container-with-the-logging-agent
* https://kubernetes.io/docs/tasks/access-application-cluster/communicate-containers-same-pod-shared-volume/
* https://kubernetes.io/blog/2015/06/the-distributed-system-toolkit-patterns/


Simple multi-container pod with sidecar container:

```
apiVersion: v1
kind: Pod
metadata:
  name: multi-container-pod
spec:
  containers:
  - name: nginx
    image: nginx:1.15.8
    ports:
    - containerPort: 80
  - name: busybox-sidecar
    image: busybox
    command: ['sh', '-c', 'while true; do sleep 30; done;']
    
 ```
