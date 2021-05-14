# Build a Website on Google Cloud: Challenge Lab

## Task 1 :

```bash
git clone https://github.com/googlecodelabs/monolith-to-microservices.git

cd ~/monolith-to-microservices

./setup.sh

cd ~/monolith-to-microservices/monolith

npm start

```

### Stop it and then

```bash
gcloud services enable cloudbuild.googleapis.com

gcloud builds submit --tag gcr.io/${GOOGLE_CLOUD_PROJECT}/fancytest:1.0.0 .
```

## Task 2 :

```bash

gcloud config set compute/zone us-central1-a

gcloud services enable container.googleapis.com

gcloud container clusters create fancy-cluster --num-nodes 3

kubectl create deployment fancytest --image=gcr.io/${GOOGLE_CLOUD_PROJECT}/fancytest:1.0.0

kubectl expose deployment fancytest --type=LoadBalancer --port 80 --target-port 8080

```

## Task 3 :

```bash

cd ~/monolith-to-microservices/microservices/src/orders

gcloud builds submit --tag gcr.io/${GOOGLE_CLOUD_PROJECT}/orders:1.0.0 .

cd ~/monolith-to-microservices/microservices/src/products

gcloud builds submit --tag gcr.io/${GOOGLE_CLOUD_PROJECT}/products:1.0.0 .

```

## Task 4:

```bash

kubectl create deployment orders --image=gcr.io/${GOOGLE_CLOUD_PROJECT}/orders:1.0.0

kubectl expose deployment orders --type=LoadBalancer --port 80 --target-port 8081

kubectl create deployment products --image=gcr.io/${GOOGLE_CLOUD_PROJECT}/products:1.0.0

kubectl expose deployment products --type=LoadBalancer --port 80 --target-port 8082

```

## Task 5 :

```bash

cd ~/monolith-to-microservices/react-app

nano .env

```

```bash

npm run build

```

## Task 6 :

```bash

cd ~/monolith-to-microservices/microservices/src/frontend

gcloud builds submit --tag gcr.io/${GOOGLE_CLOUD_PROJECT}/frontend:1.0.0 .

```

## Task 7 :

```bash

kubectl create deployment frontend --image=gcr.io/${GOOGLE_CLOUD_PROJECT}/frontend:1.0.0



kubectl expose deployment frontend --type=LoadBalancer --port 80 --target-port 8080

```
