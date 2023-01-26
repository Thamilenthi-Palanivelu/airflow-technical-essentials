# Airflow Technical Essentials

This repository contains code and examples used alongside the Apache Airflow Technical Essentials live training on O'Reilly.

## Prerequisites
* [Docker](https://docs.docker.com/get-docker/)

## Standalone Installation
```
mkdir airflow-training && cd airflow-training
python3 -m venv airflow-venv
source ./airflow-venv/bin/activate
python_version=$(python --version | cut -d " " -f 2 | cut -d "." -f 1-2)
constraint_url="https://raw.githubusercontent.com/apache/airflow/constraints-2.5.0/constraints-${python_version}.txt"
pip install apache-airflow==2.5.0 --constraint "${constraint_url}"
export AIRFLOW_HOME=$(pwd)

airflow standalone
```

## docker-compose Installation
```
echo -e "AIRFLOW_UID=$(id -u)" >> .env
docker-compose up airflow-init
docker-compose up

# user: airflow
# password: airflow

# cleanup
docker-compose down --volumes --remove-orphans
```

## dags/example_dag.py
A basic DAG that introduces the concepts of **tasks**, **operators**, **dependencies**, and the **DAG**.

## dags/weather_checker.py
A more robust DAG that uses **variable**, **connection**, **xcom**, **SimpleHTTPOperator**, and **task context**.

## dags/query_dag.py
A dag that queries the Airflow metastore using the `PostgresOperator`.

## Accessing the metastore container
```
docker exec -it $POSTGRES_CONTAINER psql -h postgres -U airflow 
# password is "airflow"
```

## Notes
- The **logical date**, formerly known as **execution date**, is the start of the _data interval_, not when the DAG is actually executed.
- Environment variables override `airflow.cfg` (See `.env` for examples)