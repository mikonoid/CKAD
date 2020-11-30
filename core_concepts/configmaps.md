## ConfigMaps

* Documentation
* https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/

Example configmap for pod:

```
apiVersion: v1
kind: ConfigMap
metadata:
   name: my-config-map
data:
   Key1: Value
   Key2: anotherValue

```

Get value from configmap within container:

```
apiVersion: v1
kind: Pod
metadata:
  name: my-configmap-pod
spec:
  containers:
  - name: myapp-container
    image: busybox
    command: ['sh', '-c', "echo $(VAR) && sleep 3600"]
    env:
    - name: VAR
      valueFrom:
        configMapKeyRef:
          name: my-config-map
          key: Key1

```

It's also possible get configmap data to container using mounted volume:

```
apiVersion: v1
kind: Pod
metadata:
  name: my-configmap-volume-pod
spec:
  containers:
  - name: myapp-container
    image: busybox
    command: ['sh', '-c', "echo $(cat /etc/config/Key1) && sleep 3600"]
    volumeMounts:
      - name: config-volume
        mountPath: /etc/config
  volumes:
    - name: config-volume
      configMap:
        name: my-config-map
```

Commands for checking out values: 

```
kubectl logs my-configmap-pod

kubectl logs my-configmap-volume-pod

kubectl exec my-configmap-volume-pod -- ls /etc/config

kubectl exec my-configmap-volume-pod -- cat /etc/config/Key1

```