# dataproc-hive-samples
A set of common init actions for Hive. 

## add_hive_jars.sh
Add JARs, most commonly UDFs, to Hive aux lib path and bounce required services.

This script performs following actions:
- Copy UDF's JARs to local file system
- In the property file `/etc/hive/conf/hive-site.xml` set `hive.reloadable.aux.jars.path` parameter to `file:///<path/to/jar>/*` value.
    ```    
    bdconfig set_property \
      --configuration_file "/etc/hive/conf/hive-site.xml" \
      --name 'hive.reloadable.aux.jars.path' --value 'file:///<path/to/jar>/*' \
      --clobber
    ```
- For `master` node
  - Update hive-server2 property `/etc/default/hive-server2`. Set `AUX_CLASSPATH` to `file:///<path/to/jar>/*`.
    ``` 
    bdconfig set_property \
      --configuration_file "/etc/default/hive-server2" \
      --name 'AUX_CLASSPATH' --value 'file:///<path/to/jar>/*' \
      --clobber
    ```
  - restart hive-server2 `systemctl restart hive-server2`
- create UDF `CREATE FUNCTION <your_function_name> AS '<fully_qualified_class_name>';`
- There is no need to bounce Hive Metastore.

### Other ways to add UDF to Hive
#### Loading UDF JARs from HDFS
- Copy UDF JARs to HDFS file system (example: `hdfs dfs -cp gs://<bucket>/<path/to/jar>/* <path/to/jar>`)
- Create UDF function
  - Using `ADD JAR`
    - Add JAR to hive `ADD JAR hdfs:///<path/to/jar>.jar`
    - Create function `CREATE FUNCTION <your_function_name> AS '<fully_qualified_class_name>';`
  - Using `USING JAR`
    - Create function `CREATE FUNCTION <your_function_name> AS '<fully_qualified_class_name>' USING JAR 'hdfs:///<path/to/jar>.jar'`;
- There is no need to bounce Hive Metastore or HiveServer2 while using this option

#### Loading UDF JARs from GCS
- Create UDF function
  - Using `ADD JAR`
    - Add JAR to hive `ADD JAR gs://<bucket>/<path/to/jar>.jar`
    - Create function `CREATE FUNCTION <your_function_name> AS '<fully_qualified_class_name>';`
  - Using `USING JAR`
    - Create function `CREATE FUNCTION <your_function_name> AS
                        '<fully_qualified_class_name>' USING JAR
                        'gs://<bucket>/<path/to/jar>.jar'`;
- There is no need to bounce Hive Metastore or HiveServer2 while using this option
- There is no need to create init-action script
