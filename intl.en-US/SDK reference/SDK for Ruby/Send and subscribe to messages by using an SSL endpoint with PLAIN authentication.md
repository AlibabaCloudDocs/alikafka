# Send and subscribe to messages by using an SSL endpoint with PLAIN authentication

This topic describes how a Ruby client uses SDK for Ruby to connect to the Secure Sockets Layer \(SSL\) endpoint of a Message Queue for Apache Kafka instance over the Internet and uses the PLAIN mechanism to send and subscribe to messages.

Ruby is installed. For more information, see [Install Ruby](http://www.ruby-lang.org/zh_cn/downloads/).

## Install the Ruby library

1.  Run the following command to install the Ruby library:

    ```
    
            gem install ruby-kafka -v 0.6.8 
          
    ```


## Prepare configurations

1.  [Download an SSL root certificate](https://code.aliyun.com/alikafka/aliware-kafka-demos/raw/master/kafka-ruby-demo/vpc-ssl/cert.pem).


## Send messages

1.  Create a message sender named producer.rb.

    ```
    # frozen_string_literal: true
    
    $LOAD_PATH.unshift(File.expand_path(".. /../lib", __FILE__))
    
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

    |Parameter|Description|
    |---------|-----------|
    |brokers|The SSL endpoint of the Message Queue for Apache Kafka instance. You can obtain the SSL endpoint in the **Basic Information** section of the **Instance Details** page in the Message Queue for Apache Kafka console.|
    |topic|The name of the topic. You can obtain the name of the topic on the **Topics** page in the Message Queue for Apache Kafka console.|
    |username|The name of the Simple Authentication and Security Layer \(SASL\) user.    -   If access control list \(ACL\) is disabled for the Message Queue for Apache Kafka instance, you can obtain the default username on the **Instance Details** page in the Message Queue for Apache Kafka console.
    -   If ACL is enabled for the Message Queue for Apache Kafka instance, make sure that the SASL user to be used is of the PLAIN type and that the user is authorized to send and subscribe to messages. For more information, see [Authorize SASL users](/intl.en-US/Access control/Authorize SASL users.md). |
    |password|The password of the SASL user.    -   If ACL is disabled for the Message Queue for Apache Kafka instance, you can obtain the password of the default user on the **Instance Details** page in the Message Queue for Apache Kafka console.
    -   If ACL is enabled for the Message Queue for Apache Kafka instance, make sure that the SASL user to be used is of the PLAIN type and that the user is authorized to send and subscribe to messages. For more information, see [Authorize SASL users](/intl.en-US/Access control/Authorize SASL users.md). |

2.  Run the following command to send messages:

    ```
    ruby producer.rb
    ```


## Subscribe to messages

1.  Create a subscription program named consumer.rb.

    ```
    # frozen_string_literal: true
    
    $LOAD_PATH.unshift(File.expand_path(".. /../lib", __FILE__))
    
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

    |Parameter|Description|
    |---------|-----------|
    |brokers|The SSL endpoint of the Message Queue for Apache Kafka instance. You can obtain the SSL endpoint in the **Basic Information** section of the **Instance Details** page in the Message Queue for Apache Kafka console.|
    |topic|The name of the topic. You can obtain the name of the topic on the **Topics** page in the Message Queue for Apache Kafka console.|
    |username|The name of the SASL user.    -   If ACL is disabled for the Message Queue for Apache Kafka instance, you can obtain the default username on the **Instance Details** page in the Message Queue for Apache Kafka console.
    -   If ACL is enabled for the Message Queue for Apache Kafka instance, make sure that the SASL user to be used is of the PLAIN type and that the user is authorized to send and subscribe to messages. For more information, see [Authorize SASL users](/intl.en-US/Access control/Authorize SASL users.md). |
    |password|The password of the SASL user.    -   If ACL is disabled for the Message Queue for Apache Kafka instance, you can obtain the password of the default user on the **Instance Details** page in the Message Queue for Apache Kafka console.
    -   If ACL is enabled for the Message Queue for Apache Kafka instance, make sure that the SASL user to be used is of the PLAIN type and that the user is authorized to send and subscribe to messages. For more information, see [Authorize SASL users](/intl.en-US/Access control/Authorize SASL users.md). |
    |consumerGroup|The name of the consumer group. You can obtain the name of the consumer group on the **Consumer Groups** page in the Message Queue for Apache Kafka console.|

2.  Run the following command to consume messages:

    ```
    ruby consumer.rb
    ```


