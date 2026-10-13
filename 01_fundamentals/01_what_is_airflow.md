# 01 - What Is Apache Airflow?

## 1. Understanding Apache Airflow

Apache Airflow is an open-source platform used to **author, schedule, and monitor workflows**.

In simple terms, Airflow helps me automate a sequence of tasks instead of running each task manually.

For example, imagine I need to fetch customer data from an API every day, clean the data, store it in a database, and generate a report. Airflow helps me define the task order, schedule the workflow, and monitor whether each task succeeds or fails.

**Key concept:** Airflow manages the execution of tasks. The actual work is performed by the code, SQL queries, APIs, or external systems used by those tasks.

### What I learned

- Airflow is used for workflow orchestration.
- I can define workflows using Python.
- I can schedule and monitor tasks.
- I can manage task dependencies and failures.

## 2. What Is a Workflow?

A workflow is a sequence of tasks that work together to achieve a goal.

Consider a daily customer data pipeline:

```text
Fetch Data
    |
    v
Clean Data
    |
    v
Save to Database
    |
    v
Generate Report
```

Each step is a task. The workflow defines how these tasks are connected and in what order they should run.

In Airflow, I define this workflow as a **DAG (Directed Acyclic Graph)**.

- **Directed:** Dependencies have a direction.
- **Acyclic:** Dependencies cannot form a cycle.
- **Graph:** Tasks are connected through relationships.

### What I learned

A workflow contains tasks, and a DAG describes the tasks and their dependencies.

## 3. What Is Workflow Orchestration?

Workflow orchestration means coordinating multiple tasks, controlling when they execute, and managing their dependencies.

For example, I should not load customer data into a database before the extraction task has completed successfully.

Airflow lets me define this dependency:

```text
Extract Data → Transform Data → Load Data
```

Airflow determines when a task is eligible to run based on its dependencies and other scheduling and execution rules.

It also supports retries, execution logs, and monitoring.

### What I learned

Orchestration is about coordinating the work, not necessarily performing the work itself.

## 4. Why Do I Need Airflow?

Suppose I have three Python scripts:

- `extract.py` fetches data.
- `transform.py` cleans the data.
- `load.py` saves the data to a database.

Initially, I might run each script manually. Later, I could schedule a script using cron, but coordinating more complex workflows and monitoring failures can become difficult.

Airflow provides a central place to define dependencies, schedule workflows, configure retries, and monitor execution.

| Approach | What it provides |
|---|---|
| Manual execution | I start each script myself. |
| Cron | I schedule commands at specified times. |
| Apache Airflow | I define, schedule, coordinate, and monitor workflows with task dependencies. |

Cron is still useful for simple jobs. Airflow becomes especially useful when workflows have multiple dependent tasks and operational monitoring requirements.

### What I learned

Airflow helps me manage complex workflows more systematically than manually coordinating individual scripts.

## 5. Real-World Example: Customer Data Pipeline

Suppose I work on a data engineering project that processes customer transactions every day.

The pipeline needs to:

1. Fetch transactions from an API.
2. Validate and clean the data.
3. Store the results in PostgreSQL.
4. Generate a daily summary.

Airflow can schedule this workflow, manage task dependencies, and provide visibility into execution status and logs.

If extraction fails, I can configure retries. Downstream tasks can be configured to wait for successful upstream tasks.

### What I learned

Airflow is useful when a business process requires several tasks to execute reliably in a defined order.

## 6. What Airflow Does and Does Not Do

| Airflow does | Airflow does not primarily do |
|---|---|
| Schedule workflows | Process large datasets like Spark |
| Manage task dependencies | Replace Python or SQL |
| Track execution status | Act as the main database for pipeline data |
| Support retries and logging | Guarantee that every task will succeed |
| Coordinate external systems | Automatically make processing faster |

### What I learned

Airflow is an orchestration platform. I still need to write or configure the actual processing logic.

## 7. Hands-On Thinking Exercise

Consider this requirement:

"I want to download a CSV file every morning, remove invalid records, save the cleaned data into PostgreSQL, and send a notification when processing finishes."

Answer these questions in your own words:

1. How many tasks would I create?
2. Which task should execute first?
3. Which tasks depend on the previous task?
4. What should happen if downloading the CSV fails?
5. How could Airflow help me monitor this workflow?

Try answering before looking up additional examples.

## 8. My Learning Summary

After studying this topic, I should be able to explain:

- What Apache Airflow is.
- What a workflow and a DAG mean.
- What workflow orchestration means.
- Why Airflow is useful for multi-step pipelines.
- The difference between orchestration and data processing.

**My takeaway:** I understand Apache Airflow as a tool for defining, scheduling, coordinating, and monitoring workflows. My next step is to understand when Airflow is needed and how it compares with manual scripts and cron jobs.

## References

- [Apache Airflow Documentation](https://airflow.apache.org/docs/)
- [Official Airflow Tutorial](https://airflow.apache.org/docs/apache-airflow/stable/tutorial/index.html)
