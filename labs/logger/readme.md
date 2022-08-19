* Create a deployment named deployment-xyz in the default namespace, that:
* Includes a primary
* busybox container, named logger-dev
* includes a sidecar fluentd:v0.12 container, named adapter-zen
* Mounts a shared volume /tmp/log on both containers, which does not persist when the pod is deleted * Instructs the logger-dev container to run the command

command for busybox container

```
while true; do
echo "I like k8s" >> /
tnp/log/input.log;
sleep 10;
done 

```

Solution

```
kubectl apply -f myvol1.yaml

kubectl apply -f myvol2.yaml

kubectl apply -f pvc.yaml

kubectl apply -f deployment.yaml

```


