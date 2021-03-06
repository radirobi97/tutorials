# What is airflow?
Articles:
- [Beginners guide](https://medium.com/@itunpredictable/apache-airflow-on-docker-for-complete-beginners-cf76cf7b2c9a)

It’s a general purpose workflow scheduler. Most of the time it is used as an ETL tool.

### Components:
- `Operators`: different types of things that you can do in workflows
- `Tasks`: instances of an operators
- `DAG`: made up from tasks
- `XComs`: communicate between tasks in form of key/values pairs
	- **ONLY FOR SMALL PIECES OF DATA**
- `Variables`: they have a global scope so they are used most of the time for configuration
- `Connections`: makes possible to connect to databases
- `Hooks`: connection to our Connections

Basic DAG looks like the following:
```python
#imports
from airflow import DAG
from airflow.operators import PythonOperator
from datetime import datetime

dag = DAG(
	dag_id = 'my_first_dag',
	start_date = datetime(2019,1,15),
	schedule_interval = '0 2 * * *')

def print_hello():
	return "hello!"

def print_goodbye():
	return "goodbye!"

print_hello = PythonOperator(
	task_id = 'print_hello',
	#python_callable param points to the function you want to run
	python_callable = print_hello,
	#dag param points to the DAG that this task is a part of
	dag = dag)

print_goodbye = PythonOperator(
	task_id = 'print_goodbye',
	python_callable = print_goodbye,
	dag = dag)

#Assign the order of the tasks in our DAG
print_hello >> print_goodbye
```

#### Lifecycle of a task
![lifecycle](./images/task_lifecycle_diagram.png)


#### Default arguments
If a dictionary of **default_args** is passed to a DAG, it will apply them to any of its operators. This makes it easy to apply a common parameter to many operators without having to type it many times.

#### Airflow database/webserver
Airflow has its own database to store credentials and history. It can be initialized `airflow initdb`. <br/>
Airflow has a front-end UI. It can be started with `airflow webserver`.

#### Airflow executor:
They actually executes our tasks. These are configurable. Default executor is Sequential Exectuor. For example airflow can operate on multiple worker nodes. Blogs about it:
- [guide to build airflow server](http://stlong0521.github.io/20161023%20-%20Airflow.html)

#### Scheduler
- to run for example periodically in every ten minutes: `timedelta(minutes=10)`
- cron based notation can be used: `* * * * *`
- to call airflow specific variables: `{{variable_name}}`
	- `{{execution_date.strftime('%Y-%m-%d')}}`: execution date in a given format
			- for example there are variables that can be used only in a given mode. `execution_date` is usable if the workflow has been started by scheduler, not by manually

#### Best practices
- partition data into smaller pieces [for example one file by day]
- using specific variables to process always the actual new coming data
