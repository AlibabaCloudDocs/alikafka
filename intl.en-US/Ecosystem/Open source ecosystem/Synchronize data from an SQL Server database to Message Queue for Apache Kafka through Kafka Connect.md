# Synchronize data from an SQL Server database to Message Queue for Apache Kafka through Kafka Connect

This topic describes how to synchronize data from an SQL Server database to Message Queue for Apache Kafka through a source connector of Kafka Connect.

## Prerequisites

Ensure that you have completed the following operations:

-   You have downloaded an SQL Server source connector.

    **Note:** To download an SQL Server source connector, visit [Download link](https://repo1.maven.org/maven2/io/debezium/debezium-connector-sqlserver/).

-   You have downloaded Kafka Connect.

    **Note:** Currently, SQL Server source connectors support only Kafka Connect [2.1.0](http://kafka.apache.org/downloads#2.1.0) and later.

-   You have installed Docker.

## Step 1: Configure Kafka Connect

1.  Decompress the downloaded SQL Server source connector package to the specified directory.

2.  In the configuration file connect-distributed.properties of Kafka Connect, configure the plug-in installation path.

    ```
    ## Specify the path where the decompressed plug-in is stored. plugin.path=/kafka/connect/plugins
    ```

    **Note:**

    In Kafka Connect 0.10.2.2 and earlier, the configuration of plugin.path is not supported, and you need to specify the plug-in path in CLASSPATH.

    ```
    export CLASSPATH=/kafka/connect/plugins/sqlserver-connector/*
    ```


## Step 2: Start Kafka Connect

After connect-distributed.properties is configured, run the following command to start Kafka Connect:

```
## Start Kafka Connect.
bin/connect-distributed.sh config/connect-distributed.properties
```

## Step 3: Install SQL Server

**Note:** Versions later than [SQL Server 2016 SP1](https://blogs.msdn.microsoft.com/sqlreleaseservices/sql-server-2016-service-pack-1-sp1-released/) support [change data capture \(CDC\)](https://docs.microsoft.com/en-us/sql/relational-databases/track-changes/about-change-data-capture-sql-server?view=sql-server-2017). Therefore, your SQL Server must be later than this version.

1.  Download [docker-compose-sqlserver.yaml](https://github.com/AliwareMQ/aliware-kafka-demos/blob/master/kafka-connect-demo/SqlserverSourceConnector/docker-compose-sqlserver.yaml).

2.  Run the following command to install SQL Server.

    ```
    docker-compose -f docker-compose-sqlserver.yaml up
    ```


## Step 4: Configure SQL Server

1.  Download [inventory.sql](https://github.com/AliwareMQ/aliware-kafka-demos/blob/master/kafka-connect-demo/SqlserverSourceConnector/inventory.sql).

2.  Run the following command to initialize the test data in the SQL Server database.

    ```
    cat inventory.sql | docker exec -i tutorial_sqlserver_1 bash -c '/opt/mssql-tools/bin/sqlcmd -U sa -P $SA_PASSWORD'
    ```

3.  To listen to existing tables in SQL Server, specify the following configurations:

    1.  Run the following command to enable CDC configuration:

        ```
        ## Enable Database for CDC template
        USE testDB
        GO EXEC sys.sp_cdc_enable_db
        GO
        ```

    2.  Run the following command to enable the CDC configuration for the specified table:

        ```
        ## Enable a Table Specifying Filegroup Option Template
        USE testDB
        GO
        EXEC sys.sp_cdc_enable_table
        @source_schema = N'dbo',
        @source_name   = N'MyTable',
        @role_name     = N'MyRole',
        @filegroup_name = N'MyDB_CT',
        @supports_net_changes = 1
        GO
        ```

    3.  Run the following command to check whether you have the permission to access the CDC table:

        ```
        EXEC sys.sp_cdc_help_change_data_capture
        GO
        ```

        **Note:** If an empty result is returned, check whether you have the permission to access the table.

    4.  Run the following command to check whether the SQL Server agent is enabled:

        ```
        EXEC master.dbo.xp_servicecontrol N'QUERYSTATE',N'SQLSERVERAGENT'
        ```

        **Note:** If `Running` is returned, the SQL Server agent is enabled.


## Step 5: Start the SQL Server source connector

1.  Download [register-sqlserver.json](https://github.com/AliwareMQ/aliware-kafka-demos/blob/master/kafka-connect-demo/SqlserverSourceConnector/register-sqlserver.json).

2.  Edit register-sqlserver.json.

    -   Access from VPC

        ```
        ## The endpoint of the Message Queue for Apache Kafka instance, which can be obtained in the console. ## The default endpoint that you obtained in the console. 
        "database.history.kafka.bootstrap.servers" : "kafka:9092",
        ## You need to create a topic with this name in the console in advance. In this example, create a server1 topic. 
        ## All table change data is recorded in the server1. $DATABASE. $TABLE topics, for example, the server1.testDB.products topic. 
        ## Therefore, you must create all the related topics in the console in advance. 
        "database.server.name": "server1",
        ## Schema changes are recorded in this topic.
        ## You must create this topic in the console in advance. 
        "database.history.kafka.topic": "schema-changes-inventory"
        ```

3.  After configuring register-sqlserver.json, you must create the corresponding topics in the console according to the configuration. For more information about the steps, see [Step 1: Create a topic](/intl.en-US/Quick-start/Step 3: Create resources.md).

    **Note:**

    In SQL Server installed according to this tutorial, you can see that db name:testDB has been created in advance. The SQL Server database contains the following tables:

    -   customers
    -   orders
    -   products
    -   products\_on\_hand
    According to the preceding configuration of register-sqlserver.json, you need to create topics by calling the CreateTopic API operation:

    -   server1
    -   server1.testDB.customers
    -   server1.testDB.orders
    -   server1.testDB.products
    -   server1.testDB.products\_on\_hand
    According to the configuration in register-sqlserver.json, schema change information needs to be stored in schema-changes-testDB. Therefore, you need to create the schema-changes-inventory topic by calling the CreateTopic API operation. For more information about how to create a topic by calling the CreateTopic API operation, see [CreateTopic](/intl.en-US/API reference/Topics/CreateTopic.md).

4.  Run the following command to start SQL Server.

    ```
    > curl -i -X POST -H "Accept:application/json" -H  "Content-Type:application/json" http://localhost:8083/connectors/ -d @register-sqlserver.json
    ```


## Verification

Follow the following steps to check whether Message Queue for Apache Kafka can receive change data from SQL Server.

1.  Change the data in the monitored SQL Server database.

2.  In the console, click **Message Query** and then query the change data on the page that appears.


