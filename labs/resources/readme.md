Create an nginx pod with name nginx-resources and with following resources:
* requests cpu 100m, memory 256Mi
* limits cpu 300m, memory 512Mi

Solution: 

```
kubectl run nginx-resources --image=nginx --dry-run=client -o yaml > pod.yaml
vi pod.yaml
```

edit yaml: 

```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: nginx-resources
  name: nginx-resources
spec:
  containers:
  - image: nginx
    name: nginx
    resources:
      requests:
        memory: "256Mi"
        cpu: 100m
      limits:    
        memory: "512Mi"
        cpu: 300m
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```
