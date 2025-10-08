# Airflow 101: Building Your First Workflow

[In this guide](https://airflow.apache.org/docs/apache-airflow/stable/tutorial/fundamentals.html), we'll learn the essential concepts of Airflow, helping you understand how to write your first Dag.

# What is a Dag?

* At its core, a Dag is a collection of tasks organized in a way that reflects their relationships and dependencies.
* It’s like a roadmap for your workflow, showing how each task connects to the others.


## Understanding the Dag Definition File [`tutorial.py`](./tutorial.py)

* Think of the Airflow Python script as a configuration file that lays out the structure of your Dag in code. 
* The actual tasks you define here run in a different environment, which means this script isn’t meant for data processing. Its main job is to define the Dag object, and it needs to evaluate quickly since the Dag File Processor checks it regularly for any changes.

### Setting Default Arguments

When creating a Dag and its tasks, you can either pass arguments directly to each task or define a set of default parameters in a dictionary. The latter approach is usually more efficient and cleaner.

```
# These args will get passed on to each operator
# You can override them on a per-task basis during operator initialization
default_args={
    "depends_on_past": False,
    "retries": 1,
    "retry_delay": timedelta(minutes=5),
    # 'queue': 'bash_queue',
    # 'pool': 'backfill',
    # 'priority_weight': 10,
    # 'end_date': datetime(2016, 1, 1),
    # 'wait_for_downstream': False,
    # 'execution_timeout': timedelta(seconds=300),
    # 'on_failure_callback': some_function, # or list of functions
    # 'on_success_callback': some_other_function, # or list of functions
    # 'on_retry_callback': another_function, # or list of functions
    # 'sla_miss_callback': yet_another_function, # or list of functions
    # 'on_skipped_callback': another_function, #or list of functions
    # 'trigger_rule': 'all_success'
},
```

If you want to dive deeper into the parameters of the BaseOperator, take a look at the documentation for [airflow.sdk.BaseOperator documentation](https://airflow.apache.org/docs/apache-airflow/2.0.2/_api/airflow/models/baseoperator/index.html).

### Creating a Dag

Next, we’ll need to create a Dag object to house our tasks. We’ll provide a unique identifier for the Dag, known as the dag_id, and specify the default arguments we just defined. We’ll also set a schedule for our Dag to run every day.

```
with DAG(
    dag_id="tutorial",
    # These args will get passed on to each operator
    # You can override them on a per-task basis during operator initialization
    default_args={
        "depends_on_past": False,
        "retries": 1,
        "retry_delay": timedelta(minutes=5),
        # 'queue': 'bash_queue',
        # 'pool': 'backfill',
        # 'priority_weight': 10,
        # 'end_date': datetime(2016, 1, 1),
        # 'wait_for_downstream': False,
        # 'execution_timeout': timedelta(seconds=300),
        # 'on_failure_callback': some_function, # or list of functions
        # 'on_success_callback': some_other_function, # or list of functions
        # 'on_retry_callback': another_function, # or list of functions
        # 'sla_miss_callback': yet_another_function, # or list of functions
        # 'on_skipped_callback': another_function, #or list of functions
        # 'trigger_rule': 'all_success'
    },
    description="A simple tutorial DAG",
    schedule=timedelta(days=1),
    start_date=datetime(2021, 1, 1),
    catchup=False,
    tags=["example"],
) as dag:
```

### Defining Tasks

- To use an operator, you must instantiate it as a task.
- Tasks dictate how the operator will perform its work within the Dag’s context. 
- The task_id serves as a unique identifier for each task.

```
t1 = BashOperator(
    task_id="print_date",
    bash_command="date",
)

t2 = BashOperator(
    task_id="sleep",
    depends_on_past=False,
    bash_command="sleep 5",
    retries=3,
)
```

Notice how we mix operator-specific arguments (like bash_command) with common arguments (like retries) inherited from BaseOperator.

The precedence for task arguments is as follows:
1. Explicitly passed arguments
2. Values from the default_args dictionary
3. The operator’s default values, if available

### Setting up Dependencies

In Airflow, tasks can depend on one another.

```
t1.set_downstream(t2)

t1 << t2
```

# Testing Your Pipeline

First, ensure that your script parses successfully. If you saved your code in tutorial.py within the Dags folder specified in your airflow.cfg, you can run:

```
python ~/airflow/dags/tutorial.py
```

If the script runs without errors, congratulations! Your Dag is set up correctly.

# Testing Task Instances and Dag Runs

You can test specific task instances for a designated logical date. This simulates the scheduler running your task for a particular date and time.

```
# command layout: command subcommand [dag_id] [task_id] [(optional) date]

# testing print_date
airflow tasks test tutorial print_date 2015-06-01

# testing sleep
airflow tasks test tutorial sleep 2015-06-01
```

- Keep in mind that the airflow tasks test command runs task instances locally, outputs their logs to stdout, and doesn’t track state in the database. This is a handy way to test individual task instances.

- Similarly, airflow dags test runs a single Dag run without registering any state in the database, which is useful for testing your entire Dag locally.