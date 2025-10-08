# Pythonic Dags with the TaskFlow API

The TaskFlow API is designed to make your code simpler, cleaner, and easier to maintain. You write plain Python functions, decorate them, and Airflow handles the rest — including task creation, dependency wiring, and passing data between tasks.

[In this tutorial](https://airflow.apache.org/docs/apache-airflow/stable/tutorial/taskflow.html#pythonic-dags-with-the-taskflow-api), we’ll create a simple ETL pipeline — Extract → Transform → Load using the TaskFlow API.

# Understanding Dag file [`tutorial_taskflow_api.py`](./tutorial_taskflow_api.py)

## Step 1: Define the Dag

Just like before, your Dag is a Python script that Airflow loads and parses. But this time, we’re using the @dag decorator to define it.

```
@dag(
    schedule=None,
    start_date=pendulum.datetime(2021, 1, 1, tz="UTC"),
    catchup=False,
    tags=["example"],
)
def tutorial_taskflow_api():
    pass
```

To make this Dag discoverable by Airflow, we can call the Python function that was decorated with @dag:

```
tutorial_taskflow_api()
```

## Step 2: Write Your Tasks with @task

With TaskFlow, each task is just a regular Python function. You can use the @task decorator to turn it into a task that Airflow can schedule and run.

```
@task()
def extract():
    """
    #### Extract task
    A simple Extract task to get data ready for the rest of the data
    pipeline. In this case, getting data is simulated by reading from a
    hardcoded JSON string.
    """
    data_string = '{"1001": 301.27, "1002": 433.21, "1003": 502.22}'

    order_data_dict = json.loads(data_string)
    return order_data_dict
```

The function’s return value is passed to the next task — no manual use of XComs required. Under the hood, TaskFlow uses XComs to manage data passing automatically, abstracting away the complexity of manual XCom management from the previous methods.


## Step 3: Build the Flow

Once the tasks are defined, you can build the pipeline by simply calling them like Python functions. Airflow uses this functional invocation to set task dependencies and manage data passing.

```
order_data = extract()
order_summary = transform(order_data)
load(order_summary["total_order_value"])
```

That’s it! Airflow knows how to schedule and orchestrate your pipeline from this code alone.

# Running Your Dag

Remember to save the file within the Dags folder specified in your airflow.cfg (`~/airflow/dags/tutorial_taskflow_api.py`)


To enable and trigger your Dag:

1. Navigate to the Airflow UI.
2. Find your Dag in the list and click the toggle to enable it.
3. You can trigger it manually by clicking the “Trigger Dag” button, or wait for it to run on its schedule.

- We can also run a standalone Dag from the terminal by running

```python
python ~/airflow/dags/[filename].py
```