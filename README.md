# Explore Machine Learning Models with Explainable AI: Challenge Lab

## Train the first model

- Train the first model on the complete dataset. Use train_data for your data and train_labels for you labels.

```bash

model = Sequential()
model.add(layers.Dense(200, input_shape=(input_size,), activation='relu'))
model.add(layers.Dense(50, activation='relu'))
model.add(layers.Dense(20, activation='relu'))
model.add(layers.Dense(1, activation='sigmoid'))
model.compile(loss='mean_squared_error', optimizer='adam', metrics=['accuracy'])
model.fit(train_data, train_labels, epochs=10, batch_size=2048, validation_split=0.1)

```

## Train your second model

- Train your second model on the limited dataset. Use limited_train_data for your data and limited_train_labels for your labels. Use the same input_size for the limited_model

```bash

create the limited_model = Sequential()
limited_model.add (your layers)
limited_model.compile
limited_model.fit

```

### Create alert

- Open monitoring
- Create an alert
- Configure the policy to email your email and set

```
   Resource Type : VM Instance
   Metric : CPU utilization
   Filter : instance_name
            Value : kraken-admin
   Condition : is above
   Threshold : 50%
   For : 1 minute

```

## Task 3 : Verify the Spinnaker deployment

- Switch to cloudshell, run

```bash
gcloud config set compute/zone us-east1-b

gcloud container clusters get-credentials spinnaker-tutorial

DECK_POD=$(kubectl get pods --namespace default -l "cluster=spin-deck" -o jsonpath="{.items[0].metadata.name}")

kubectl port-forward --namespace default $DECK_POD 8080:9000 >> /dev/null &

```

- Go to cloudshell webpreview

- Go to applications -> sample

- Open pipelines and manually run the pipeline if it has not already running.

- Approve the deployment to production.

- Check the production frontend endpoint (use http, not the default https)

- Back to cloudshell, run to push a change

```bash
gcloud config set compute/zone us-east1-b

gcloud source repos clone sample-app

cd sample-app
touch a

git config --global user.email "$(gcloud config get-value account)"
git config --global user.name "Student"
git commit -a -m "change"
git tag v1.0.1
git push --tags
```

## Thats It !! Make Sure to Star this Repository !!! Happy Coding !!!! :-)
