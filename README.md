# Airflow

[Apache Airflow](https://airflow.apache.org/docs/apache-airflow/stable/index.html) is an open-source platform for developing, scheduling, and monitoring batch-oriented workflows.

# Airflow Instalation and configuration

Checkout the [Quick Start page](https://airflow.apache.org/docs/apache-airflow/stable/start.html#quick-start) to install and set up Airflow

## Airflow Standalone

The airflow standalone command initializes the database, creates a user, and starts all components.

```
airflow standalone
```

Visit localhost:8080 in your browser and log in with the admin account details shown in the terminal. Enable the example_bash_operator Dag in the home page.

Checkout [Quick Start](https://airflow.apache.org/docs/apache-airflow/stable/start.html) for more info

# Tutorials

- [Airflow 101: Building Your First Workflow](./building-your-first-workflow/README.md)
- [Pythonic Dags with the TaskFlow API](./pythonic_dags_with_the_taskflow_api/)
- [More Tutorials](https://airflow.apache.org/docs/apache-airflow/stable/tutorial/index.html)