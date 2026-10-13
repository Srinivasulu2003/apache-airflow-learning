# 02 - Why Do We Need Apache Airflow?

## 1. Understanding the Problem

When I write a Python script, I can execute it to perform a specific task. For example, I can write a script to fetch data from an API or save records into a database.

But in real-world projects, a single task is often not enough. I may need to execute multiple scripts in a specific order, schedule them daily, handle failures, and monitor the entire process.

Managing all these requirements manually becomes difficult as the workflow grows.

This is where Apache Airflow becomes useful.

## 2. A Real-World Scenario

Suppose I need to build a daily customer transaction pipeline.

The pipeline has four steps:

1. Extract transactions from an API.
2. Validate and transform the data.
3. Load the processed data into PostgreSQL.
4. Generate a daily report.

The workflow looks like this:

```text
Extract Data
     |
     v
Validate and Transform
     |
     v
Load into PostgreSQL
     |
     v
Generate Report
```

The tasks have dependencies. For example, I should not load the data before transformation is complete.

I need a way to coordinate the execution, schedule the workflow, and identify failures. Apache Airflow helps me manage these requirements.

## 3. What Happens Without Airflow?

Initially, I might create separate Python scripts and execute them manually.

```bash
python extract.py
python transform.py
python load.py
python report.py
```

This approach works for a small workflow, but I need to manage several things myself:

- Run the scripts in the correct order.
- Remember to execute them at the scheduled time.
- Identify which script failed.
- Rerun failed steps safely.
- Maintain logs and track execution history.
- Manage dependencies as the pipeline grows.

I could automate some of these requirements with shell scripts or cron. However, complex workflows may require additional code and monitoring tools.

## 4. What Changes When I Use Airflow?

With Airflow, I define the workflow as a DAG using Python.

I specify the tasks, their dependencies, and the schedule. Airflow then coordinates their execution according to the configuration.

Airflow provides features such as:

- **Scheduling:** Run workflows according to a defined schedule.
- **Dependencies:** Define which tasks must finish before others can run.
- **Retries:** Retry tasks when configured failures occur.
- **Monitoring:** View task status and execution logs.
- **Execution history:** Inspect previous DAG runs and task attempts.
- **Extensibility:** Integrate tasks with databases, APIs, and other systems.

Airflow does not eliminate the need to write processing code. It helps me manage when and how that code is executed.

## 5. Manual Scripts vs Cron vs Airflow

| Feature | Manual scripts | Cron | Apache Airflow |
|---|---|---|---|
| Execute Python code | Yes | Yes | Yes, through tasks |
| Schedule execution | Manual | Yes | Yes |
| Manage task dependencies | Manual | Usually requires extra scripting | Built-in DAG dependencies |
| Retry failed tasks | Manual or custom code | Requires extra scripting | Configurable retries |
| View workflow history | Requires separate setup | Requires separate setup | Built-in UI and metadata |
| Manage complex workflows | Becomes difficult | Possible, but needs extra tooling | Designed for orchestration |

Cron is still a good option for simple scheduled jobs. Airflow is especially useful when multiple tasks depend on one another and I need centralized monitoring and workflow management.

## 6. When Should I Use Apache Airflow?

I would consider Airflow when I need to:

- Run data pipelines on a regular schedule.
- Coordinate multiple dependent tasks.
- Automate ETL or ELT workflows.
- Monitor long-running or recurring jobs.
- Retry failed tasks and investigate errors.
- Orchestrate machine learning training and evaluation pipelines.
- Integrate different systems into a single workflow.

For a simple script that runs once, Airflow may be unnecessary. It introduces additional infrastructure and operational overhead.

## 7. When Should I Avoid Airflow?

Airflow may not be the right choice when:

- I only need to execute a small script occasionally.
- A simple cron job already meets the requirements.
- I need low-latency, continuous event processing.
- I need a distributed data-processing engine rather than an orchestrator.

Airflow supports event-driven scheduling and can orchestrate streaming-related jobs, but it is not itself a real-time stream-processing engine.

## 8. Hands-On Thinking Exercise

Consider this requirement:

"I need to fetch application logs every day, filter failed requests, calculate error statistics, store the results in a database, and generate a report."

Before moving forward, answer these questions:

1. How many tasks would I create?
2. Which tasks depend on other tasks?
3. Could I manage this with cron alone?
4. What would happen if the database task failed?
5. What benefits would Airflow provide for monitoring and retries?

Write your answers in your own words. The goal is to understand the problem before learning how to implement the solution.

## 9. My Learning Summary

After completing this topic, I should understand:

- Why manually coordinating multiple scripts becomes difficult.
- How cron differs from a workflow orchestration platform.
- How Airflow manages dependencies, scheduling, retries, and monitoring.
- When Airflow is useful and when a simpler solution is enough.

**My takeaway:** I would use Apache Airflow when I need to coordinate and monitor a workflow containing multiple tasks, especially when those tasks have dependencies and must run reliably on a recurring schedule.

## References

- [Apache Airflow Documentation](https://airflow.apache.org/docs/)
- [Apache Airflow Core Concepts](https://airflow.apache.org/docs/apache-airflow/stable/core-concepts/index.html)
