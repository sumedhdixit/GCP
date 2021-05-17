# GSP304 : Build and Deploy a Docker Image to a Kubernetes Cluster

## Build Docker image with tag V1

```bash
gsutil cp gs://sureskills-ql/challenge-labs/ch04-kubernetes-app-deployment/echo-web.tar.gz .
```

### OR

```bash
gsutil cp gs://$DEVSHELL_PROJECT_ID/echo-web.tar.gz .
```

## Extract downloaded zip

```bash
tar -xvf echo-web.tar.gz
```

```bash
gcloud builds submit --tag gcr.io/$DEVSHELL_PROJECT_ID/echo-app:v1 .
```

## Create Kubernetes cluster

```bash
gcloud container clusters create echo-cluster --num-nodes 2 --zone us-central1-a --machine-type n1-standard-2
```

## Deploy application to Kubernetes cluster

```bash
kubectl create deployment echo-web --image=gcr.io/qwiklabs-resources/echo-app:v1
```

## Expose app to allow external access

```bash
kubectl expose deployment echo-web --type=LoadBalancer --port=80 --target-port=8000
```

## Extract Kubernetes services

```bash
kubectl get svc
```
