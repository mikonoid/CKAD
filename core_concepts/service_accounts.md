## Service Accounts

* Documentation
* https://kubernetes.io/docs/reference/access-authn-authz/service-accounts-admin/
* https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/


Create a service account: 

```kubectl create serviceaccount my-serviceaccount```

User service account argument in the pod:

```
apiVersion: v1
kind: Pod
metadata:
  name: my-serviceaccount-pod
spec:
  serviceAccountName: my-serviceaccount
  containers:
  - name: myapp-container
    image: busybox
    command: ['sh', '-c', "echo Kubernetes is cool! && sleep 3600"]

```
