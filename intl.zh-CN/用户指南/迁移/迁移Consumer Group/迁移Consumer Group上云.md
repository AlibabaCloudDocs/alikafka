---
keyword: [Consumer Group, metadata, 迁移, kafka]
---

# 迁移Consumer Group上云

本教程介绍如何使用消息队列Kafka版提供的迁移工具将自建Kafka集群的Consumer Group迁移到消息队列Kafka版实例。

您已完成以下操作：

-   [下载JDK 8](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)
-   [下载迁移工具JAR文件](https://aliware-images.oss-cn-hangzhou.aliyuncs.com/Kafka/migration%20tool/7.30%20Migration%20Tool/kafka-migration.jar)

**说明：**

-   迁移不会删除源Kafka的Consumer Group，只是在目标消息队列Kafka版实例创建相同配置的Consumer Group。
-   迁移内容仅为Consumer Group配置，不包含消费的Topic及位点信息。

1.  打开命令行工具。

2.  使用cd将路径切换到迁移工具所在目录。

3.  创建配置文件kafka.properties。

    kafka.properties用于构造Kafka Consumer，从自建Kafka集群获取消费者位点信息。配置文件内容如下：

    ```
    ## 接入点。
    bootstrap.servers=localhost:9092
    
    ## Consumer Group，注意该Consumer Group不能有消费者位点信息，以保证能从第一个消息开始消费。
    group.id=XXX
    
    ## 如果无安全配置，可以不配置以下内容。
    
    ## SASL鉴权方式。
    #sasl.mechanism=PLAIN
    
    ## 接入协议。
    #security.protocol=SASL_SSL
    
    ## SSL根证书的路径。
    #ssl.truststore.location=/Users/***/Documents/code/aliware-kafka-demos/main/resources/kafka.client.truststore.jks
    
    ## SSL密码。
    #ssl.truststore.password=***
    
    ## SASL路径。
    #java.security.auth.login.config=/Users/***/kafka-java-demo/vpc-ssl/src/main/resources/kafka_client_jaas.conf
    ```

4.  执行以下命令确认要迁移的Consumer Group。

    `java -jar kafka-migration.jar ConsumerGroupMigrationFromTopic --propertiesPath /usr/local/kafka_2.12-2.4.0/config/kafka.properties --destAk <yourAccessKeyId> --destSk <yourAccessKeySecret> --destRegionId <yourRegionId> --destInstanceId <yourInstanceId>`

    |参数|描述|
    |--|--|
    |propertiesPath|配置文件kafka.properties的文件路径|
    |destAk|目标消息队列Kafka版实例所属阿里云账号的AccessKey ID|
    |destSk|目标消息队列Kafka版实例所属阿里云账号的AccessKey Secret|
    |destRegionId|目标消息队列Kafka版实例的地域ID|
    |destInstanceId|目标消息队列Kafka版实例的ID|

    待确认的返回结果示例如下：

    ```
    15:29:45 INFO - Will create consumer groups:[XXX, test-consumer-group]
    ```

5.  执行以下命令提交要迁移的Consumer Group。

    `java -jar kafka-migration.jar ConsumerGroupMigrationFromTopic --propertiesPath /usr/local/kafka_2.12-2.4.0/config/kafka.properties --destAk LTAI4FwQ5aK1mFYCspJ1h*** --destSk wvDxjjRQ1tHPiL0oj7Y2Z7WDNkS*** --destRegionId cn-hangzhou --destInstanceId alikafka_pre-cn-v0h1cng00*** --commit`

    |参数|描述|
    |--|--|
    |commit|提交迁移|

    提交迁移的返回结果示例如下：

    ```
    15:35:51 INFO - cmd=ConsumerGroupMigrationFromTopic, request=null, response={"code":200,"requestId":"C9797848-FD4C-411F-966D-0D4AB5D12F55","success":true,"message":"operation success"}
    15:35:51 INFO - ConsumerCreate success, consumer group=XXX
    15:35:57 INFO - cmd=ConsumerGroupMigrationFromTopic, request=null, response={"code":200,"requestId":"3BCFDBF2-3CD9-4D48-92C3-385C8DBB9709","success":true,"message":"operation success"}
    15:35:57 INFO - ConsumerCreate success, consumer group=test-consumer-group
    ```

6.  确认Consumer Group迁移是否成功。

    1.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/)。

    2.  在顶部菜单栏，选择目标实例所在地域。

    3.  在左侧导航栏，单击**Consumer Group管理**。

    4.  在**Consumer Group管理**页面，选择目标实例。

        **Consumer Group**列表显示成功迁移的Consumer Group。


