## Creating liviness and readiness probes for container

### Overview

The kubelet uses liveness probes to know when to restart a container. For example, liveness probes could catch a deadlock, where an application is running, but unable to make progress. Restarting a container in such a state can help to make the application more available despite bugs.

The kubelet uses readiness probes to know when a container is ready to start accepting traffic. A Pod is considered ready when all of its containers are ready. One use of this signal is to control which Pods are used as backends for Services. When a Pod is not ready, it is removed from Service load balancers.


### Solution


Create a Probe to Detect and Restart Unhealthy Containers
Let's open up the pod descriptor file to add the necessary configuration for our liveness probe:

vi ~/ckad-service-pod.yml


At the bottom of the file, under the image: line, we should add the following information to configure our liveness probe:

 ```  
   livenessProbe:
       httpGet:
         path: /healthz
         port: 8081

```


The ckad-service-pod.yml file should now look like this:

```
apiVersion: v1
kind: Pod
metadata:
  name: ckad-service
spec:
  containers:
  - name: ckad-service
    image: mk51/ckad-service:2
    livenessProbe:
      httpGet:
        path: /healthz
        port: 8081
        
```


Create a Probe to Detect When the Container is Ready to Service Requests
With our ckad-service-pod.yml file still open, we can go ahead and add our readiness probe by adding the following lines to the bottom of the file, after the port: 8081 line:

```    
    readinessProbe:
      httpGet:
        path: /
        port: 80
        
```


Our ckad-service-pod.yml should now look like this:

```

apiVersion: v1
kind: Pod
metadata:
  name: ckad-service
spec:
  containers:
  - name: ckad-service
    image: mk51/ckad-service:2
    livenessProbe:
      httpGet:
        path: /healthz
        port: 8081
    readinessProbe:
      httpGet:
        path: /
        port: 80
```


### Create the Pod in the Cluster

Now that we have the probe configurations added in our pod descriptor file, we can create a pod in the cluster with the command 

```kubectl apply -f ~/ckad-service-pod.yml```

 Once our pod is created, we can verify that pod was created successfully by running the command kubectl get pods.


