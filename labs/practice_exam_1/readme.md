## CKAD Practice Exam - 1 

### The scenario

You are working on a new web application designed to run in a Kubernetes cluster. The development team has designed a web service which serves a list of 8000m mountains.

We have been asked to create a deployment that meets the app's specifications. Then, we need to expose the application using a NodePort service. This setup should meet the following criteria:

All objects should be in the mountains namespace.
The deployment should be named 8000-deployment.
The deployment should have 3 replicas.
The deployment's pods should have one container using the mk51/ckad-lab1 image with the tag latest.
Run the container with the command nginx.
Run the container with the arguments "-g", "daemon off;".
The pods should expose port 80 to the cluster.
The pods should be configured to periodically check the /healthz endpoint on port 8081, and restart automatically if the request fails.
The pods should not receive traffic from the service until the / endpoint on port 80 responds successfully.
The service should be named 8000-service.
The service should forward traffic to port 80 on the pods.
The service should be exposed externally by listening on port 38000 on each node.
Get logged in
Use the credentials and server IP in the hands-on lab overview page to log in with SSH.


Create a namespace: 

```kubectl create ns mountains```

Build the deployment and create it in the cluster
Build a deployment descriptor (with whatever text editor you like) called 8000-deployment.yml:


```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: 8000-deployment
  namespace: mountains
spec:
  replicas: 3
  selector:
    matchLabels:
      app: 8000
  template:
    metadata:
      labels:
        app: 8000
    spec:
      containers:
      - name: 8000
        image: mk51/ckad-lab1:latest
        command: ["nginx"]
        args: ["-g", "daemon off;"]
        ports:
        - containerPort: 80
        livenessProbe:
          httpGet:
            path: /healthz
            port: 8081
        readinessProbe:
          httpGet:
            path: /
            port: 80

```

Create the deployment in the cluster:

```kubectl apply -f 8000-deployment.yml```


A quick ```kubectl get deploy -n mountains``` should show us some ready pods.

Build the service and create it in the cluster
Build a service descriptor (again, with whatever text editor you like) called 8000-service.yml:

```
apiVersion: v1
kind: Service
metadata:
  name: 8000-service
  namespace: mountains
spec:
  type: NodePort
  selector:
    app: 8000
  ports:
  - protocol: TCP
    port: 80
    nodePort: 38000

```

Create the service in the cluster:

```kubectl apply -f 8000-service.yml```

We can verify the service is working by testing the nodePort with a curl command:

```curl localhost:38000```