# 使用Kafka Connect将MySQL数据同步至消息队列Kafka版

本教程介绍如何使用Kafka Connect的Source Connector将MySQL的数据同步至消息队列Kafka版。

Kafka Connect主要用于将数据流输入和输出消息队列Kafka版。Kafka Connect主要通过各种Source Connector的实现，将数据从第三方系统输入到Kafka broker，通过各种Sink Connector实现，将数据从Kafka broker中导入到第三方系统。

![system](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7219853951/p68623.png)

## 前提条件

在开始本教程前，请确保您已完成以下操作：

-   下载MySQL Source Connector。

    **说明：** 本教程以[0.5.2](https://repo1.maven.org/maven2/io/debezium/debezium-connector-mysql/0.5.2/)版本的MySQL Source Connector为例。

-   下载Kafka Connect。

    **说明：** 本教程以[0.10.2.2](http://kafka.apache.org/downloads#0.10.2.2)版本的Kafka Connect为例。

-   安装docker。

## 步骤一：配置Kafka Connect

1.  将下载完成的MySQL Connector解压到指定目录。

2.  在Kafka Connect的配置文件connect-distributed.properties中配置插件安装位置。

    ```
    plugin.path=/kafka/connect/plugins
    ```

    **说明：**

    Kafka Connect的早期版本不支持配置plugin.path，您需要在CLASSPATH中指定插件位置。

    ```
    export CLASSPATH=/kafka/connect/plugins/mysql-connector/*
    ```


## 步骤二：启动Kafka Connect

在配置好connect-distributed.properties后，执行以下命令启动Kafka Connect。

-   公网接入
    1.  执行命令`export KAFKA_OPTS="-Djava.security.auth.login.config=kafka_client_jaas.conf"`设置java.security.auth.login.config。
    2.  执行命令`bin/connect-distributed.sh config/connect-distributed.properties`启动Kafka Connect。
-   VPC接入

    执行命令`bin/connect-distributed.sh config/connect-distributed.properties`启动Kafka Connect。


## 步骤三：安装MySQL

1.  下载[docker-compose-mysql.yaml](https://github.com/AliwareMQ/aliware-kafka-demos/blob/master/kafka-connect-demo/MysqlSourceConnect/docker-compose-mysql.yaml)。

2.  执行以下命令安装MySQL。

    ```
    export DEBEZIUM_VERSION=0.5
    docker-compose -f docker-compose-mysql.yaml up
    ```


## 步骤四：配置MySQL

1.  执行以下命令开启MySQL的binlog写入功能，并配置binlog模式为row。

    ```
    [mysqld]
    log-bin=mysql-bin
    binlog-format=ROW
    server_id=1 
    ```

2.  执行以下命令设置MySQL的User权限。

    ```
    GRANT SELECT, RELOAD, SHOW DATABASES, REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'debezium' IDENTIFIED BY 'dbz';
    ```

    **说明：** 示例中MySQL的User为debezium，密码为dbz。


## 步骤五：启动MySQL Connector

1.  下载[register-mysql.json](https://github.com/AliwareMQ/aliware-kafka-demos/blob/master/kafka-connect-demo/MysqlSourceConnect/register-mysql.json)。

2.  编辑register-mysql.json。

    -   VPC接入

        ```
        ## 消息队列Kafka版接入点，通过控制台获取。
        ## 您在控制台获取的默认接入点。
        "database.history.kafka.bootstrap.servers" : "kafka:9092",
        ## 需要提前在控制台创建同名Topic，在本例中创建topic：server1。
        ## 所有Table的变更数据，会记录在server1.$DATABASE.$TABLE的Topic中，如 server1.inventory.products。
        ## 因此用户需要提前在控制台中创建所有相关 Topic。
        "database.server.name": "server1",
        ## 记录schema变化信息将记录在这个Topic中。
        ## 需要提前在控制台创建。
        "database.history.kafka.topic": "schema-changes-inventory"
        ```

    -   公网接入

        ```
        ## 消息队列Kafka版接入点，通过控制台获取。存储db中schema变化信息。
        ## 您在控制台获取的SSL接入点。
        "database.history.kafka.bootstrap.servers" : "kafka:9092",
        ## 需要提前在控制台创建同名topic，在本例中创建Topic：server1。
        ## 所有Table的变更数据，会记录在server1.$DATABASE.$TABLE的Topic中，如 server1.testDB.products。
        ## 因此用户需要提前在控制台中创建所有相关 Topic。
        "database.server.name": "server1",
        ## schema变化信息将记录在这个Topic中。
        ## 需要提前在控制台创建。
        "database.history.kafka.topic": "schema-changes-inventory",
        ## SSL公网方式访问配置。
        "database.history.producer.ssl.truststore.location": "kafka.client.truststore.jks",
        "database.history.producer.ssl.truststore.password": "KafkaOnsClient",
        "database.history.producer.security.protocol": "SASL_SSL",
        "database.history.producer.sasl.mechanism": "PLAIN",
        "database.history.consumer.ssl.truststore.location": "kafka.client.truststore.jks",
        "database.history.consumer.ssl.truststore.password": "KafkaOnsClient",
        "database.history.consumer.security.protocol": "SASL_SSL",
        "database.history.consumer.sasl.mechanism": "PLAIN",
        ```

3.  配置好register-mysql.json后，您需要根据配置在控制台创建相应的Topic，相关操作步骤请参见[步骤一：创建Topic](/intl.zh-CN/快速入门/步骤三：创建资源.md)。

    按照本教程中的方式安装的MySQL，您可以看到MySQL中已经提前创建好了database：inventory。其中有四张表：

    -   customers
    -   orders
    -   products
    -   products\_on\_hand
    根据以上配置，您需要使用OpenAPI创建Topic：

    -   server1
    -   server1.inventory.customers
    -   server1.inventory.orders
    -   server1.inventory.products
    -   server1.inventory.products\_on\_hand
    在register-mysql.json中，配置了将schema变化信息记录在schema-changes-testDB，因此您还需要使用OpenAPI创建Topic：schema-changes-inventory。 使用OpenAPI创建Topic，请参见[CreateTopic](/intl.zh-CN/API参考/Topic/CreateTopic.md)。

4.  执行以下命令启动MySQL Connector。

    ```
    curl -i -X POST -H "Accept:application/json" -H  "Content-Type:application/json" http://localhost:8083/connectors/ -d @register-mysql.json
    ```


## 结果验证

按照以下步骤操作确认消息队列Kafka版能否接收到MySQL的变更数据。

1.  变更MySQL Table中的数据。

2.  在控制台的**消息查询**页面，查询变更数据。


