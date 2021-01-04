---
keyword: [kafka, 收发消息, Python, VPC, sasl\_plain]
---

# SSL接入点PLAIN机制收发消息

本文介绍如何在公网环境下使用Python SDK接入消息队列Kafka版的SSL接入点并使用PLAIN机制收发消息。

-   [安装Python](https://www.python.org/downloads/)

    **说明：** Python版本为2.7、3.4、3.5、3.6或3.7。

-   [安装pip](https://pip.pypa.io/en/stable/installing/)

## 安装Python依赖库

1.  执行以下命令安装Python依赖库。

    ```
    pip install kafka-python
    ```


## 准备配置

1.  [下载SSL根证书](https://code.aliyun.com/alikafka/aliware-kafka-demos/raw/master/kafka-filebeat-demo/vpc-ssl/ca-cert)。

2.  创建消息队列Kafka版配置文件setting.py。

    ```
    kafka_setting = {
        'sasl_plain_username': 'XXX',
        'sasl_plain_password': 'XXX',
        'bootstrap_servers': ["XXX", "XXX", "XXX"],
        'topic_name': 'XXX',
        'consumer_id': 'XXX'
    }
    ```

    |参数|描述|
    |--|--|
    |sasl\_plain\_username|用户名。    -   如果实例未开启ACL，您可以在消息队列Kafka版控制台的**实例详情**页面获取默认用户的用户名。
    -   如果实例已开启ACL，请确保要使用的SASL用户为PLAIN类型且已授权收发消息的权限。详情请参见[SASL用户授权](/cn.zh-CN/权限控制/SASL用户授权.md)。 |
    |sasl\_plain\_password|密码。    -   如果实例未开启ACL，您可以在消息队列Kafka版控制台的**实例详情**页面获取默认用户的密码。
    -   如果实例已开启ACL，请确保要使用的SASL用户为PLAIN类型且已授权收发消息的权限。详情请参见[SASL用户授权](/cn.zh-CN/权限控制/SASL用户授权.md)。 |
    |bootstrap\_servers|SSL接入点。您可在消息队列Kafka版控制台的**实例详情**页面的**基本信息**区域获取。|
    |topic\_name|Topic名称。您可在消息队列Kafka版控制台的**Topic管理**页面获取。|
    |consumer\_id|Consumer Group名称。您可在消息队列Kafka版控制台的**Consumer Group管理**页面获取。|


## 发送消息

1.  创建发送消息程序aliyun\_kafka\_producer.py。

    ```
    #!/usr/bin/env python
    # encoding: utf-8
    
    import ssl
    import socket
    from kafka import KafkaProducer
    from kafka.errors import KafkaError
    import setting
    
    conf = setting.kafka_setting
    
    print conf
    
    context = ssl.create_default_context()
    context = ssl.SSLContext(ssl.PROTOCOL_SSLv23)
    context.verify_mode = ssl.CERT_REQUIRED
    # context.check_hostname = True
    context.load_verify_locations("ca-cert")
    
    producer = KafkaProducer(bootstrap_servers=conf['bootstrap_servers'],
                            sasl_mechanism="PLAIN",
                            ssl_context=context,
                            security_protocol='SASL_SSL',
                            api_version = (0,10),
                            retries=5,
                            sasl_plain_username=conf['sasl_plain_username'],
                            sasl_plain_password=conf['sasl_plain_password'])
    
    partitions = producer.partitions_for(conf['topic_name'])
    print 'Topic下分区: %s' % partitions
    
    try:
        future = producer.send(conf['topic_name'], 'hello aliyun-kafka!')
        future.get()
        print 'send message succeed.'
    except KafkaError, e:
        print 'send message failed.'
        print e
    ```

2.  执行以下命令发送消息。

    ```
    python aliyun_kafka_producer.py
    ```


## 接收消息

1.  创建订阅消息程序aliyun\_kafka\_consumer.py。

    ```
    #!/usr/bin/env python
    # encoding: utf-8
    
    import ssl
    import socket
    from kafka import KafkaConsumer
    from kafka.errors import KafkaError
    import setting
    
    conf = setting.kafka_setting
    
    
    context = ssl.create_default_context()
    context = ssl.SSLContext(ssl.PROTOCOL_SSLv23)
    context.verify_mode = ssl.CERT_REQUIRED
    # context.check_hostname = True
    context.load_verify_locations("ca-cert")
    
    consumer = KafkaConsumer(bootstrap_servers=conf['bootstrap_servers'],
                            group_id=conf['consumer_id'],
                            sasl_mechanism="PLAIN",
                            ssl_context=context,
                            security_protocol='SASL_SSL',
                            api_version = (0,10),
                            sasl_plain_username=conf['sasl_plain_username'],
                            sasl_plain_password=conf['sasl_plain_password'])
    
    print 'consumer start to consuming...'
    consumer.subscribe((conf['topic_name'], ))
    for message in consumer:
        print message.topic, message.offset, message.key, message.value, message.value, message.partition
    ```

2.  执行以下命令消费消息。

    ```
    python aliyun_kafka_consumer.py
    ```


