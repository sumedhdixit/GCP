# Create ML Models with BigQuery ML: Challenge Lab

---

## Task 1: Create a dataset to store your machine learning models

```bash
bq mk austin
```

---

## Task 2: Create a forecasting BigQuery machine learning model

```SQL
CREATE OR REPLACE MODEL austin.location_model
OPTIONS
  (model_type='linear_reg', labels=['duration_minutes']) AS
SELECT
    start_station_name,
    EXTRACT(HOUR FROM start_time) AS start_hour,
    EXTRACT(DAYOFWEEK FROM start_time) AS day_of_week,
    duration_minutes,
FROM
    `bigquery-public-data.austin_bikeshare.bikeshare_trips` AS trips
JOIN
    `bigquery-public-data.austin_bikeshare.bikeshare_stations` AS stations
ON
    trips.start_station_name = stations.name
WHERE
    EXTRACT(YEAR FROM start_time) = 2018
    AND duration_minutes > 0

```

### If above doesn't work try This In Cloud Shell ->

```bash
export PROJECT_ID=DEVSHELL_PROJECT_ID

bq query --use_legacy_sql=false "CREATE OR REPLACE MODEL austin.austin_1 OPTIONS(input_label_cols=['duration_minutes'], model_type='linear_reg') AS SELECT duration_minutes, location, start_station_name, CAST(EXTRACT(dayofweek FROM start_time) AS STRING) as dayofweek, CAST(EXTRACT(hour FROM start_time) AS STRING) AS hourofday, FROM \`bigquery-public-data.austin_bikeshare.bikeshare_trips\` AS trips JOIN \`bigquery-public-data.austin_bikeshare.bikeshare_stations\` AS stations ON trips.start_station_id = stations.station_id WHERE EXTRACT(year from start_time) = 2018"

```

---

## Task 3: Create the second machine learning model

```SQL

CREATE OR REPLACE MODEL austin.subscriber_model
OPTIONS
  (model_type='linear_reg', labels=['duration_minutes']) AS
SELECT
    start_station_name,
    EXTRACT(HOUR FROM start_time) AS start_hour,
    subscriber_type,
    duration_minutes
FROM `bigquery-public-data.austin_bikeshare.bikeshare_trips` AS trips
WHERE EXTRACT(YEAR FROM start_time) = 2018
```

---

## Task 4: Evaluate the two machine learning models

```SQL

-- Evaluation metrics for location_model
SELECT
  SQRT(mean_squared_error) AS rmse,
  mean_absolute_error
FROM
  ML.EVALUATE(MODEL austin.location_model, (
  SELECT
    start_station_name,
    EXTRACT(HOUR FROM start_time) AS start_hour,
    EXTRACT(DAYOFWEEK FROM start_time) AS day_of_week,
    duration_minutes
  FROM
    `bigquery-public-data.austin_bikeshare.bikeshare_trips` AS trips
  JOIN
   `bigquery-public-data.austin_bikeshare.bikeshare_stations` AS stations
  ON
    trips.start_station_name = stations.name
  WHERE EXTRACT(YEAR FROM start_time) = 2019)
)
```

```SQL

-- Evaluation metrics for subscriber_model
SELECT
  SQRT(mean_squared_error) AS rmse,
  mean_absolute_error
FROM
  ML.EVALUATE(MODEL austin.subscriber_model, (
  SELECT
    start_station_name,
    EXTRACT(HOUR FROM start_time) AS start_hour,
    subscriber_type,
    duration_minutes
  FROM
    `bigquery-public-data.austin_bikeshare.bikeshare_trips` AS trips
  WHERE
    EXTRACT(YEAR FROM start_time) = 2019)
)
```

---

## Task 5: Use the subscriber type machine learning model to predict average trip durations

```SQL

SELECT
  start_station_name,
  COUNT(*) AS trips
FROM
  `bigquery-public-data.austin_bikeshare.bikeshare_trips`
WHERE
  EXTRACT(YEAR FROM start_time) = 2019
GROUP BY
  start_station_name
ORDER BY
  trips DESC

```

```SQL
SELECT AVG(predicted_duration_minutes) AS average_predicted_trip_length
FROM ML.predict(MODEL austin.subscriber_model, (
SELECT
    start_station_name,
    EXTRACT(HOUR FROM start_time) AS start_hour,
    subscriber_type,
    duration_minutes
FROM
  `bigquery-public-data.austin_bikeshare.bikeshare_trips`
WHERE
  EXTRACT(YEAR FROM start_time) = 2019
  AND subscriber_type = 'Single Trip'
  AND start_station_name = '21st & Speedway @PCL'))

```

---
