# Send and subscribe to messages by using an SSL endpoint with PLAIN authentication

This topic describes how a PHP client uses SDK for PHP to connect to the Secure Sockets Layer \(SSL\) endpoint of a Message Queue for Apache Kafka instance over the Internet and uses the PLAIN mechanism to send and subscribe to messages.

-   [Install GCC](https://gcc.gnu.org/install/)
-   [Install PHP](https://www.php.net/downloads)
-   [Install PECL](https://www.php.net/manual/en/install.pecl.downloads.php)

## Install the C++ library

1.  Run the following command to switch to the yum source configuration Directory: /etc/yum.repos.d/.

    ```
    
            cd /etc/yum.repos.d/ 
          
    ```

2.  Create the yum source configuration file confluent.repo.

    ```
    
            [Confluent.dist] name=Confluent repository (dist) baseurl=https://packages.confluent.io/rpm/5.1/7 gpgcheck=1 gpgkey=https://packages.confluent.io/rpm/5.1/archive.key enabled=1 [Confluent] name=Confluent repository baseurl=https://packages.confluent.io/rpm/5.1 gpgcheck=1 gpgkey=https://packages.confluent.io/rpm/5.1/archive.key enabled=1 
          
    ```

3.  Run the following command to install the C++ library:

    ```
    
            yum install librdkafka-devel 
          
    ```


## Install the PHP library

1.  Run the following command to install the PHP library:

    ```
    
            pecl install rdkafka 
          
    ```

2.  In PHP's initialization file php.iniAdd the following line statement to enable Kafka extension.

    ```
    
            extension=rdkafka.so 
          
    ```


## Prepare configurations

1.  [Download an SSL root certificate](https://code.aliyun.com/alikafka/aliware-kafka-demos/raw/master/kafka-php-demo/vpc-ssl/ca-cert.pem).

2.  Create a Message Queue for Apache Kafka configuration file.

    ```
    <? php
    
    return [
        'sasl_plain_username' => 'xxx',
        'sasl_plain_password' => 'xxx',
        'bootstrap_servers' => "xxx:xx,xxx:xx",
        'topic_name' => 'xxx',
        'consumer_id' => 'xxx'
    ];
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

1.  Create a message sender named kafka-producer.php.

    ```
    <? php
    
    $setting = require __DIR__ . '/setting.php';
    
    $conf = new RdKafka\Conf();
    $conf->set('sasl.mechanisms', 'PLAIN');
    $conf->set('api.version.request', 'true');
    $conf->set('sasl.username', $setting['sasl_plain_username']);
    $conf->set('sasl.password', $setting['sasl_plain_password']);
    $conf->set('security.protocol', 'SASL_SSL');
    $conf->set('ssl.ca.location', __DIR__ . '/ca-cert.pem');
    $conf->set('message.send.max.retries', 5);
    $rk = new RdKafka\Producer($conf);
    # if want to debug, set log level to LOG_DEBUG
    $rk->setLogLevel(LOG_INFO);
    $rk->addBrokers($setting['bootstrap_servers']);
    $topic = $rk->newTopic($setting['topic_name']);
    $a = $topic->produce(RD_KAFKA_PARTITION_UA, 0, "Message hello kafka");
    $rk->poll(0);
    while ($rk->getOutQLen() > 0) {
        $rk->poll(50);
    }
    echo "send succ" . PHP_EOL;
    ```

2.  Run the following command to send messages:

    ```
    php kafka-producer.php
    ```


## Subscribe to messages

1.  Create a subscription program named kafka-consumer.php.

    ```
    <? php
    $setting = require __DIR__ . '/setting.php';
    $conf = new RdKafka\Conf();
    $conf->set('sasl.mechanisms', 'PLAIN');
    $conf->set('api.version.request', 'true');
    $conf->set('sasl.username', $setting['sasl_plain_username']);
    $conf->set('sasl.password', $setting['sasl_plain_password']);
    $conf->set('security.protocol', 'SASL_SSL');
    $conf->set('ssl.ca.location', __DIR__ . '/ca-cert.pem');
    
    $conf->set('group.id', $setting['consumer_id']);
    
    $conf->set('metadata.broker.list', $setting['bootstrap_servers']);
    
    $topicConf = new RdKafka\TopicConf();
    
    $conf->setDefaultTopicConf($topicConf);
    
    $consumer = new RdKafka\KafkaConsumer($conf);
    
    $consumer->subscribe([$setting['topic_name']]);
    
    echo "Waiting for partition assignment... (make take some time when\n";
    echo "quickly re-joining the group after leaving it.)\n";
    
    while (true) {
        $message = $consumer->consume(30 * 1000);
        switch ($message->err) {
            case RD_KAFKA_RESP_ERR_NO_ERROR:
                var_dump($message);
                break;
            case RD_KAFKA_RESP_ERR__PARTITION_EOF:
                echo "No more messages; will wait for more\n";
                break;
            case RD_KAFKA_RESP_ERR__TIMED_OUT:
                echo "Timed out\n";
                break;
            default:
                throw new \Exception($message->errstr(), $message->err);
                break;
        }
    }
    
    ? >
    ```

2.  Run the following command to consume messages:

    ```
    php kafka-consumer.php
    ```


