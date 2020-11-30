##Creating PODS

* Documentation
* https://kubernetes.io/docs/concepts/workloads/pods/pod-overview/

Create a new yaml file for pod definition. Use vim for that: 

```vi mypod.yml```

mypod.yml

```
apiVersion: v1
kind: Pod
metadata:
  name: mypod
  labels:
    app: myapp
spec:
  containers:
  - name: myappcontainer
    image: busybox
    command: ['sh', '-c', 'echo Hello! && sleep 3600']

```

Create pod from yaml file: 

```kubectl create -f mypod.yml```

Edit a pod by updating the yaml definiton: 

```kubectl apply -f my-pod.yml```

If you need to edit the pod: 

```kubectl edit pod mypod```

Describe pod: 

```kubectl describe pod mypod```

Delete pod: 

```kubectl delete pod mypod```






