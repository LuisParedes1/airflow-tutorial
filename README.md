# Airflow

[Apache Airflow](https://airflow.apache.org/docs/apache-airflow/stable/index.html) is an open-source platform for developing, scheduling, and monitoring batch-oriented workflows.

Checkout [Notion Page](https://mis-notas.notion.site/Airflow-28025f24dbe280c69580e39aabfac26d?source=copy_link) for a summary of the core concepts.

# Airflow Instalation and configuration

Checkout the [Quick Start page](https://airflow.apache.org/docs/apache-airflow/stable/start.html#quick-start) to install and set up Airflow

## Airflow Standalone

The airflow standalone command initializes the database, creates a user, and starts all components.

```
airflow standalone
```

Visit `localhost:8080` in your browser and log in with the admin account details shown in the terminal.

Checkout [Quick Start](https://airflow.apache.org/docs/apache-airflow/stable/start.html) for more info

## Running Dags

- Remember to save the file within the Dags folder specified in your `airflow.cfg` 
- Default dag folder path is `~/airflow/dags`

To enable and trigger your Dag:

1. Navigate to the Airflow UI. (Run on the terminal `airflow standalone` and go to `http://localhost:8080/dags`)
2. Find your Dag in the list and click the toggle to enable it.
3. You can trigger it manually by clicking the “Trigger Dag” button, or wait for it to run on its schedule.

- We can also run a standalone Dag from the terminal by running

```python
python ~/airflow/dags/[filename].py
```

# Tutorials

- [Airflow 101: Building Your First Workflow](./building-your-first-workflow/README.md)
- [Pythonic Dags with the TaskFlow API](./pythonic_dags_with_the_taskflow_api/)
- [More Tutorials](https://airflow.apache.org/docs/apache-airflow/stable/tutorial/index.html)

# How to Guides

There are several How-to guides available at https://airflow.apache.org/docs/apache-airflow/stable/howto/index.html