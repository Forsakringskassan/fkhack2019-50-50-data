# Data

Install Virtual Box
Install docker-machine
docker-machine create <name>
eval $(docker-machine env <name>)

Install Google Cloud SDK latest.
Install kubectl for gcloud:
```gcloud components install kubectl```

https://cloud.google.com/kubernetes-engine/docs/tutorials/hello-app
Build image:
```
docker build -t fkhack2019/data-image .
```
Test image locally:
```
docker run -d --name data -p 8091-8096:8091-8096 -p 11210-11211:11210-11211 fkhack2019/data-image
```
Tag image:
```
docker tag fkhack2019/data-image gcr.io/hackforfk/data-image
```
Upload image to gcloud
```
docker push
```
Deploy instance to gcloud cluster:
```
kubectl run data-image --image=gcr.io/hackforfk/data-image --port=8091 --port=8092
deployment.apps/data-image created

$ kubectl get pods
NAME                          READY     STATUS    RESTARTS   AGE
data-image-598cc9fd67-s2rtz   1/1       Running   0          45s
```
Expose endpoint:
```
kubectl expose deployment data-image --type=LoadBalancer --port 8091 --target-port 8091
```
