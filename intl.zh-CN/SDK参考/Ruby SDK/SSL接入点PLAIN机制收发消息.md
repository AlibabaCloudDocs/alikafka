# SSL接入点PLAIN机制收发消息

本文介绍如何在公网环境下使用Ruby SDK接入消息队列Kafka版的SSL接入点并使用PLAIN机制收发消息。

您已安装Ruby。更多信息，请参见[安装Ruby](http://www.ruby-lang.org/zh_cn/downloads/)。

## 安装Ruby依赖库

1.  执行以下命令安装Ruby依赖库。

    ```
    gem install ruby-kafka -v 0.6.8
    ```


## 准备配置

1.  [下载SSL根证书](https://code.aliyun.com/alikafka/aliware-kafka-demos/raw/master/kafka-ruby-demo/vpc-ssl/cert.pem)。


## 发送消息

1.  创建发送消息程序producer.rb。

    ```
    # frozen_string_literal: true
    
    $LOAD_PATH.unshift(File.expand_path("../../lib", __FILE__))
    
    require "kafka"
    
    logger = Logger.new($stdout)
    #logger.level = Logger::DEBUG
    logger.level = Logger::INFO
    
    brokers = "xxx:xx,xxx:xx"
    topic = "xxx"
    username = "xxx"
    password = "xxx"
    
    kafka = Kafka.new(
        seed_brokers: brokers,
        client_id: "sasl-producer",
        logger: logger,
        # put "./cert.pem" to anywhere this can read
        ssl_ca_cert: File.read('./cert.pem'),
        sasl_plain_username: username,
        sasl_plain_password: password,
        )
    
    producer = kafka.producer
    
    begin
        $stdin.each_with_index do |line, index|
    
        producer.produce(line, topic: topic)
    
        producer.deliver_messages
    end
    
    ensure
    
        producer.deliver_messages
    
        producer.shutdown
    end
    ```

    |参数|描述|
    |--|--|
    |brokers|SSL接入点。您可在消息队列Kafka版控制台的**实例详情**页面的**基本信息**区域获取。|
    |topic|Topic名称。您可在消息队列Kafka版控制台的**Topic管理**页面获取。|
    |username|用户名。    -   如果实例未开启ACL，您可以在消息队列Kafka版控制台的**实例详情**页面获取默认用户的用户名。
    -   如果实例已开启ACL，请确保要使用的SASL用户为PLAIN类型且已授权收发消息的权限。详情请参见[SASL用户授权](/intl.zh-CN/权限控制/SASL用户授权.md)。 |
    |password|密码。    -   如果实例未开启ACL，您可以在消息队列Kafka版控制台的**实例详情**页面获取默认用户的密码。
    -   如果实例已开启ACL，请确保要使用的SASL用户为PLAIN类型且已授权收发消息的权限。详情请参见[SASL用户授权](/intl.zh-CN/权限控制/SASL用户授权.md)。 |

2.  执行以下命令发送消息。

    ```
    ruby producer.rb
    ```


## 订阅消息

1.  创建订阅消息程序consumer.rb。

    ```
    # frozen_string_literal: true
    
    $LOAD_PATH.unshift(File.expand_path("../../lib", __FILE__))
    
    require "kafka"
    
    logger = Logger.new(STDOUT)
    #logger.level = Logger::DEBUG
    logger.level = Logger::INFO
    
    brokers = "xxx:xx,xxx:xx"
    topic = "xxx"
    username = "xxx"
    password = "xxx"
    consumerGroup = "xxx"
    
    kafka = Kafka.new(
            seed_brokers: brokers,
            client_id: "sasl-consumer",
            socket_timeout: 20,
            logger: logger,
            # put "./cert.pem" to anywhere this can read
            ssl_ca_cert: File.read('./cert.pem'),
            sasl_plain_username: username,
            sasl_plain_password: password,
            )
    
    consumer = kafka.consumer(group_id: consumerGroup)
    consumer.subscribe(topic, start_from_beginning: false)
    
    trap("TERM") { consumer.stop }
    trap("INT") { consumer.stop }
    
    begin
        consumer.each_message(max_bytes: 64 * 1024) do |message|
        logger.info("Get message: #{message.value}")
        end
    rescue Kafka::ProcessingError => e
        warn "Got error: #{e.cause}"
        consumer.pause(e.topic, e.partition, timeout: 20)
    
        retry
    end
    ```

    |参数|描述|
    |--|--|
    |brokers|SSL接入点。您可在消息队列Kafka版控制台的**实例详情**页面的**基本信息**区域获取。|
    |topic|Topic名称。您可在消息队列Kafka版控制台的**Topic管理**页面获取。|
    |username|用户名。    -   如果实例未开启ACL，您可以在消息队列Kafka版控制台的**实例详情**页面获取默认用户的用户名。
    -   如果实例已开启ACL，请确保要使用的SASL用户为PLAIN类型且已授权收发消息的权限。详情请参见[SASL用户授权](/intl.zh-CN/权限控制/SASL用户授权.md)。 |
    |password|密码。    -   如果实例未开启ACL，您可以在消息队列Kafka版控制台的**实例详情**页面获取默认用户的密码。
    -   如果实例已开启ACL，请确保要使用的SASL用户为PLAIN类型且已授权收发消息的权限。详情请参见[SASL用户授权](/intl.zh-CN/权限控制/SASL用户授权.md)。 |
    |consumerGroup|Consumer Group名称。您可在消息队列Kafka版控制台的**Consumer Group管理**页面获取。|

2.  执行以下命令消费消息。

    ```
    ruby consumer.rb
    ```


