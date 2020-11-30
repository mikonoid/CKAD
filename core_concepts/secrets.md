## Secrets

* Documentation
* https://kubernetes.io/docs/concepts/configuration/secret/


Create a secret with password:

```
apiVersion: v1
kind: Secret
metadata:
  name: mysecret
stringData:
  myKey: mypassword

```

Create pod and pass credentials from env variable:

```
apiVersion: v1
kind: Pod
metadata:
  name: my-secret-pod
spec:
  containers:
  - name: myapp-container
    image: busybox
    command: ['sh', '-c', "echo Kubernetes is cool! && sleep 3600"]
    env:
    - name: MY_PASSWORD
      valueFrom:
        secretKeyRef:
          name: mysecret
          key: myKey

```