# [Airflow 101: Building Your First Workflow](https://airflow.apache.org/docs/apache-airflow/stable/tutorial/fundamentals.html)

In this guide, we'll learn the essential concepts of Airflow, helping you understand how to write your first Dag.

# What is a Dag?

* At its core, a Dag is a collection of tasks organized in a way that reflects their relationships and dependencies.
* It’s like a roadmap for your workflow, showing how each task connects to the others.


## Understanding the Dag Definition File `tutorial.py`

* Think of the Airflow Python script as a configuration file that lays out the structure of your Dag in code. 
* The actual tasks you define here run in a different environment, which means this script isn’t meant for data processing. Its main job is to define the Dag object, and it needs to evaluate quickly since the Dag File Processor checks it regularly for any changes.

