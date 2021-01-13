## Services

* Documentation
* https://kubernetes.io/docs/concepts/services-networking/service/
* https://kubernetes.io/docs/tutorials/kubernetes-basics/expose/expose-intro/


Create a deployment:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.7.9
        ports:
        - containerPort: 80
```

Expose the deployment's replica pods with a service:

```

apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  type: ClusterIP
  selector:
    app: nginx
  ports:
  - protocol: TCP
    port: 8080
    targetPort: 80

```

You can get more information about the service with these commands:

```
kubectl get svc
kubectl get endpoints my-service

```
