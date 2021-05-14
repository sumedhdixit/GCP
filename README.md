# GSP330 : Implement DevOps in Google Cloud: Challenge Lab

## First Task

```bash
gcloud config set compute/zone us-east1-b
```

```bash
git clone https://source.developers.google.com/p/$DEVSHELL_PROJECT_ID/r/sample-app
```

```bash
gcloud container clusters get-credentials jenkins-cd
```

```bash
kubectl create clusterrolebinding cluster-admin-binding --clusterrole=cluster-admin --user=$(gcloud config get-value account)
```

```bash
helm repo add stable https://charts.helm.sh/stable
```

```bash
helm repo update
```

```bash
helm install cd stable/jenkins
```

```bash
export POD_NAME=$(kubectl get pods --namespace default -l "app.kubernetes.io/component=jenkins-master" -l "app.kubernetes.io/instance=cd" -o jsonpath="{.items[0].metadata.name}")
kubectl port-forward $POD_NAME 8080:8080 >> /dev/null &
printf $(kubectl get secret cd-jenkins -o jsonpath="{.data.jenkins-admin-password}" | base64 --decode);echo
```

```bash
cd sample-app
kubectl create ns production
kubectl apply -f k8s/production -n production
kubectl apply -f k8s/canary -n production
kubectl apply -f k8s/services -n production
```

```bash
kubectl get svc
kubectl get service gceme-frontend -n production
```

### After these commands do the jenkins part then do the rest commands -

```bash
git init
git config credential.helper gcloud.sh
git remote add origin https://source.developers.google.com/p/$DEVSHELL_PROJECT_ID/r/sample-app
```

```bash
git config --global user.email "<user email>"
```

```bash
git config --global user.name "<user name>"
```

```bash
git add .
git commit -m "initial commit"
git push origin master
```

## 2nd Task

```bash
git checkout -b new-feature
```

### After this command make the required changes!

```bash
git add Jenkinsfile html.go main.go
git commit -m "Version 2.0.0"
git push origin new-feature
```

## 3rd Task

```
curl http://localhost:8001/api/v1/namespaces/new-feature/services/gceme-frontend:80/proxy/version
kubectl get service gceme-frontend -n production
git checkout -b canary
git push origin canary
export FRONTEND_SERVICE_IP=$(kubectl get -o \
jsonpath="{.status.loadBalancer.ingress[0].ip}" --namespace=production services gceme-frontend)
git checkout master
git push origin master
```

## 4rth Task

```

export FRONTEND_SERVICE_IP=$(kubectl get -o \
jsonpath="{.status.loadBalancer.ingress[0].ip}" --namespace=production services gceme-frontend)
while true; do curl http://$FRONTEND_SERVICE_IP/version; sleep 1; done
```

```bash
kubectl get service gceme-frontend -n production
```

```bash
git merge canary
git push origin master
export FRONTEND_SERVICE_IP=$(kubectl get -o \
jsonpath="{.status.loadBalancer.ingress[0].ip}" --namespace=production services gceme-frontend)
```

### DONE
