# Use the default endpoint to send and subscribe to messages

This topic describes how a PHP client uses SDK for PHP to connect to the default endpoint of Message Queue for Apache Kafka and send and subscribe to messages in a virtual private cloud \(VPC\).

-   GNU Compiler Collection \(GCC\) is installed. For more information, see [Installing GCC](https://gcc.gnu.org/install/).
-   PHP is installed. For more information, see [Download PHP](https://www.php.net/downloads).
-   PHP Extension Community Library \(PECL\) is installed. For more information, see [Downloading PECL extensions](https://www.php.net/manual/en/install.pecl.downloads.php).

## Install the C ++ library

1.  Run the following command to switch to the yum repository directory /etc/yum.repos.d/:

    ```
    cd /etc/yum.repos.d/
    ```

2.  Create a yum repository configuration file confluent.repo.

    ```
    [Confluent.dist]
    name=Confluent repository (dist)
    baseurl=https://packages.confluent.io/rpm/5.1/7
    gpgcheck=1
    gpgkey=https://packages.confluent.io/rpm/5.1/archive.key
    enabled=1
    
    [Confluent]
    name=Confluent repository
    baseurl=https://packages.confluent.io/rpm/5.1
    gpgcheck=1
    gpgkey=https://packages.confluent.io/rpm/5.1/archive.key
    enabled=1
    ```

3.  Run the following command to install the C ++ library:

    ```
    yum install librdkafka-devel
    ```


## Install the PHP library

1.  Run the following command to install the PHP library:

    ```
    pecl install rdkafka
    ```

2.  In the PHP initialization file php.ini, add the following line to enable Kafka extensions:

    ```
    extension=rdkafka.so
    ```


## Prepare configurations

1.  Create a configuration file setting.php.

    ```
    <? php
    
    return [
        'bootstrap_servers' => "xxx:xx,xxx:xx",
        'topic_name' => 'xxx',
        'consumer_id' => 'xxx'
    ];
    ```

    |Parameter|Description|
    |---------|-----------|
    |bootstrap\_servers|The default endpoint of the Message Queue for Apache Kafka instance. You can obtain the default endpoint in the **Basic Information** section of the **Instance Details** page in the Message Queue for Apache Kafka console.|
    |topic\_name|The name of the topic. You can obtain the name of the topic on the **Topics** page in the Message Queue for Apache Kafka console.|
    |consumer\_id|The name of the consumer group. You can obtain the name of the consumer group on the **Consumer Groups** page in the Message Queue for Apache Kafka console.|


## Send messages

1.  Create a message sender kafka-producer.php.

    ```
    <? php
    
    $setting = require __DIR__ . '/setting.php';
    
    $conf = new RdKafka\Conf();
    $conf->set('api.version.request', 'true');
    $conf->set('message.send.max.retries', 5);
    $rk = new RdKafka\Producer($conf);
    ## If want to debug, set log level to LOG_DEBUG
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

1.  Create a subscription program kafka-consumer.php.

    ```
    <? php
    $setting = require __DIR__ . '/setting.php';
    $conf = new RdKafka\Conf();
    $conf->set('api.version.request', 'true');
    
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


