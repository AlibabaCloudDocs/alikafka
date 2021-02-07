# Use Kafka Connect to synchronize data from a MySQL database to Message Queue for Apache Kafka

This tutorial shows you how to synchronize data from a MySQL database to Message Queue for Apache Kafka by using a source connector of Kafka Connect.

Kafka Connect is used to import data streams to and export data streams from Message Queue for Apache Kafka. Kafka Connect uses various source connectors to import data from third-party systems to Message Queue for Apache Kafka brokers, and uses various sink connectors to export data from Message Queue for Apache Kafka brokers to third-party systems.

![system](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9053611951/p68623.png)

## Prerequisites

Before you start this tutorial, make sure that the following operations are completed:

-   A MySQL source connector is downloaded.

    **Note:** In this tutorial, MySQL source connector [0.5.2](https://repo1.maven.org/maven2/io/debezium/debezium-connector-mysql/0.5.2/) is used as an example.

-   Kafka Connect is downloaded.

    **Note:** In this tutorial, Kafka Connect [0.10.2.2](http://kafka.apache.org/downloads#0.10.2.2) is used as an example.

-   Docker is installed.

## Step 1: Configure Kafka Connect

1.  Decompress the downloaded MySQL source connector package to the specified directory.

2.  In the configuration file connect-distributed.properties of Kafka Connect, configure the plug-in installation path.

    ```
    plugin.path=/kafka/connect/plugins
    ```

    **Note:**

    In earlier versions of Kafka Connect, plugin.path cannot be configured, and you must specify the plug-in path in CLASSPATH.

    ```
    export CLASSPATH=/kafka/connect/plugins/mysql-connector/*
    ```


## Step 2: Start Kafka Connect

After connect-distributed.properties is configured, run the following commands to start Kafka Connect:

-   Access from the Internet
    1.  Run the command `export KAFKA_OPTS="-Djava.security.auth.login.config=kafka_client_jaas.conf"` to specify java.security.auth.login.config.
    2.  Run the command `bin/connect-distributed.sh config/connect-distributed.properties` to start Kafka Connect.
-   Access from a VPC

    Run the command `bin/connect-distributed.sh config/connect-distributed.properties` to start Kafka Connect.


## Step 3: Install MySQL

1.  Download [docker-compose-mysql.yaml](https://github.com/AliwareMQ/aliware-kafka-demos/blob/master/kafka-connect-demo/MysqlSourceConnect/docker-compose-mysql.yaml).

2.  Run the following commands to install MySQL:

    ```
    export DEBEZIUM_VERSION=0.5
    docker-compose -f docker-compose-mysql.yaml up
    ```


## Step 4: Configure MySQL

1.  Run the following commands to enable binary logging for MySQL and set the binary logging mode to row:

    ```
    [mysqld]
    log-bin=mysql-bin
    binlog-format=ROW
    server_id=1 
    ```

2.  Run the following command to grant permissions to the MySQL user:

    ```
    GRANT SELECT, RELOAD, SHOW DATABASES, REPLICATION SLAVE, REPLICATION CLIENT ON *. * TO 'debezium' IDENTIFIED BY 'dbz';
    ```

    **Note:** In this example, the MySQL user is debezium and the password is dbz.


## Step 5: Start the MySQL source connector

1.  Download [register-mysql.json](https://github.com/AliwareMQ/aliware-kafka-demos/blob/master/kafka-connect-demo/MysqlSourceConnect/register-mysql.json).

2.  Edit register-mysql.json.

    -   Access from a VPC

        ```
        ##  Message Queue for Apache Kafka  endpoints, which are obtained in the Message Queue for Apache Kafka console.
        ## The default endpoint that you obtained in the Message Queue for Apache Kafka console.
        "database.history.kafka.bootstrap.servers" : "kafka:9092",
        ## You must create a topic in the Message Queue for Apache Kafka console with the same name as the specified topic in the MySQL database in advance. In this example, create a topic named server1.
        ## All table change data is recorded in server1. $DATABASE. $TABLE topics, such as the server1.inventory.products topic.
        ## Therefore, you must create all related topics in the Message Queue for Apache Kafka console in advance.
        "database.server.name": "server1",
        ## Schema changes are recorded in this topic.
        ## You must create this topic in the Message Queue for Apache Kafka console in advance.
        "database.history.kafka.topic": "schema-changes-inventory"
        ```

    -   Access from the Internet

        ```
        ##  Message Queue for Apache Kafka  endpoints, which are obtained in the Message Queue for Apache Kafka console. Schema changes of the database are stored here.
        ## The SSL endpoint that you obtained in the Message Queue for Apache Kafka console.
        "database.history.kafka.bootstrap.servers" : "kafka:9092",
        ## You must create a topic in the Message Queue for Apache Kafka console with the same name as the specified topic in the MySQL database in advance. In this example, create a topic named server1.
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

3.  After you configure register-mysql.json, you must create the corresponding topics in the Message Queue for Apache Kafka console based on the configuration. For more information about the steps, see [Step 1: Create a topic](/intl.en-US/Quick-start/Step 3: Create resources.md).

    In MySQL that is installed based on this tutorial, database:inventory is created in advance. The database contains the following tables:

    -   customers
    -   orders
    -   products
    -   products\_on\_hand
    Based on the preceding configuration, you must create the following topics by calling the CreateTopic operation:

    -   server1
    -   server1.inventory.customers
    -   server1.inventory.orders
    -   server1.inventory.products
    -   server1.inventory.products\_on\_hand
    Based on the configuration in register-mysql.json, schema change information needs to be stored in schema-changes-testDB. Therefore, you must create the schema-changes-inventory topic by calling the CreateTopic operation. For more information about how to create a topic by calling the CreateTopic operation, see [CreateTopic](/intl.en-US/API reference/Topic/CreateTopic.md).

4.  Run the following command to start the MySQL source connector:

    ```
    curl -i -X POST -H "Accept:application/json" -H  "Content-Type:application/json" http://localhost:8083/connectors/ -d @register-mysql.json
    ```


## Verify the result

Perform the following steps to check whether Message Queue for Apache Kafka can receive change data from MySQL.

1.  Modify the data of a table in MySQL.

2.  Log on to the Message Queue for Apache Kafka console. On the **Message Query** page, query the table change data.


