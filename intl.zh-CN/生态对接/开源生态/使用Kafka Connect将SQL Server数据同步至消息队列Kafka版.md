# 使用Kafka Connect将SQL Server数据同步至消息队列Kafka版

本教程介绍如何使用Kafka Connect的Source Connector将SQL Server的数据同步至消息队列Kafka版。

## 前提条件

在开始本教程前，请确保您已完成以下操作：

-   已下载SQL Server Source Connector。具体信息，请参见[SQL Server Source Connector](https://repo1.maven.org/maven2/io/debezium/debezium-connector-sqlserver/)。
-   已下载Kafka Connect。具体信息，请参见[Kafka Connect](http://kafka.apache.org/downloads#2.1.0)。

    **说明：** SQL Server Source Connector目前只支持2.1.0及以上版本的Kafka Connect。

-   已下载Docker。具体信息，请参见[Docker](https://www.docker.com/products/docker-desktop)。

## 步骤一：配置Kafka Connect

1.  将下载完成的SQL Server Connector解压到指定目录。

2.  在Kafka Connect的配置文件connect-distributed.properties中配置插件安装位置。

    ```
    ## 指定插件解压后的路径。
    plugin.path=/kafka/connect/plugins
    ```

    **说明：**

    Kafka Connect的早期版本不支持配置plugin.path，您需要在CLASSPATH中指定插件位置。

    ```
    export CLASSPATH=/kafka/connect/plugins/sqlserver-connector/*
    ```


## 步骤二：启动Kafka Connect

配置好connect-distributed.properties后，执行以下命令启动Kafka Connect。

1.  如果是公网接入，需先设置java.security.auth.login.config，如果是VPC接入，可以跳过这一步。

    ```
    export KAFKA_OPTS="-Djava.security.auth.login.config=kafka_client_jaas.conf"
    ```

2.  启动Kafka Connect。

    ```
    bin/connect-distributed.sh config/connect-distributed.properties
    ```


## 步骤三：安装SQL Server

**说明：** [SQL Server 2016 SP1](https://blogs.msdn.microsoft.com/sqlreleaseservices/sql-server-2016-service-pack-1-sp1-released/)以上版本支持[CDC](https://docs.microsoft.com/en-us/sql/relational-databases/track-changes/about-change-data-capture-sql-server?view=sql-server-2017)，因此您的SQL Server版本必须高于该版本。

1.  下载[docker-compose-sqlserver.yaml](https://github.com/AliwareMQ/aliware-kafka-demos/blob/master/kafka-connect-demo/SqlserverSourceConnector/docker-compose-sqlserver.yaml)。

2.  执行以下命令安装SQL Server。

    ```
    docker-compose -f docker-compose-sqlserver.yaml up
    ```


## 步骤四：配置SQL Server

1.  下载[inventory.sql](https://github.com/AliwareMQ/aliware-kafka-demos/blob/master/kafka-connect-demo/SqlserverSourceConnector/inventory.sql)。

2.  执行以下命令初始化SQL Server中的测试数据。

    ```
    cat inventory.sql | docker exec -i tutorial_sqlserver_1 bash -c '/opt/mssql-tools/bin/sqlcmd -U sa -P $SA_PASSWORD'
    ```

3.  如果您需要监听SQL Server中已有的数据表，请完成以下配置：

    1.  执行以下命令开启CDC配置。

        ```
        ## 开启CDC模板数据库。
        USE testDB
        GO
        EXEC sys.sp_cdc_enable_db
        GO
        ```

    2.  执行以下命令开启指定Table的CDC配置。

        ```
        ## 开启指定Table的CDC配置。
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

    3.  执行以下命令确认是否有权限访问CDC Table。

        ```
        EXEC sys.sp_cdc_help_change_data_capture
        GO
        ```

        **说明：** 如果返回结果为空，您需要确认是否有权限访问该表。

    4.  执行以下命令确认SQL Server Agent已开启。

        ```
        EXEC master.dbo.xp_servicecontrol N'QUERYSTATE',N'SQLSERVERAGENT'
        ```

        **说明：** 如果返回结果为`Running`，则说明SQL Server Agent已开启。


## 步骤五：启动SQL Server Connector

1.  下载[register-sqlserver.json](https://github.com/AliwareMQ/aliware-kafka-demos/blob/master/kafka-connect-demo/SqlserverSourceConnector/register-sqlserver.json)。

2.  编辑register-sqlserver.json。

    -   VPC接入

        ```
        ## 消息队列Kafka版实例的默认接入点，您可以在消息队列Kafka版控制台获取。
        "database.history.kafka.bootstrap.servers" : "kafka:9092",
        ## 您需要提前在消息队列Kafka版控制台创建同名Topic，在本例中创建topic：server1。
        ## 所有table的变更数据，会记录在server1.$DATABASE.$TABLE的topic中，例如server1.testDB.products。
        ## 因此您需要提前在消息队列Kafka版控制台中创建所有相关Topic。
        "database.server.name": "server1",
        ## 记录schema变化信息将记录在该Topic中。
        ## 您需要提前在消息队列Kafka版控制台创建该Topic。
        "database.history.kafka.topic": "schema-changes-inventory"
        ```

    -   公网接入

        ```
        ## 消息队列Kafka版实例的SSL接入点，您可以在消息队列Kafka版控制台获取。
        "database.history.kafka.bootstrap.servers" : "kafka:9092",
        ## 您需要提前在消息队列Kafka版控制台创建同名Topic，在本例中创建topic：server1。
        ## 所有table的变更数据，会记录在server1.$DATABASE.$TABLE的Topic中，例如server1.testDB.products。
        ## 因此您需要提前在消息队列Kafka版控制台中创建所有相关Topic。
        "database.server.name": "server1",
        ## 记录schema变化信息将记录在该Topic中。
        ## 您需要提前在消息队列Kafka版控制台创建该Topic。
        "database.history.kafka.topic": "schema-changes-inventory",
        ## 通过SSL接入点访问，还需要修改以下配置。
        "database.history.producer.ssl.truststore.location": "kafka.client.truststore.jks",
        "database.history.producer.ssl.truststore.password": "KafkaOnsClient",
        "database.history.producer.security.protocol": "SASL_SSL",
        "database.history.producer.sasl.mechanism": "PLAIN",
        "database.history.consumer.ssl.truststore.location": "kafka.client.truststore.jks",
        "database.history.consumer.ssl.truststore.password": "KafkaOnsClient",
        "database.history.consumer.security.protocol": "SASL_SSL",
        "database.history.consumer.sasl.mechanism": "PLAIN",
        ```

3.  完成register-sqlserver.json配置后，您需要根据配置在控制台创建相应的Topic，相关操作步骤请参见[步骤一：创建Topic](/intl.zh-CN/快速入门/步骤三：创建资源.md)。

    按照本教程中的方式安装的SQL Server，您可以看到SQL Server中已经提前创建db name：testDB。其中有四张表：

    -   customers
    -   orders
    -   products
    -   products\_on\_hand
    根据以上register-sqlserver.json的配置，您需要使用OpenAPI创建Topic：

    -   server1
    -   server1.testDB.customers
    -   server1.testDB.orders
    -   server1.testDB.products
    -   server1.testDB.products\_on\_hand
    在register-sqlserver.json中，配置了将schema变化信息记录在schema-changes-testDB，因此您还需要使用OpenAPI创建Topic：schema-changes-inventory，相关操作请参见[CreateTopic](/intl.zh-CN/API参考/Topic/CreateTopic.md)。

4.  执行以下命令启动SQL Server。

    ```
    curl -i -X POST -H "Accept:application/json" -H  "Content-Type:application/json" http://localhost:8083/connectors/ -d @register-sqlserver.json
    ```


## 结果验证

确认消息队列Kafka版能否接收到SQL Server的变更数据：

1.  变更监听SQL Server中的数据。

2.  在控制台的**消息查询**页面，查询变更消息。具体操作步骤，请参见[查询消息](/intl.zh-CN/用户指南/查询消息.md)。


