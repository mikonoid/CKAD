
* Documentation
* https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/

Get all api resources

```kubectl api-resources -o name```

Get pods for specific namespace

```kubectl get pods -n kube-system```

Get all nodes 

```kubectl get nodes```

Get specific node

```kubectl get nodes $node_name```

Get specific node in yaml

```kubectl get nodes $node_name -o yaml```

Describe specific node 

```kubectl describe node $node_name```
