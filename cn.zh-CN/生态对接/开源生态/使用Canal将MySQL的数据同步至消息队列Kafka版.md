# 使用Canal将MySQL的数据同步至消息队列Kafka版

本教程介绍如何使用Canal将MySQL的数据同步至消息队列Kafka版。

Canal的主要用途是基于MySQL数据库增量日志解析，提供增量数据订阅和消费。Canal伪装自己为MySQL Slave，向MySQL Master发送dump请求。MySQL Master收到dump请求，开始推送Binary log给Canal，Canal解析Binary log来同步数据。Canal与消息队列Kafka版建立对接，您可以把MySQL更新的数据写入到消息队列Kafka版中来分析。其详细的工作原理，请参见[Canal官网](https://github.com/alibaba/canal)。

![背景介绍](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9119655261/p290023.png)

## 前提条件

在开始本教程前，请确保您已完成以下操作：

-   安装MySQL，并进行相关初始化与设置。具体操作，请参见 [Canal QuickStart](https://github.com/alibaba/canal/wiki/QuickStart)。
-   在[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)创建实例以及Topic资源。具体操作，请参见[步骤三：创建资源](/cn.zh-CN/快速入门/步骤三：创建资源.md)。

## 操作步骤

1.  下载[Canal压缩包](https://github.com/alibaba/canal)，本教程以1.1.5版本为例。

2.  执行以下命令，创建目录文件夹。本教程以/home/doc/tools/canal.deployer-1.1.5路径为例。

    ```
    mkdir -p /home/doc/tools/canal.deployer-1.1.5
    ```

3.  将[Canal压缩包](https://github.com/alibaba/canal)复制到/home/doc/tools/canal.deployer-1.1.5路径并解压。

    ```
    tar -zxvf canal.deployer-1.1.5-SNAPSHOT.tar.gz -C /home/doc/tools/canal.deployer-1.1.5
    ```

4.  在/home/doc/tools/canal.deployer-1.1.5路径，执行以下命令，编辑instance.properties文件。

    ```
    vi conf/example/instance.properties
    ```

    根据[表 1](#table_r3w_zhj_i8v)配置参数。

    ```
    #  根据实际情况修改为您的数据库信息。
    #################################################
    ...
    # 数据库地址。
    canal.instance.master.address=192.168.XX.XX:3306
    # username/password为数据库的用户名和密码。
    ...
    canal.instance.dbUsername=****
    canal.instance.dbPassword=****
    ...
    # mq config
    # 您在消息队列Kafka版控制台创建的Topic。
    canal.mq.topic=mysql_test
    # 针对数据库名或者表名发送动态Topic。
    #canal.mq.dynamicTopic=mytest,.*,mytest.user,mytest\\..*,.*\\..*
    # 数据同步到消息队列Kafka版Topic的指定分区。
    canal.mq.partition=0
    # 以下两个参数配置与canal.mq.partition互斥。配置以下两个参数可以使数据发送至消息队列Kafka版Topic的不同分区。
    #canal.mq.partitionsNum=3
    #库名.表名: 唯一主键，多个表之间用逗号分隔。
    #canal.mq.partitionHash=mytest.person:id,mytest.role:id
    #################################################
    ```

    |参数|描述|
    |--|--|
    |canal.instance.master.address|MySQL数据库的连接地址。|
    |canal.instance.dbUsername|MySQL数据库的用户名。|
    |canal.instance.dbPassword|MySQL数据库的用户名密码。|
    |canal.mq.topic|消息队列Kafka版实例的Topic。您可以在[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)的**Topic 管理**页面创建。具体操作，请参见[步骤三：创建资源](/cn.zh-CN/快速入门/步骤三：创建资源.md)。|
    |canal.mq.dynamicTopic|动态Topic规则表达式。设置Topic匹配规则表达式，可以将不同的数据库数据同步至不同的Topic。具体设置方法，请参见[参数说明](https://github.com/alibaba/canal/wiki/Canal-Kafka-RocketMQ-QuickStart#mq%E7%9B%B8%E5%85%B3%E5%8F%82%E6%95%B0%E8%AF%B4%E6%98%8E)。|
    |canal.mq.partition|数据库数据同步到消息队列Kafka版Topic的指定分区。|
    |canal.mq.partitionsNum|Topic的分区数量。该参数与canal.mq.partitionHash一起使用，可以将数据同步至消息队列Kafka版Topic不同的分区。|
    |canal.mq.partitionHash|分区的规则表达式。具体设置方法，请参见[参数说明](https://github.com/alibaba/canal/wiki/Canal-Kafka-RocketMQ-QuickStart#mq%E7%9B%B8%E5%85%B3%E5%8F%82%E6%95%B0%E8%AF%B4%E6%98%8E)。|

5.  执行以下命令，编辑canal.properties文件。

    ```
    vi conf/canal.properties
    ```

    根据[表 2](#table_52f_e3z_xlv)说明配置参数。

    -   公网环境，消息采用SASL\_SSL协议进行鉴权并加密，通过SSL接入点访问消息队列Kafka版。接入点的详细信息，请参见[接入点对比](/cn.zh-CN/产品简介/接入点对比.md)。

        ```
        # ...
        # 您需设置为kafka。
        canal.serverMode = kafka
        # ...
        # kafka配置。
        #在消息队列Kafka版实例详情页面获取的SSL接入点。
        kafka.bootstrap.servers = 192.168.XX.XX:9093,192.168.XX.XX:9093,192.168.XX.XX:9093
        # 以下参数请您可以按照实际情况调整，也可以保持默认设置。
        kafka.acks = all
        kafka.compression.type = none
        kafka.batch.size = 16384
        kafka.linger.ms = 1
        kafka.max.request.size = 1048576
        kafka.buffer.memory = 33554432
        kafka.max.in.flight.requests.per.connection = 1
        kafka.retries = 0
        
        # 公网环境，通过SASL_SSL鉴权并加密，您需配置网络协议与身份校验机制。
        kafka.ssl.truststore.location= ../conf/kafka_client_truststore_jks
        kafka.ssl.truststore.password= KafkaOnsCl****
        kafka.security.protocol= SASL_SSL
        kafka.sasl.mechanism = PLAIN
        kafka.ssl.endpoint.identification.algorithm =
        ```

        |参数|描述|
        |--|--|
        |canal.serverMode|您需设置为kafka。|
        |kafka.bootstrap.servers|消息队列Kafka版实例接入点。您可在[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)的**实例详情**页面的**接入点信息**区域获取。|
        |kafka.ssl.truststore.location|SSL根证书[kafka.client.truststore.jks](https://github.com/AliwareMQ/aliware-kafka-demos/blob/master/kafka-spring-stream-demo/sasl-ssl/src/main/resources/kafka.client.truststore.jks)的存放路径。**说明：** 公网环境下，消息必须进行鉴权与加密，才能确保传输的安全。即需通过SSL接入点采用SASL\_SSL协议进行传输。具体信息，请参见[接入点对比](/cn.zh-CN/产品简介/接入点对比.md)。 |
        |kafka.acks|消息队列Kafka版接收到数据之后给客户端发出的确认信号。取值说明如下：        -   0：表示客户端不需要等待任何确认收到的信息。
        -   1：表示等待Leader成功写入而不等待所有备份是否成功写入。
        -   all：表示等待Leader成功写入并且所有备份都成功写入。 |
        |kafka.compression.type|压缩数据的压缩算法，默认是无压缩。取值如下：        -   none。
        -   gzip。
        -   snappy。 |
        |kafka.batch.size|批量处理消息字节数。|
        |kafka.linger.ms|批量处理消息的延迟时长。单位：ms。|
        |kafka.max.request.size|客户端每次请求的最大字节数。|
        |kafka.buffer.memory|缓存数据的内存大小。|
        |kafka.max.in.flight.requests.per.connection|限制客户端在单个连接上能够发送的未响应请求的个数。设置此值是1表示Broker在响应请求之前客户端不能再向同一个Broker发送请求。|
        |kafka.retries|消息发送失败时，是否重复发送。设置为0，表示不会重复发送；设置大于0的值，客户端重新发送数据。|
        |kafka.ssl.truststore.password|SSL根证书的密码。|
        |kafka.security.protocol|采用SASL\_SSL协议进行鉴权并加密，即设置为SASL\_SSL。|
        |kafka.sasl.mechanism|SASL身份认证的机制。SSL接入点采用PLAIN机制验证身份。|

        公网环境，需通过SASL进行身份校验，需要在bin/startup.sh配置环境变量，并编辑kafka\_client\_producer\_jaas.conf文件，配置消息队列Kafka版实例的用户名与密码。

        1.  执行`vi bin/startup.sh`命令，编辑startup.sh文件，配置环境变量。

            ```
            JAVA_OPTS=" $JAVA_OPTS -Djava.awt.headless=true -Djava.net.preferIPv4Stack=true -Dfile.encoding=UTF-8 -Djava.security.auth.login.config=/home/doc/tools/canal.deployer-1.1.5/conf/kafka_client_jaas.conf"
            ```

        2.  执行`vi conf/kafka_client_producer_jaas.conf`命令，编辑kafka\_client\_producer\_jaas.conf文件，配置实例用户名与密码信息。

            **说明：**

            -   如果实例未开启ACL，您可以在消息队列Kafka版控制台的**实例详情**页面获取默认用户的用户名和密码。
            -   如果实例已开启ACL，请确保要使用的SASL用户为PLAIN类型且已授权收发消息的权限。具体信息，请参见[SASL用户授权](/cn.zh-CN/访问控制（权限管理）/SASL用户授权.md)。
            ```
            KafkaClient {  org.apache.kafka.common.security.plain.PlainLoginModule required
                           username="实例的用户名"  
                           password="实例的用户名密码";
            };
            ```

    -   VPC环境，消息采用PLAINTEXT协议不鉴权不加密传输，通过默认接入点访问消息队列Kafka版，仅需配置canal.serverMode与kafka.bootstrap.servers参数。接入点的详细信息，请参见[接入点对比](/cn.zh-CN/产品简介/接入点对比.md)。

        ```
        # ...
        # 您需设置为kafka。
        canal.serverMode = kafka
        # ...
        # kafka配置。
        # 在消息队列Kafka版实例详情页面获取的默认接入点。
        kafka.bootstrap.servers = 192.168.XX.XX:9092,192.168.XX.XX:9092,192.168.XX.XX:9092
        # 以下参数请您可以按照实际情况调整，也可以保持默认设置。
        kafka.acks = all
        kafka.compression.type = none
        kafka.batch.size = 16384
        kafka.linger.ms = 1
        kafka.max.request.size = 1048576
        kafka.buffer.memory = 33554432
        kafka.max.in.flight.requests.per.connection = 1
        kafka.retries = 0
        ```

6.  在/home/doc/tools/canal.deployer-1.1.5路径，执行以下命令，启动Canal。

    ```
    sh bin/startup.sh
    ```

    -   查看/home/doc/tools/canal.deployer-1.1.5/logs/canal/canal.log日志文件，确认Canal与消息队列Kafka版连接成功，Canal正在运行。

        ```
        2013-02-05 22:45:27.967 [main] INFO  com.alibaba.otter.canal.deployer.CanalLauncher - ## start the canal server.
        2013-02-05 22:45:28.113 [main] INFO  com.alibaba.otter.canal.deployer.CanalController - ## start the canal server[10.1.XX.XX:11111]
        2013-02-05 22:45:28.210 [main] INFO  com.alibaba.otter.canal.deployer.CanalLauncher - ## the canal server is running now ......
        ```

    -   查看/home/doc/tools/canal.deployer-1.1.5/logs/example/example.log日志文件，确认Canal Instance已启动。

        ```
        2013-02-05 22:50:45.636 [main] INFO  c.a.o.c.i.spring.support.PropertyPlaceholderConfigurer - Loading properties file from class path resource [canal.properties]
        2013-02-05 22:50:45.641 [main] INFO  c.a.o.c.i.spring.support.PropertyPlaceholderConfigurer - Loading properties file from class path resource [example/instance.properties]
        2013-02-05 22:50:45.803 [main] INFO  c.a.otter.canal.instance.spring.CanalInstanceWithSpring - start CannalInstance for 1-example 
        2013-02-05 22:50:45.810 [main] INFO  c.a.otter.canal.instance.spring.CanalInstanceWithSpring - start successful....
        ```


## 测试验证

启动Canal之后，进行数据同步验证。

1.  在MySQL数据库mysql中，新建数据表T\_Student。数据表数据示例如下：

    ```
    mysql> select * from T_Student;
    +--------+---------+------+------+
    | stuNum | stuName | age  | sex  |
    +--------+---------+------+------+
    |      1 | 小王    |   18 | girl |
    |      2 | 小张    |   17 | boy  |
    +--------+---------+------+------+
    2 rows in set (0.00 sec)
    ```

    查看/home/doc/tools/canal.deployer-1.1.5/logs/example/meta.log日志文件，数据库的每次增删改操作，都会在meta.log中生成一条记录，查看该日志可以确认Canal是否有采集到数据。

    ```
    tail -f example/meta.log
    2020-07-29 09:21:05.110 - clientId:1001 cursor:[log.000001,29723,1591190230000,1,] address[/192.168.XX.XX:3306]
    2020-07-29 09:23:46.109 - clientId:1001 cursor:[log.000001,30047,1595985825000,1,] address[localhost/192.168.XX.XX:3306]
    2020-07-29 09:24:50.547 - clientId:1001 cursor:[log.000001,30047,1595985825000,1,] address[/192.168.XX.XX:3306]
    2020-07-29 09:26:45.547 - clientId:1001 cursor:[log.000001,30143,1595986005000,1,] address[localhost/192.168.XX.XX:3306]
    2020-07-29 09:30:04.546 - clientId:1001 cursor:[log.000001,30467,1595986204000,1,] address[localhost/192.168.XX.XX:3306]
    2020-07-29 09:30:16.546 - clientId:1001 cursor:[log.000001,30734,1595986215000,1,] address[localhost/192.168.XX.XX:3306]
    2020-07-29 09:30:36.547 - clientId:1001 cursor:[log.000001,31001,1595986236000,1,] address[localhost/192.168.XX.XX:3306]
    ```

2.  登录[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)，查询消息，确认MySQL的数据被同步在消息队列Kafka版。控制台查询消息的具体操作，请参见[查询消息](/cn.zh-CN/控制台使用指南/查询消息.md)。

    ![查询消息](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/5727655261/p290416.png)

3.  数据同步完毕，执行以下命令，关闭Canal。

    ```
    sh bin/stop.sh
    ```


