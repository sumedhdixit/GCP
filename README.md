# Secure Workloads in Google Kubernetes Engine: Challenge Lab

## Task 0: Download the necessary files

```
gsutil cp gs://spls/gsp335/gsp335.zip .
```

```
unzip gsp335.zip
```

## Task 1

```bash
gcloud container clusters create kraken-cluster \
   --zone us-central1-c \
   --machine-type n1-standard-4 \
   --num-nodes 2 \
   --enable-network-policy
```

```bash
gcloud sql instances create kraken-cloud-sql --region us-central1
```

## Task 2

### Create database - wordpress

### Add user - wordpress (no password)

### Service account

```
gcloud iam service-accounts create kraken-wordpress-sa
```

```
gcloud projects add-iam-policy-binding $DEVSHELL_PROJECT_ID \
   --member="serviceAccount:kraken-wordpress-sa@$DEVSHELL_PROJECT_ID.iam.gserviceaccount.com" \
 --role="roles/cloudsql.client"
```

```
gcloud iam service-accounts keys create key.json --iam-account=kraken-wordpress-sa@$DEVSHELL_PROJECT_ID.iam.gserviceaccount.com
```

```bash
kubectl create secret generic cloudsql-instance-credentials --from-file key.json
```

```bash
kubectl create secret generic cloudsql-db-credentials \
 --from-literal username=wordpress \
 --from-literal password=''
```

### goto editor and reolace isntance name with sql instance name

`save`

```
kubectl apply -f wordpress.yaml
```

## Task 3

```
helm version
```

```
helm repo add stable https://charts.helm.sh/stable
helm repo update
```

```
helm install nginx-ingress stable/nginx-ingress --set rbac.create=true
```

```
kubectl get service
```

```
. add_ip.sh
student03aea8d07e853d.labdns.xyz
```

```
kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v1.2.0/cert-manager.yaml
```

```
kubectl create clusterrolebinding cluster-admin-binding \
 --clusterrole=cluster-admin \
 --user=$(gcloud config get-value core/account)
```

### goto editor and edit issuer.yaml to include lab email address

```
kubectl apply -f issuer.yaml
```

```
goto editor and edit ingress.yaml to include dns address received as output from . add_ip.sh
```

```
kubectl apply -f ingress.yaml'
```

### goto editor and in network-policy.yaml add to end

```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
name: allow-world-to-nginx-ingress
namespace: default
spec:
podSelector:
matchLabels:
app: nginx-ingress
policyTypes:

- Ingress
  ingress:
- {}
```

```
kubectl apply -f network-policy.yaml
```

## Task 5

### goto security - Binary authorisatioin

- configure policy
- disallow all images
- create specific rules, select cluster
- add specific rule, type us and select from dropdown, click add
- custom expetion path
- add iamge paths given
- save policy
  enanble binary authorisation for kuberetes clusater

## Task 6

### edit psp-restrictive.yaml

### line 2 change extensions/v1beta1 to policy/v1beta1

```
kubectl apply -f psp-restrictive.yaml
kubectl apply -f psp-role.yaml
kubectl apply -f psp-use.yaml
```
