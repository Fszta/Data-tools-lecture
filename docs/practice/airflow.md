# Airflow in practice

## Installation
We've seen many way of deploying airflow, for practice, the most straightforward way is
to install it via pip and run it in standalone mode.

Here a quick tutorial on how to install & run it : [airflow documentation](https://airflow.apache.org/docs/apache-airflow/stable/start.html)

### Create an admin user to connect to the ui
If you not run all the commands separately, you'll probably need to create a user to connect to the UI

```shell
airflow users create \
          --username antoine \
          --firstname admin \
          --lastname admin \
          --role Admin \
          --password admin \
          --email admin@example.org
```

### Set dags folder
!!! tip

    This is not mandatory, but you can modify the default path of the dag folder to a more convenient place.
    This is the folder from where airflow will grab the dags you've developed
In `airflow.cfg` file, update `dags_folder` path to the folder you want on your machine.
By default, it's set to your HOME directory.

## Test to access the UI

By default airflow run on [localhost:8080](localhost:8080). Normally you should have some examples dags that come with
the installation if you haven't updated the config file.


## Exercice
The aim of the exercise is to create a more realistic dag to retrieve some data from an API,
perform some transformation, and store it in a database.

Requirements :
For this exercise, you'll need : 

* airflow
* a database : postgres (feel free to try another)
* metabase


### Your first dag
Create a hello world dag that run every minute and just print `hello world`, just to ensure that all is working 
as expected.

### A real dag
For this exercise, we will use the [velib API](https://velib-metropole-opendata.smoove.pro/opendata/Velib_Metropole/station_status.json),

Your dag must include (at least the following task):

- A task to retrieve the data from the API
- A task to quickly transform the data (process the data to make it fit the target system)
- A task to store the result in your a table of your database
- A task to run some aggregation on the table previously feeded with new data.

#### 1 - Data acquisition task
Use the following endpoint : https://velib-metropole-opendata.smoove.pro/opendata/Velib_Metropole/station_status.json
to extract the last updated data from the velib api.

#### 2 - Transform the data

In this task, you need the json data to be in a flattened format. For each station we want to have the following 
information : 

* station_id
* number_of_available_docks
* number_of_electric_bike
* number_of_mechanical_bike
* the timestamp at which data has been updated on the server side

!!! tip

    You can use the method of your choice, you can do it using pure python, or using some libraries
    

#### 3 - Store the data

!!! warning

    Before developing the task, you'll need to create a connection to your postgres database

Create a task that takes the previously transformed data an insert in a table called `velib_station_status`.
You can manage the table creation from your dag (using `CREATE TABLE IF NOT EXISTS`, (before the insert statement),
or do it using the pg cli.

Here is the expected schema of the table : 

Column Name | Description                                                 | Type
------------|-------------------------------------------------------------|-----
id | ID of the record - primary key                              | INT
station_id | ID of the station                                           | INT
number_of_available_docks | Number of available docks at the station                    | INT
number_of_electric_bike | Number of available electric bikes at the station           | INT
number_of_mechanical_bike | Number of available mechanical bikes at the station         | INT
timestamp_data_updated_on_server | Timestamp at which data has been updated on the server side | TIMESTAMP


Here is an example form the [airflow documentation](https://airflow.apache.org/docs/apache-airflow-providers-postgres/stable/operators/postgres_operator_howto_guide.html)

#### 4 - Aggregate the data

Create a last task which execute a postgres request to count : 

* the total number of free docks
* the total number of available mechanical bike
* the total number of available electric bike

The result must be store in another table : `stations_aggregates` with the following columns : 
Here is the expected schema of the table :

Column Name | Description | Type
------------|-------------|-----
id | ID of the record - Primary key | INT
number_free_docks | Number of free docks at the station | INT
number_available_e_bikes | Number of available electric bikes at the station | INT
number_available_mechanical_bikes | Number of available mechanical bikes at the station | INT
created_at | Timestamp of the computation | TIMESTAMP


!!! tip


#### 5 - Plug metabase on your database

#### Bonus
Create a slack integration and add an on_failure callback to send an alert when your dag fail
    