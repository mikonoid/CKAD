### Implement State Persistence for Kubernetes Pods


### Overview 
Our company needs a small database server to support a new application. They have asked us to deploy a pod running a MySQL container, but they want the data to persist even if the pod is deleted or replaced. Therefore, the MySQL database pod requires persistent storage.

We need to accomplish the following:

Create a PersistentVolume:

* The PersistentVolume should be named mysql-pv.
* The volume needs a capacity of 1Gi.
* Use a storageClassName of localdisk.
* Use the accessMode ReadWriteOnce.
* Store the data locally on the node using a hostPath volume at the location /mnt/data.
* Create a PersistentVolumeClaim:

The PersistentVolumeClaim should be named mysql-pv-claim.
Set a resource request on the claim for 500Mi of storage.
Use the same storageClassName and accessModes as the PersistentVolume so that this claim can bind to the PersistentVolume.


Create a MySQL Pod configured to use the PersistentVolumeClaim:

* The Pod should be named mysql-pod.
* Use the image mysql:5.6.
* Expose the containerPort 3306.
* Set an environment variable called MYSQL_ROOT_PASSWORD with the value password.
* Add the PersistentVolumeClaim as a volume and mount it to the container at the path /var/lib/mysql.
* Get logged in
* Use the credentials and server IP in the hands-on lab overview page to log in with SSH.

### Solution 

Create a PersistentVolume
Create a descriptor file using ```vi mysql-pv.yml```, with these contents:

```
kind: PersistentVolume
apiVersion: v1
metadata:
  name: mysql-pv
spec:
  storageClassName: localdisk
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/mnt/data"
```


Create the PersistentVolume:

```kubectl apply -f mysql-pv.yml ```

Create a PersistentVolumeClaim
Create a descriptor file using ```vi mysql-pv-claim.yml``` with these contents:

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mysql-pv-claim
spec:
  storageClassName: localdisk
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 500Mi
```

Create the PersistentVolumeClaim:

```kubectl apply -f mysql-pv-claim.yml```


How are we doing? Are these created and behaving properly? Let's check:

```kubectl get pvc```

We should see our claim, and it should have a status of Bound. We're all set, so we can keep moving.

Create a MySQL Pod configured to use the PersistentVolumeClaim
Create a descriptor file using ```vi mysql-pod.yml``` with these contents:

```
apiVersion: v1
kind: Pod
metadata:
  name: mysql-pod
spec:
  containers:
  - name: mysql
    image: mysql:5.6
    ports:
    - containerPort: 3306
    env:
    - name: MYSQL_ROOT_PASSWORD
      value: password
    volumeMounts:
    - name: mysql-storage
      mountPath: /var/lib/mysql
  volumes:
  - name: mysql-storage
    persistentVolumeClaim:
      claimName: mysql-pv-claim
```

Create the Pod:

```kubectl apply -f mysql-pod.yml```
Check the status of the pod with kubectl get pod mysql-pod. After a few moments it should be in the RUNNING status.

We can also verify that the pod is interacting with the filesystem at /mnt/data on the node. 

Login into master node and check PV directory: 

```ls /mnt/data```

You should see files and directories there related to MySQL. These were created by the pod, meaning that your PersistentVolume and PersistentVolumeClaim are working! 