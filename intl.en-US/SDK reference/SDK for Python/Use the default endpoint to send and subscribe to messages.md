---
keyword: [kafka, send and subscribe to messages, python, vpc, plaintext]
---

# Use the default endpoint to send and subscribe to messages

This topic describes how a Python client uses SDK for Python to connect to the default endpoint of Message Queue for Apache Kafka and send and subscribe to messages in a virtual private cloud \(VPC\).

-   Python is installed. For more information, see [Python download](https://www.python.org/downloads/).

    **Note:** Python 2.7, 3.4, 3.5, 3.6, and 3.7 are supported.

-   pip is installed. For more information, see [pip installation](https://pip.pypa.io/en/stable/installing/).

## Install the Python library

1.  Run the following command to install the Python library:

    ```
    pip install kafka-python
    ```


## Prepare configurations

1.  Create a Kafka configuration file setting.py.

    ```
    kafka_setting = {
        'bootstrap_servers': ["XXX", "XXX", "XXX"],
        'topic_name': 'XXX',
        'consumer_id': 'XXX'
    }
    ```

    |Parameter|Description|
    |---------|-----------|
    |bootstrap\_servers|The default endpoint of the Message Queue for Apache Kafka instance. You can obtain the default endpoint in the **Basic Information** section of the **Instance Details** page in the Message Queue for Apache Kafka console.|
    |topic\_name|The name of the topic. You can obtain the name of the topic on the **Topics** page in the Message Queue for Apache Kafka console.|
    |consumer\_id|The name of the consumer group. You can obtain the name of the consumer group on the **Consumer Groups** page in the Message Queue for Apache Kafka console.|


## Send messages

1.  Create a message sender aliyun\_kafka\_producer.py.

    ```
    #! /usr/bin/env python
    # encoding: utf-8
    
    import socket
    from kafka import KafkaProducer
    from kafka.errors import KafkaError
    import setting
    
    conf = setting.kafka_setting
    
    print conf
    
    
    producer = KafkaProducer(bootstrap_servers=conf['bootstrap_servers'],
                            api_version = (0,10),
                            retries=5)
    
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

1.  Create a subscription program aliyun\_kafka\_consumer.py.

    ```
    #! /usr/bin/env python
    # encoding: utf-8
    
    import socket
    from kafka import KafkaConsumer
    from kafka.errors import KafkaError
    import setting
    
    conf = setting.kafka_setting
    
    consumer = KafkaConsumer(bootstrap_servers=conf['bootstrap_servers'],
                            group_id=conf['consumer_id'],
                            api_version = (0,10,2), 
                            session_timeout_ms=25000,
                            max_poll_records=100,
                            fetch_max_bytes=1 * 1024 * 1024)
    
    print 'consumer start to consuming...'
    consumer.subscribe((conf['topic_name'], ))
    for message in consumer:
        print message.topic, message.offset, message.key, message.value, message.value, message.partition
    ```

2.  Run the following command to consume messages:

    ```
    python aliyun_kafka_consumer.py
    ```


