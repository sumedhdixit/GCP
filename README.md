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

```bash
limited_model = Sequential()
limited_model.add(layers.Dense(200, input_shape=(input_size,), activation='relu'))
limited_model.add(layers.Dense(50, activation='relu'))
limited_model.add(layers.Dense(20, activation='relu'))
limited_model.add(layers.Dense(1, activation='sigmoid'))
limited_model.compile(loss='mean_squared_error', optimizer='adam', metrics=['accuracy'])
limited_model.fit(limited_train_data, limited_train_labels, epochs=10, batch_size=2048, validation_split=0.1)
```

## Add WithConfigBuilder code

```
config_builder = (WitConfigBuilder(
    examples_for_wit[:num_datapoints],feature_names=column_names)
    .set_custom_predict_fn(limited_custom_predict)
    .set_target_feature('loan_granted')
    .set_label_vocab(['denied', 'accepted'])
    .set_compare_custom_predict_fn(custom_predict)
    .set_model_name('limited')
    .set_compare_model_name('complete'))
WitWidget(config_builder, height=800)

```

## Thats It !! Make Sure to Star this Repository !!! Happy Coding !!!! :-)
