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

### Lab with ambassador container (Haproxy + legacy app)

Description

The legacy app in docker mk51/ambassador-example:latest is hard-coded to only
server content on port 8775, but you wanna to be able to access the service with standart port 80.
The task is to build kubernetes setup with ambassador container with Haproxy and expose port 80.


Configmap:

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: ambassador-config
data:
  haproxy.cfg: |-
    global
        daemon
        maxconn 256

    defaults
        mode http
        timeout connect 5000ms
        timeout client 50000ms
        timeout server 50000ms

    listen http-in
        bind *:80
        server server1 127.0.0.1:8775 maxconn 32
 ```
 
 Apply this configmap
 
 ```kubectl apply -f ambassador-config.yml```
 
 Create pod definition YAML file:
 
 ```
 apiVersion: v1
kind: Pod
metadata:
  name: legacy-service
spec:
  containers:
  - name: legacy-service
    image: mk51/ambassador-example:latest
  - name: haproxy-ambassador
    image: haproxy:1.7
    ports:
    - containerPort: 80
    volumeMounts:
    - name: config-volume
      mountPath: /usr/local/etc/haproxy
  volumes:
  - name: config-volume
    configMap:
      name: ambassador-config
 ```

Apply this file:

```kubectl apply -f fruit-service.yml```

Check that pod is ready:

```kubectl get pods```

Now, can check on legacy-service from another pod, on port 80

Create busybox.yml

```
apiVersion: v1
kind: Pod
metadata:
  name: busybox
spec:
  containers:
  - name: myapp-container
    image: radial/busyboxplus:curl
    command: ['sh', '-c', 'while true; do sleep 3600; done']
```

Apply pod definition yml with kubectl:

```kubectl apply -f busybox.yml```

Check legacy app with curl:

```kubectl exec busybox -- curl $(kubectl get pod legacy-service -o=custom-columns=IP:.status.podIP --no-headers):80```

If everything is working, you should see some JSON listing
