# Use Kafka Connect to synchronize data from an SQL Server database to Message Queue for Apache Kafka

This tutorial shows you how to synchronize data from an SQL Server database to Message Queue for Apache Kafka by using a source connector of Kafka Connect.

## Prerequisites

Before you start this tutorial, make sure that the following operations are completed:

-   An SQL Server source connector is downloaded. For more information, see [SQL Server Source Connector](https://repo1.maven.org/maven2/io/debezium/debezium-connector-sqlserver/).
-   Kafka Connect is downloaded. For more information, see [Kafka Connect](http://kafka.apache.org/downloads#2.1.0).

    **Note:** SQL Server source connectors support only Kafka Connect 2.1.0 and later.

-   Docker is downloaded. For more information, see [Docker](https://www.docker.com/products/docker-desktop).

## Step 1: Configure Kafka Connect

1.  Decompress the downloaded SQL Server source connector package to the specified directory.

2.  In the configuration file connect-distributed.properties of Kafka Connect, configure the plug-in installation path.

    ```
    ## Specify the path where the decompressed plug-in is stored.
    plugin.path=/kafka/connect/plugins
    ```

    **Note:**

    In earlier versions of Kafka Connect, plugin.path cannot be configured, and you must specify the plug-in path in CLASSPATH.

    ```
    export CLASSPATH=/kafka/connect/plugins/sqlserver-connector/*
    ```


## Step 2: Start Kafka Connect

After connect-distributed.properties is configured, run the following commands to start Kafka Connect:

```
## For access from the Internet, configure java.security.auth.login.config first.
## For access from a virtual private cloud (VPC), skip this step.
export KAFKA_OPTS="-Djava.security.auth.login.config=kafka_client_jaas.conf"
## Start Kafka Connect.
bin/connect-distributed.sh config/connect-distributed.properties
```

```
## Start Kafka Connect.
bin/connect-distributed.sh config/connect-distributed.properties
```

## Step 3: Install SQL Server

**Note:** Versions later than [SQL Server 2016 SP1](https://blogs.msdn.microsoft.com/sqlreleaseservices/sql-server-2016-service-pack-1-sp1-released/) support [change data capture \(CDC\)](https://docs.microsoft.com/en-us/sql/relational-databases/track-changes/about-change-data-capture-sql-server?view=sql-server-2017). Therefore, your SQL Server must be later than this version.

1.  Download [docker-compose-sqlserver.yaml](https://github.com/AliwareMQ/aliware-kafka-demos/blob/master/kafka-connect-demo/SqlserverSourceConnector/docker-compose-sqlserver.yaml).

2.  Run the following command to install SQL Server:

    ```
    docker-compose -f docker-compose-sqlserver.yaml up
    ```


## Step 4: Configure SQL Server

1.  Download [inventory.sql](https://github.com/AliwareMQ/aliware-kafka-demos/blob/master/kafka-connect-demo/SqlserverSourceConnector/inventory.sql).

2.  Run the following command to initialize the test data in the SQL Server database:

    ```
    cat inventory.sql | docker exec -i tutorial_sqlserver_1 bash -c '/opt/mssql-tools/bin/sqlcmd -U sa -P $SA_PASSWORD'
    ```

3.  To listen to existing tables in SQL Server, complete the following configurations:

    1.  Run the following commands to enable CDC:

        ```
        ## Enable CDC for the database.
        USE testDB
        GO
        EXEC sys.sp_cdc_enable_db
        GO
        ```

    2.  Run the following commands to enable CDC for a specified table:

        ```
        ## Enable CDC for a specified table.
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

    3.  Run the following commands to check whether you have the permissions to access the CDC table:

        ```
        EXEC sys.sp_cdc_help_change_data_capture
        GO
        ```

        **Note:** If no result is returned, check whether you have the permissions to access the table.

    4.  Run the following command to check whether the SQL Server agent is enabled:

        ```
        EXEC master.dbo.xp_servicecontrol N'QUERYSTATE',N'SQLSERVERAGENT'
        ```

        **Note:** If `Running` is returned, the SQL Server agent is enabled.


## Step 5: Start the SQL Server source connector

1.  Download [register-sqlserver.json](https://github.com/AliwareMQ/aliware-kafka-demos/blob/master/kafka-connect-demo/SqlserverSourceConnector/register-sqlserver.json).

2.  Edit register-sqlserver.json.

    -   Access from a VPC

        ```
        ## The default endpoint of the Message Queue for Apache Kafka instance, which can be obtained in the Message Queue for Apache Kafka console.
        "database.history.kafka.bootstrap.servers" : "kafka:9092",
        ## You must create a topic in the Message Queue for Apache Kafka console with the same name as the specified topic in the SQL Server database in advance. In this example, create a topic named server1.
        ## All table change data is recorded in server1. $DATABASE. $TABLE topics, such as the server1.testDB.products topic.
        ## Therefore, you must create all related topics in the Message Queue for Apache Kafka console in advance.
        "database.server.name": "server1",
        ## Schema changes are recorded in this topic.
        ## You must create this topic in the Message Queue for Apache Kafka console in advance.
        "database.history.kafka.topic": "schema-changes-inventory"
        ```

    -   Access from the Internet

        ```
        ## The SSL endpoint of the Message Queue for Apache Kafka instance, which can be obtained in the Message Queue for Apache Kafka console.
        "database.history.kafka.bootstrap.servers" : "kafka:9092",
        ## You must create a topic in the Message Queue for Apache Kafka console with the same name as the specified topic in the SQL Server database in advance. In this example, create a topic named server1.
        ## All table change data is recorded in server1. $DATABASE. $TABLE topics, such as the server1.testDB.products topic.
        ## Therefore, you must create all related topics in the Message Queue for Apache Kafka console in advance.
        "database.server.name": "server1",
        ## Schema changes are recorded in this topic.
        ## You must create this topic in the Message Queue for Apache Kafka console in advance.
        "database.history.kafka.topic": "schema-changes-inventory",
        ## Modify the following configurations for SSL-based Internet access:
        "database.history.producer.ssl.truststore.location": "kafka.client.truststore.jks",
        "database.history.producer.ssl.truststore.password": "KafkaOnsClient",
        "database.history.producer.security.protocol": "SASL_SSL",
        "database.history.producer.sasl.mechanism": "PLAIN",
        "database.history.consumer.ssl.truststore.location": "kafka.client.truststore.jks",
        "database.history.consumer.ssl.truststore.password": "KafkaOnsClient",
        "database.history.consumer.security.protocol": "SASL_SSL",
        "database.history.consumer.sasl.mechanism": "PLAIN",
        ```

3.  After you configure register-sqlserver.json, you must create the corresponding topics in the console based on the configuration. For more information about the steps, see [Step 1: Create a topic](/intl.en-US/Quick-start/Step 3: Create resources.md).

    In SQL Server installed based on this tutorial, db name:testDB is created in advance. The database contains the following tables:

    -   customers
    -   orders
    -   products
    -   products\_on\_hand
    Based on the preceding configuration of register-sqlserver.json, you must create topics by calling the CreateTopic operation:

    -   server1
    -   server1.testDB.customers
    -   server1.testDB.orders
    -   server1.testDB.products
    -   server1.testDB.products\_on\_hand
    Based on the configuration in register-sqlserver.json, schema change information needs to be stored in schema-changes-testDB. Therefore, you must create the schema-changes-inventory topic by calling the CreateTopic operation. For more information, see [CreateTopic](/intl.en-US/API reference/Topic/CreateTopic.md).

4.  Run the following command to start SQL Server:

    ```
    curl -i -X POST -H "Accept:application/json" -H  "Content-Type:application/json" http://localhost:8083/connectors/ -d @register-sqlserver.json
    ```


## Verify the result

Perform the following steps to check whether Message Queue for Apache Kafka can receive change data from SQL Server:

1.  Modify the data in the monitored SQL Server database.

2.  Log on to the Message Queue for Apache Kafka console. On the **Message Query** page, query the table change data. For more information, see [Query messages](/intl.en-US/User guide/Query messages.md).


