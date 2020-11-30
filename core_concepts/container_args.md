## Container custom configuration 

* Documentation
* https://kubernetes.io/docs/tasks/inject-data-application/define-command-argument-container/

You can specify custom command for container: 

```
apiVersion: v1
kind: Pod
metadata:
  name: my-command-pod
  labels:
    app: myapp
spec:
  containers:
  - name: myapp-container
    image: busybox
    command: ['echo']
  restartPolicy: Never

  ```

And add args for this command: 

```
apiVersion: v1
kind: Pod
metadata:
  name: my-args-pod
  labels:
    app: myapp
spec:
  containers:
  - name: myapp-container
    image: busybox
    command: ['echo']
    args: ['This is my custom argument']
  restartPolicy: Never

```

Or define port for container, nginx for example:

```
apiVersion: v1
kind: Pod
metadata:
  name: my-containerport-pod
  labels:
    app: myapp
spec:
  containers:
  - name: myapp-container
    image: nginx
    ports:
    - containerPort: 80

```



