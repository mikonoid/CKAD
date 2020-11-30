## Security Context 

* Documentation
* https://kubernetes.io/docs/tasks/configure-pod-container/security-context/


Create users, groups and files on both worker nodes:

```
sudo useradd -u 2000 container-user-0
sudo groupadd -g 3000 container-group-0
sudo useradd -u 2001 container-user-1
sudo groupadd -g 3001 container-group-1
sudo mkdir -p /etc/userfile/
echo "Kubernetes is coool!" | sudo tee -a /etc/userfile/userfile.txt
sudo chown 2000:3000 /etc/userfile/userfile.txt
sudo chmod 640 /etc/userfile/userfile.txt

```

On the master node, create a pod: 

```vi my-securitycontext-pod.yml```

YAML file: 

```
apiVersion: v1
kind: Pod
metadata:
  name: my-securitycontext-pod
spec:
  containers:
  - name: myapp-container
    image: busybox
    command: ['sh', '-c', "cat /userfile/userfile.txt && sleep 3600"]
    volumeMounts:
    - name: userfile-volume
      mountPath: /userfile
  volumes:
  - name: userfile-volume
    hostPath:
      path: /etc/userfile

```
Check logs: 

```kubectl logs my-securitycontext-pod```

Delete pod: 

```kubectl delete pod my-securitycontext-pod --now```

Create pod again but with user and group:

```
apiVersion: v1
kind: Pod
metadata:
  name: my-securitycontext-pod
spec:
  securityContext:
    runAsUser: 2001
    fsGroup: 3001
  containers:
  - name: myapp-container
    image: busybox
    command: ['sh', '-c', "cat /userfile/userfile.txt && sleep 3600"]
    volumeMounts:
    - name: userfile-volume
      mountPath: /userfile
  volumes:
  - name: userfile-volume
    hostPath:
      path: /etc/userfile

```

Check logs again, you should see like a "Permission denied"

```kubectl logs my-securitycontext-pod```

Delete pod  and create a new one with user 2000 and group 3000 (it is correct permission for file)

```kubectl delete pod my-securitycontext-pod --now```

```
apiVersion: v1
kind: Pod
metadata:
  name: my-securitycontext-pod
spec:
  securityContext:
    runAsUser: 2000
    fsGroup: 3000
  containers:
  - name: myapp-container
    image: busybox
    command: ['sh', '-c', "cat /userfile/userfile.txt && sleep 3600"]
    volumeMounts:
    - name: userfile-volume
      mountPath: /userfile
  volumes:
  - name: userfile-volume
    hostPath:
      path: /etc/userfile

```

Check logs again and ensure you see userfile from file:

```kubectl logs my-securitycontext-pod```








