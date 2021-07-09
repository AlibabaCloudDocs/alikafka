# 使用Spring Cloud框架收发消息

本文介绍如何使用Spring Cloud框架接入消息队列Kafka版并收发消息。

Spring Cloud是用于构建消息驱动的微服务应用程序的框架。详细信息，请参见[Spring Cloud Stream](https://docs.spring.io/spring-cloud-stream/docs/current/reference/html/)。

## 前提条件

-   [安装1.8或以上版本JDK](https://www.oracle.com/java/technologies/javase-downloads.html)。
-   [安装2.5或以上版本Maven](http://maven.apache.org/download.cgi#)。
-   下载[kafka-spring-stream-demo](https://github.com/AliwareMQ/aliware-kafka-demos/tree/master/kafka-spring-stream-demo)，并将其上传在准备好的Linux操作系统。
-   确保您的消息队列Kafka版实例为2.x或以上版本。具体操作，请参见[升级实例版本](/intl.zh-CN/控制台使用指南/实例/升级实例版本.md)。

## 公网环境（消息传输需鉴权与加密）

公网环境，消息采用SASL\_SSL协议进行鉴权并加密。客户端通过SSL接入点访问消息队列Kafka版。接入点的详细信息，请参见[接入点对比](/intl.zh-CN/产品简介/接入点对比.md)。

本示例将Demo包上传在/home/doc/project/aliware-kafka-demos/kafka-spring-stream-demo路径。

1.  登录Linux系统，执行以下命令，进入Demo包所在路径/home/doc/project/aliware-kafka-demos/kafka-spring-stream-demo。

    ```
    cd /home/doc/project/aliware-kafka-demos/kafka-spring-stream-demo
    ```

2.  执行以下命令，进入配置文件路径。

    ```
    cd sasl-ssl/src/main/resources/
    ```

3.  执行以下命令，编辑application.properties文件，并根据[表 1](#table_48u_or1_ast)配置实例信息。

    ```
    vi application.properties
    ```

    ```
    ##以下参数，您需配置为实际使用的实例信息。
    kafka.bootstrap-servers=47.99.XX.XX:9093,118.178.XX.XX:9093,47.110.XX.XX:9093,116.62.XX.XX:9093,101.37.XX.XX:9093
    kafka.consumer.group=test-spring
    kafka.output.topic.name=test-output
    kafka.input.topic.name=test-input
    kafka.ssl.truststore.location=/home/doc/project/aliware-kafka-demos/kafka-spring-stream-demo/sasl-ssl/src/main/resources/kafka.client.truststore.jks
    
    ### 配置Binding参数可以把消息队列Kafka版和Spring Cloud Stream的Binder绑定在一起，以下参数保持默认即可。
    spring.cloud.stream.bindings.MyOutput.destination=${kafka.output.topic.name}
    spring.cloud.stream.bindings.MyOutput.contentType=text/plain
    spring.cloud.stream.bindings.MyInput.group=${kafka.consumer.group}
    spring.cloud.stream.bindings.MyInput.destination=${kafka.input.topic.name}
    spring.cloud.stream.bindings.MyInput.contentType=text/plain
    
    ### Binder是Spring Cloud对消息中间件的封装模块，以下参数保持默认即可。
    spring.cloud.stream.kafka.binder.autoCreateTopics=false
    spring.cloud.stream.kafka.binder.brokers=${kafka.bootstrap-servers}
    spring.cloud.stream.kafka.binder.configuration.security.protocol=SASL_SSL
    spring.cloud.stream.kafka.binder.configuration.sasl.mechanism=PLAIN
    spring.cloud.stream.kafka.binder.configuration.ssl.truststore.location=${kafka.ssl.truststore.location}
    spring.cloud.stream.kafka.binder.configuration.ssl.truststore.password=KafkaOnsCl****
    ### 如果Demo中没有以下参数，请手动增加。该参数表示是否需要进行服务器主机名验证。因消息传输使用SASL身份校验，可设置为空字符串关闭服务器主机名验证。
    ### 服务器主机名验证是验证SSL证书的主机名与服务器的主机名是否匹配，默认为HTTPS。
    spring.cloud.stream.kafka.binder.configuration.ssl.endpoint.identification.algorithm=
    ```

    |参数|描述|
    |--|--|
    |kafka.bootstrap-servers|消息队列Kafka版实例接入点。您可在[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)的**实例详情**页面的**接入点信息**区域获取。|
    |kafka.consumer.group|订阅消息的Consumer Group。您可以在[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)的**Group 管理**页面创建。具体操作，请参见[步骤三：创建资源](/intl.zh-CN/快速入门/步骤三：创建资源.md)。|
    |kafka.output.topic.name|消息的Topic。控制台程序通过此Topic每隔一段时间发送消息，内容是固定的。您可以在[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)的**Topic 管理**页面创建。具体操作，请参见[步骤三：创建资源](/intl.zh-CN/快速入门/步骤三：创建资源.md)。|
    |kafka.input.topic.name|消息的Topic。您可以通过此Topic在控制台发送消息，Demo程序会消费消息，并将消息打印在日志中。|
    |ssl.truststore.location|SSL根证书[kafka.client.truststore.jks](https://github.com/AliwareMQ/aliware-kafka-demos/blob/master/kafka-spring-stream-demo/sasl-ssl/src/main/resources/kafka.client.truststore.jks)的存放路径。|

4.  执行以下命令，打开kafka\_client\_jaas.conf文件，配置实例的用户名与密码。

    ```
    vi kafka_client_jaas.conf
    ```

    **说明：**

    -   如果实例未开启ACL，您可以在消息队列Kafka版控制台的**实例详情**页面获取默认用户的用户名和密码。
    -   如果实例已开启ACL，请确保要使用的SASL用户为PLAIN类型且已授权收发消息的权限。具体信息，请参见[SASL用户授权](/intl.zh-CN/访问控制（权限管理）/SASL用户授权.md)。
    ```
    KafkaClient {
      org.apache.kafka.common.security.plain.PlainLoginModule required
      username="XXX"
      password="XXX";
    };
    ```

5.  进入/home/doc/project/aliware-kafka-demos/kafka-spring-stream-demo/sasl-ssl路径，执行以下命令，运行Demo。

    ```
    sh run_demo.sh
    ```

    程序打印如下信息，说明接收到控制台程序通过kafka.output.topic.name配置的Topic所发送的消息。

    ```
    Send: hello world !!
    Send: hello world !!
    Send: hello world !!
    Send: hello world !!
    ```

6.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)验证消息收发是否成功。

    -   查询kafka.output.topic.name配置的Topic是否接收到控制台程序发送的消息。具体操作，请参见[查询消息](/intl.zh-CN/控制台使用指南/查询消息.md)。
    -   在kafka.input.topic.name配置的Topic发送消息，查看Demo程序日志中是否会打印消息。具体操作，请参见[发送消息](/intl.zh-CN/控制台使用指南/实例/实例问题/如何快速测试消息队列Kafka版服务端是否正常？.md)。

## VPC环境（消息传输不鉴权不加密）

VPC环境，消息可以采用PLAINTEXT协议不鉴权不加密传输。客户端通过默认接入点访问消息队列Kafka版。接入点的详细信息，请参见[接入点对比](/intl.zh-CN/产品简介/接入点对比.md)。

本示例将Demo包上传在/home/doc/project/aliware-kafka-demos/kafka-spring-stream-demo路径。

1.  登录Linux系统，执行以下命令，进入Demo包所在路径/home/doc/project/aliware-kafka-demos/kafka-spring-stream-demo。

    ```
    cd /home/doc/project/aliware-kafka-demos/kafka-spring-stream-demo
    ```

2.  执行以下命令，进入配置文件路径。

    ```
    cd vpc/src/main/resources/
    ```

3.  执行以下命令，编辑application.properties文件，并根据[表 1](#table_48u_or1_ast)配置实例信息。

    ```
    vi application.properties
    ```

    ```
    ###以下参数请修改为实际使用的实例的信息
    kafka.bootstrap-servers=192.168.XX.XX:9092,192.168.XX.XX:9092,192.168.XX.XX:9092,192.168.XX.XX:9092,192.168.XX.XX:9092
    kafka.consumer.group=test-spring
    kafka.output.topic.name=test-output
    kafka.input.topic.name=test-input
    ```

4.  进入/home/doc/project/aliware-kafka-demos/kafka-spring-stream-demo/vpc路径，执行以下命令，运行Demo。

    ```
    sh run_demo.sh
    ```

    程序打印如下信息。

    ```
    Send: hello world !!
    Send: hello world !!
    Send: hello world !!
    Send: hello world !!
    ```

5.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)验证消息收发是否成功。

    -   查询kafka.output.topic.name配置的Topic是否接收到控制台程序发送的消息。具体操作，请参见[查询消息](/intl.zh-CN/控制台使用指南/查询消息.md)。
    -   在kafka.input.topic.name配置的Topic发送消息，查看Demo程序日志中是否会打印消息。具体操作，请参见[发送消息](/intl.zh-CN/控制台使用指南/实例/实例问题/如何快速测试消息队列Kafka版服务端是否正常？.md)。

