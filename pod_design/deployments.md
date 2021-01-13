## Deployments

* Documentation
* https://kubernetes.io/docs/concepts/workloads/controllers/deployment/


Here is a simple deployment for three replica pods running nginx.

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

You can explore and manage deployments using the same kubectl commands you would use for other object types.

```
kubectl get deployments

kubectl get deployment <deployment name>

kubectl describe deployment <deployment name>

kubectl edit deployment <deployment name>

kubectl delete deployment <deployment name>

```