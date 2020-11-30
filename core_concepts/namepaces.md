##NAMESPACES

*Documentation
*https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/

List of namespaces:

```kubectl get namespaces```

Create your own namespace:

```kubectl create ns mynamespace```

Now you can assign a custom namespace for object like a pod:


```
apiVersion: v1
kind: Pod
metadata:
  name: mypod
  namespace: mynamespace
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

And use flag "-n" for commands:

```kubectl get pods -n mynamespace```

Also you can use flag "--all-namespaces" for list of object from all namespaces:

```kubectl get pods --all-namespaces```

Describe pod from specific namespace:

```kubectl describe pod mypod -n mynamespace```
