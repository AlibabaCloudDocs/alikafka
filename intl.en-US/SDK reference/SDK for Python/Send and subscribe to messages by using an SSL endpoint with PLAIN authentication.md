---
keyword: [kafka, send and subscribe to messages, python, vpc, sasl\_plain]
---

# Send and subscribe to messages by using an SSL endpoint with PLAIN authentication

This topic describes how a Python client uses SDK for Python to connect to the Secure Sockets Layer \(SSL\) endpoint of a Message Queue for Apache Kafka instance over the Internet and uses the PLAIN mechanism to send and subscribe to messages.

-   [Install Python](https://www.python.org/downloads/)

    **Note:** Python 2.7, 3.4, 3.5, 3.6, and 3.7 are supported.

-   [Install pip](https://pip.pypa.io/en/stable/installing/)

## Install the Python library

1.  Run the following command to install the Python library:

    ```
    
            pip install kafka-python 
          
    ```


## Prepare configurations

1.  [Download an SSL root certificate](https://code.aliyun.com/alikafka/aliware-kafka-demos/raw/master/kafka-filebeat-demo/vpc-ssl/ca-cert).

2.  Create a Message Queue for Apache Kafka configuration file named setting.py.

    ```
    kafka_setting = {
        'sasl_plain_username': 'XXX',
        'sasl_plain_password': 'XXX',
        'bootstrap_servers': ["XXX", "XXX", "XXX"],
        'topic_name': 'XXX',
        'consumer_id': 'XXX'
    }
    ```

    |Parameter|Description|
    |---------|-----------|
    |sasl\_plain\_username|The name of the Simple Authentication and Security Layer \(SASL\) user.    -   If access control list \(ACL\) is disabled for the Message Queue for Apache Kafka instance, you can obtain the default username on the **Instance Details** page in the Message Queue for Apache Kafka console.
    -   If ACL is enabled for the Message Queue for Apache Kafka instance, make sure that the SASL user to be used is of the PLAIN type and that the user is authorized to send and subscribe to messages. For more information, see [Authorize SASL users](/intl.en-US/Access control/Authorize SASL users.md). |
    |sasl\_plain\_password|The password of the SASL user.    -   If ACL is disabled for the Message Queue for Apache Kafka instance, you can obtain the password of the default user on the **Instance Details** page in the Message Queue for Apache Kafka console.
    -   If ACL is enabled for the Message Queue for Apache Kafka instance, make sure that the SASL user to be used is of the PLAIN type and that the user is authorized to send and subscribe to messages. For more information, see [Authorize SASL users](/intl.en-US/Access control/Authorize SASL users.md). |
    |bootstrap\_servers|The SSL endpoint of the Message Queue for Apache Kafka instance. You can obtain the SSL endpoint in the **Basic Information** section of the **Instance Details** page in the Message Queue for Apache Kafka console.|
    |topic\_name|The name of the topic. You can obtain the name of the topic on the **Topics** page in the Message Queue for Apache Kafka console.|
    |consumer\_id|The name of the consumer group. You can obtain the name of the consumer group on the **Consumer Groups** page in the Message Queue for Apache Kafka console.|


## Send messages

1.  Create a message sender named aliyun\_kafka\_producer.py.

    ```
    #! /usr/bin/env python
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
    print 'partition in topic: %s' % partitions
    
    try:
        future = producer.send(conf['topic_name'], 'hello aliyun-kafka!')
        future.get()
        print 'send message succeed.'
    except KafkaError, e:
        print 'send message failed.'
        print e
    ```

2.  Run the following command to send messages:

    ```
    python aliyun_kafka_producer.py
    ```


## Subscribe to messages

1.  Create a subscription program named aliyun\_kafka\_consumer.py.

    ```
    #! /usr/bin/env python
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

2.  Run the following command to consume messages:

    ```
    python aliyun_kafka_consumer.py
    ```


