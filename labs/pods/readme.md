Create a pod with the following characteristics, and leave it running when complete: 
* The pod must run in the web namespace
* The name of the pod should be cache
* Use the redis image with the 3.2 tag
* Expose port 6379


Solution: 

```
kubectl run cache --image=redis:3.2 --port=6379 -n web
```
