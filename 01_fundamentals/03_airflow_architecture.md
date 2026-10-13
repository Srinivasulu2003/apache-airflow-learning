# 03 - Apache Airflow Architecture

## 1. Introduction

Apache Airflow consists of multiple components that work together to schedule, execute, and monitor workflows.

In this topic, I will learn how these components communicate with each other and how a task moves from a Python DAG file to successful execution.

Understanding the architecture will help me troubleshoot failed workflows and understand how Airflow is deployed in real-world environments.

## 2. High-Level Architecture

The following diagram represents the main components of a modern Airflow 3 deployment.

```text
              DAG Files (Python)
                      |
                      v
              DAG Processor
                      |
                      v
              Metadata Database
                      ^
                      |
                 Scheduler
                      |
                      v
                   Executor
                      |
                      v
                   Workers
                      |
                      v
              Execute Task Code


              API Server
                  |
                  v
              Web Interface
```

This is a simplified conceptual diagram. The actual communication paths are more detailed: the scheduler reads serialized DAG information from the metadata database, and Airflow 3 workers communicate task execution state through the API server rather than accessing the metadata database directly.

Reference: [Official Airflow Architecture](https://airflow.apache.org/docs/apache-airflow/stable/core-concepts/overview.html)

## 3. Main Components of Airflow

### 3.1 DAG Files

DAG files are Python files in which I define workflows, tasks, schedules, and dependencies.

For example, I might define a workflow that extracts data, transforms it, and loads it into a database.

Airflow reads these files to understand the structure of the workflow.

**What I learned:** A DAG file defines what the workflow should do and how its tasks depend on each other.

### 3.2 DAG Processor

The DAG processor reads and parses DAG files to understand the workflow definitions.

In Airflow 3, the DAG processor runs as a separate component and stores serialized DAG information in the metadata database.

**What I learned:** The DAG processor interprets my Python DAG definitions so Airflow can use them for scheduling.

### 3.3 Scheduler

The scheduler determines when a DAG run should be created and which tasks are ready to execute.

It checks task dependencies and scheduling rules before submitting eligible tasks to the configured executor.

For example, if the extraction task has not succeeded, a downstream transformation task normally cannot run when it requires that extraction task to succeed.

**What I learned:** The scheduler decides when tasks are eligible to run; it does not perform all the actual data processing itself.

### 3.4 Executor

The executor determines how eligible tasks are launched for execution.

The executor is configured as part of Airflow's scheduling setup. Different executors support different execution models.

Examples include:

- **LocalExecutor:** Executes tasks locally using processes.
- **CeleryExecutor:** Distributes tasks to Celery workers.
- **KubernetesExecutor:** Runs tasks in Kubernetes pods.

The right executor depends on workload requirements, infrastructure, and operational needs.

**What I learned:** The executor controls the task execution mechanism, while the scheduler determines which tasks are ready to run.

### 3.5 Workers

Workers execute the task code assigned to them. Separate worker processes or services are used in distributed execution models.

For example, a worker might run Python code that downloads data from an API or executes a transformation.

In a simple local setup, execution may happen on the same machine rather than on separate worker machines.

**What I learned:** Workers perform the actual work defined by my tasks when the selected execution model uses separate workers.

### 3.6 Metadata Database

The metadata database stores information Airflow needs to manage workflows, such as:

- DAG runs and task states.
- Scheduling and execution metadata.
- Connections and variables.
- Other operational information.

PostgreSQL and MySQL are commonly used as metadata database backends.

The metadata database is different from the database where my pipeline stores business data. For example, my DAG could use Airflow's metadata database for orchestration while writing customer transactions to a separate PostgreSQL database.

**What I learned:** The metadata database stores information about Airflow's operation; it is not automatically the destination for my pipeline data.

### 3.7 API Server and Web Interface

The API server provides Airflow's API and serves the web interface used to inspect and manage workflows.

Through the interface, I can:

- View DAGs and their structure.
- Trigger DAG runs.
- Check task status.
- Inspect logs and failures.
- Review execution history.

In Airflow 3, the API server also plays an important role in communication between task execution and Airflow's control plane.

**What I learned:** The web interface helps me interact with Airflow, while the API server provides the underlying API and application functionality.

### 3.8 Triggerer

The triggerer handles deferred tasks by monitoring asynchronous events without requiring a worker to remain occupied while waiting.

For example, a task may need to wait for an external file or another event. With a suitable deferrable operator, the task can defer while the triggerer monitors the event.

**What I learned:** The triggerer improves resource efficiency for supported tasks that spend significant time waiting. It is not required for every basic Airflow setup.

## 4. How the Components Work Together

Consider a daily customer transaction pipeline.

1. I write a Python DAG file containing extraction, transformation, and loading tasks.
2. The DAG processor parses the file and stores the workflow definition in the metadata database.
3. The scheduler determines when the workflow should run and evaluates task dependencies.
4. The executor launches tasks according to the configured execution model.
5. Workers execute the task code where separate workers are used.
6. Task execution states are communicated back to Airflow.
7. The metadata database stores the resulting execution information.
8. I use the web interface to inspect task status and logs.

This process repeats according to the workflow's schedule and configuration.

## 5. Example: What Happens When a Task Fails?

Suppose the extraction task fails because an API is temporarily unavailable.

Airflow can be configured to retry the task. The scheduler and executor coordinate subsequent execution attempts according to the retry configuration.

Downstream tasks that require successful extraction will normally wait until their dependencies are satisfied.

I can inspect the task logs in the web interface to investigate the cause.

**Important:** Retries do not guarantee success. I should also design tasks to handle repeated execution safely, especially when they write data or perform external actions.

## 6. Hands-On Thinking Exercise

Imagine I am running Airflow with KubernetesExecutor.

My DAG downloads a CSV file, transforms its contents, and stores the results in PostgreSQL.

Answer these questions in your own words:

1. Which component reads and parses my DAG file?
2. Which component decides when the workflow should run?
3. What is the executor responsible for?
4. Where does the task code execute with KubernetesExecutor?
5. What information is stored in the metadata database?
6. Where would I look to investigate a failed task?
7. Is the metadata database necessarily the same database that stores the processed CSV data?

Try answering without referring back to the explanations.

## 7. My Learning Summary

After studying this topic, I should understand:

- How Airflow components work together.
- The difference between the DAG processor, scheduler, and executor.
- The role of workers in task execution.
- Why Airflow needs a metadata database.
- How the API server and web interface support workflow management.
- Why the triggerer is useful for deferred tasks.

**My takeaway:** Airflow separates workflow definition, scheduling, execution, and monitoring into cooperating components. Understanding their responsibilities helps me design, debug, and deploy reliable workflows.

## References

- [Airflow Architecture Overview](https://airflow.apache.org/docs/apache-airflow/stable/core-concepts/overview.html)
- [Airflow Core Concepts](https://airflow.apache.org/docs/apache-airflow/stable/concepts/)
- [Airflow Executors](https://airflow.apache.org/docs/apache-airflow/stable/core-concepts/executor/index.html)
