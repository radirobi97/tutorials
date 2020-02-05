# Oozie

### What exactly Oozie is?
Workflow scheduling system for managing Hadoop jobs. If we want to run any kind of job we always need additional configuration files. Namely:
- `job.properties`: parameters used in **workflow.xml** can be defined here | it should be in local filesystem
- `workflow.xml`: it describes that what should be running | it should be in HDFS


#### What kind of jobs can be executed via Oozie?
- map reduce
- pig
- hive
- shell script
- Java code
- ssh
- sqoop

These are the so called action nodes in Oozie.

#### Control-flow nodes
- start: starting the whole process
- kill: killing the job in case of error
- fork: calling multiple actions
- join: merge multiple actions into one
- decision: making decision
- end: end the whole process

#### workflow.xml
`${parameter_name}` will be read from job.properties file. <br/>
<br/>`<job-tracker>${jobTracker}</job-tracker>`<br/>
Oozie schedules jobs so it should know about YARN resource manager. YARN resource manager is the one who is responsible for starting the proper job.

`<name-node>${nameNode}</job-tracker>`<br/>
Oozie also should have information about HDFS name node.

`<ok to="end"/>`<br/>
If there were no errors go to end. It can go to a following action as well.

`<error to="fail"/>`<br/>
It goes to fail in case of errors.

`<kill name="fail">`<br/>
`<message>`This is the message in case of fail.`</message>`<br/>
`</kill>`
