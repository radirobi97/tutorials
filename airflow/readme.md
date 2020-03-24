# Complete Airflow Tutorial
This tutorial based on the book **[Data Pipelines with Apache Airflow](https://www.manning.com/books/data-pipelines-with-apache-airflow)**.

## Table of contents
<!-- TOC depthFrom:1 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [Complete Airflow Tutorial](#complete-airflow-tutorial)
	- [Table of contents](#table-of-contents)
		- [What is Airflow?](#what-is-airflow)
				- [Airflow nomenclature](#airflow-nomenclature)
				- [What is Airflow for?](#what-is-airflow-for)
				- [Airflow components](#airflow-components)
				- [Backfilling](#backfilling)
		- [DAGs](#dags)
				- [Structure of a basic dag](#structure-of-a-basic-dag)
				- [Scheduling workflows](#scheduling-workflows)
				- [Failure](#failure)
					- [In case of task failing](#in-case-of-task-failing)

<!-- /TOC -->

### What is Airflow?
Airflow is a workflow management system. Airflow provides a Python framework to develop data pipelines. It can operate and scale out on multiple machines.<br/> **AIRFLOW IS DESIGNED TO HANDLE BATCH PROCESSING** not for streaming data.

##### Airflow nomenclature

The followings are the most used
- Airflow **DAGs** are composed of **Tasks**
- **Tasks** is created by instantiating an **Operator** class. A configured instance of an Operator becomes a Task
- When a DAG is started, Airflow creates a **DAG Run** entry in its database
- When a Task is executed in the context of a particular DAG Run, then a **Task Instance** is created


##### What is Airflow for?
Airflow ensuring tasks are started in the correct order, sending notifications in case of failure or retry the failed process if desired, monitor the status of tasks, and collect logs of all tasks.

Workflows defined in form of **Directed Acyclic Graphs**. Every rectangle is a `Task`. An example can be seen below.
![branch](./images/airflow_example.png)<br/>

**Dependency terminology using the image above**:
- `branch_a` is **downstream**  of `branching`
- `branching` is an **upstream** of `branch_a`
- arrows indicates the order of execution

##### Airflow components
- webserver => UI  `airflow webserver`
- scheduler => Scheduling tasks `airflow scheduler`
- database  => Storing information `airflow initdb`

##### Backfilling
Backfilling makes possible running workflows back in time. So if there was a change in the logic, it could run on historical data.

<======================================================><br/>
<======================================================>

### DAGs
DAG python files must be located in the **dags** folder by default. It can be overwritten in the config file.
##### Structure of a basic dag
Common includes:
- `import airflow`
- `from airflow import DAG`
- `from airflow.operators.[name] import [name]`

**Steps to create a DAG**<br/>
**1. DAG INSTANCE**<br/>
The DAG is the starting point of any workflow. All tasks within the workflow reference this DAG object so that Airflow knows which tasks belong to which DAG
```python
dag = DAG(
   dag_id="download_rocket_launches",
   start_date=airflow.utils.dates.days_ago(14),
   schedule_interval=None,
)
```

**2. OPERATORS** <br/>
Each operator performs a single unit of work, and multiple operators together form a workflow or DAG in Airflow.

```python
download_launches_task = BashOperator(
   task_id="download_launches",
   bash_command="curl -o /tmp/launches.json 'https://launchlibrary.net/1.4/launch?next=5&mode=verbose'",
   dag=dag,
)
```

**3. DEPENDENCIES** <br/>
Operators run independent of each other, although you can define the order of execution, which we call “dependencies” in Airflow.<br/>

Lets take an example:<br/>
`download_launches_task >> task_two >> task_three` <br/>
This ensures the get_pictures task runs only after download_launches has been completed successfully, and the notify task only runs after get_pictures has completed successfully.


##### Scheduling workflows
We can schedule a DAG to run at certain intervals. This is controlled on the DAG by setting the `schedule_interval` argument:

##### Failure
It’s not uncommon for tasks to fail, which could be for a multitude of reasons
###### In case of task failing
Checking the log is a good practice to find the root of an error. It can be done via the UI by clicking on that particular task and selcet view logs.

Assume that one task failed. it would be unnecessary to restart the entire workflow. A nice feature of Airflow is that you can restart from the point of failure and onwards, without having to restart any previously succeeded tasks. To do this click on **CLEAR** button.
