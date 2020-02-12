# How to solve: <br/> *Could not load db driver class: com.mysql.jdbc.Driver*?

Reason of failre is missing mysql driver from sqoop sharelib.

#### 1. lists out sqoop jars in its sharelib
`oozie admin -oozie http://localhost:11000/oozie -shareliblist sqoop` <br/>
Check it out if it contains **mysql-connector-java.jar**. Sharelib default path is on HDFS: `/user/oozie/share/lib/`.<br/>
 It can be modified in `/etc/oozie/conf/oozie-site.xml` by overwriting **oozie.service.WorkflowAppService.system.libpath** property.

#### 2. make it executable and accessible

#### 3. update sharelib
`oozie admin -oozie http://localhost:11000/oozie -sharelibupdate`
