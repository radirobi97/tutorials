## How to pass parameters which using airflow variables to the python function

```python
def _calculate_stats(**context):
   """Calculates event statistics."""
   input_path = context["templates_dict"]["input_path"]
   output_path = context["templates_dict"]["output_path"]

   events = pd.read_json(input_path)
   stats = events.groupby(["date", "user"]).size().reset_index()
   stats.to_csv(output_path, index=False)


calculate_stats = PythonOperator(
   task_id="calculate_stats",
   python_callable=_calculate_stats,
   templates_dict={
       "input_path": "data/events/{{ds}}.json",
       "output_path": "data/stats/{{ds}}.csv",
   },
   provide_context=True,
   dag=dag,
)
```
**templates_dict, provide_context** must be defined as above.

#### Basic function parameters to Python operator
- `op_args`: serves as non-keyword parameters
- `op_kwargs`: key-value pairs can be defined

```python
def _calculate_stats(input_path, output_path):
    """Calculates event statistics."""
    events = pd.read_json(input_path)
    stats = events.groupby(["date", "user"]).size().reset_index()
    stats.to_csv(output_path, index=False)

calculate_stats = PythonOperator(
    task_id="calculate_stats",
    python_callable=_calculate_stats,
    op_kwargs={
        "input_path": "data/events.json",
        "output_path": "data/stats.csv",
    },
    dag=dag,
)
```
##### Print out parameters of the called funtion in PythonOperator
- this can be done on the UI after the particular dag has been started by scheduler
- or anytime via CLI
  - `airflow render [dag_id] [pythonoperator_id] [value_we_want_to_pass_to_the_function]`
