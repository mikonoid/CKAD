### Create deployment with redis pod, expose deployment and apply NetworkPolicy


### Overview 

Create a redis deployment using the image redis:alpine with 1 replica and label app=redis.
Expose it via a ClusterIP service called redis on port 6379.
Create a new Ingress Type NetworkPolicy called redis-access which allows
only the pods with label access=redis to access the deployment.

### Solution

Create a deployment

```kubectl create deploy redis --image=redis:alpine --replicas=1```

Expose deployment with a service ClusterIP

```kubectl expose deploy redis --name=redis --port=6379 --type=ClusterIP```

Create a NetworkPolicy

```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: redis-access
spec:
  podSelector:
    matchLabels:
      app: redis
  ingress:
  - from:
    - podSelector:
        matchLabels:
          access: redis
 ```
 
 Apply network policy with 
 
 ``` kubectl apply -f networkpolicy.yaml```
