GridDB connector for KairosDB

## Overview

GridDB connector for KairosDB is a module supporting connection between GridDB and KairosDB. By implementing this module, clients are able to interact with GridDB via KairosDB as interface.

[KairosDB](https://github.com/kairosdb/kairosdb) is a time series database with metrics and tags.
And it can be configured to use one of several backends(H2, Cassandra and HBase) for storing data.  
http://kairosdb.github.io/docs/build/html/GettingStarted.html

So, we made it possible to use GridDB as another backend of KairosDB.

## Operating environment

Library building and program execution are checked in the environment below.

    OS:         CentOS6.7(x64)
    Java:       JDK 1.8.0_101
    GridDB:     Version 3.0
	KairosDB:   Version 1.1.1

## QuickStart
### Preparations

1. Install KairosDB

        $ curl -O --location https://github.com/kairosdb/kairosdb/archive/v1.1.1.tar.gz 
        $ tar xfvz v1.1.1.tar.gz 
        $ cd kairosdb-1.1.1

2. Copy GridDB connector for KairosDB under KairosDB src folder
   
        $ cp -r <src/main/java/org/kairosdb/datastore/griddb folder in GridDB connector> src/main/java/org/kairosdb/datastore/.

3. Please edit file ivy.xml

		<!-- https://mvnrepository.com/artifact/org.apache.commons/commons-pool2 -->
		<dependency org="org.apache.commons" name="commons-pool2" rev="2.0"/>

4. Please edit file src/main/resources/logback.xml
 
        <logger name="com.toshiba.mwcloud.gs.GridStoreLogger" level="INFO"/>

5. Please modify file src/main/resources/kairosdb.properties
 
        #kairosdb.service.datastore=org.kairosdb.datastore.h2.H2Module
        kairosdb.service.datastore=org.kairosdb.datastore.griddb.GriddbModule

        ...
 
        #===============================================================================
        #GridDB properties
        kairosdb.datastore.griddb.cluster_name=<GridDB cluster name>
        kairosdb.datastore.griddb.user=<GridDB user name>
        kairosdb.datastore.griddb.password=<GridDB password>
        #Define address and port for multicast method, leave it blank if using other method
        kairosdb.datastore.griddb.notification_address=<GridDB notification address(default is 239.0.0.1)>
        kairosdb.datastore.griddb.notification_port=<GridDB notification port(default is 31999)>
        #Define nodes fixed list method, leave it blank if using other method, node list is in the form> 1.1.1.1:10001,1.1.1.2:10001
        kairosdb.datastore.griddb.notification_member=
        #Define url for provider method, leave it blank if using other method
        kairosdb.datastore.griddb.notification_provider_url=
        kairosdb.datastore.griddb.consistency=
        kairosdb.datastore.griddb.container_cache_size=
        kairosdb.datastore.griddb.data_affinity_pattern=
        kairosdb.datastore.griddb.failover_timeout=
        kairosdb.datastore.griddb.transaction_timeout=
        kairosdb.datastore.griddb.datapoint_ttl=0
        kairosdb.datastore.griddb.max_data_cache_size=50000
        kairosdb.datastore.griddb.max_write_buffer_size=500000
        kairosdb.datastore.griddb.number_request_concurrency=100
        kairosdb.datastore.griddb.write_delay=1000
        kairosdb.datastore.griddb.write_buffer_datapoint=8

    Please refer to [Configuration](Configuration.md) for GridDB properties.

    Note: Make sure that GriddbModule configuration line in kairosdb.properties file is uncommented.

6. Build a GridDB Java client and place the created gridstore.jar under the following folder.

     lib/

### Build and Run

GridDB cluster needs to be started in advance.

Execute the command on kairosdb-1.1.1 folder.
  
     $ java -cp tools/tablesaw-1.2.2.jar make run


## Caution and Limitation

  * Caution  
    Datapoint is stored in time series type container assigned for each different set of metric name, tag name and tag value.
    Datapoint time to live in seconds (TTL) is only applied to time series type container at creation time of container.
  * Limitation  
    Metric name, tag name, tag value in current database schema can only include alphabet and numerical characters, cannot contain special ones. The metric name should not be case sensitive and has to start with alphabet characters.

## Community

  * Issues  
    Use the GitHub issue function if you have any requests, questions, or bug reports. 
  * PullRequest  
    Use the GitHub pull request function if you want to contribute code.
    You'll need to agree GridDB Contributor License Agreement(CLA_rev1.1.pdf).
    By using the GitHub pull request function, you shall be deemed to have agreed to GridDB Contributor License Agreement.

## License
  
  The GridDB connector for KairosDB source license is Apache License, version 2.0.
