## K8S Cronjob

Our company has a simple data cleanup process that is run periodically for maintenance purposes. They would like to stop doing this manually in order to save time, so we've have been asked to implement a cron job in the Kubernetes cluster that runs this process. We need to create a cron job called cleanup-cronjob using the mk51/cleanup-data-cronjob image, and the job needs to run every minute.


### Create a cronjob

```
vi ~/cleanup-cronjob.yml

```

Add to file

```

apiVersion: batch/v1beta1
kind: CronJob
metadata:
  name: cleanup-cronjob
spec:
  schedule: "*/1 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: data-cleanup
            image: mk51/cleanup-data-cronjob:latest
          restartPolicy: OnFailure

```

Run kubectl apply for creating cronjob

```

kubectl apply -f ~/cleanup-cronjob.yml

```

Check if cronjob is working 

```
kubectl get cronjob cleanup-cronjob

```



