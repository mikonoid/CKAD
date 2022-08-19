## Secrets

Documentation

* https://kubernetes.io/docs/concepts/configuration/secret/
* https://www.hashicorp.com/blog/injecting-vault-secrets-into-kubernetes-pods-via-a-sidecar
* https://www.bmc.com/blogs/kubernetes-secrets/

Create a secret using a yaml definition like this. It's only for lesson!!! Not good idea to store secrets in yaml file. 

```
apiVersion: v1
kind: Secret
metadata:
  name: my-secret
stringData:
  myKey: myPassword
```
  
Once a secret is created, pass the sensitive data to containers as an environment variable:

```
apiVersion: v1
kind: Pod
metadata:
  name: my-secret-pod
spec:
  containers:
  - name: myapp-container
    image: busybox
    command: ['sh', '-c', "echo Hello, Kubernetes! && sleep 3600"]
    env:
    - name: MY_PASSWORD
      valueFrom:
        secretKeyRef:
          name: my-secret
          key: myKey
```
