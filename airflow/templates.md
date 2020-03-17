# Templates

Every operator has a whitelist that tells which attributes are templatable. This list is set by the attribute **template_fields** on every operator. <br/> <br/>
[List of templatable arguments of operators](https://airflow.readthedocs.io/en/stable/_api/airflow/operators/)

#### Templatable files
Not only strings can be templated, but content of files. Templatable file extensions is set by the attribute **template_ext** on every operator. <br/><br/>
By default only the path of the DAG file is searched for. If we stored our templatable files in a different location, it should be specified in the `template_searchpath` argument while creating the dag.
