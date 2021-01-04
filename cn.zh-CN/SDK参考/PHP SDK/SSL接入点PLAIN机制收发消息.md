# SSL接入点PLAIN机制收发消息

本文介绍如何在公网环境下使用PHP SDK接入消息队列Kafka版的SSL接入点并使用PLAIN机制收发消息。

-   [安装GCC](https://gcc.gnu.org/install/)
-   [安装PHP](https://www.php.net/downloads)
-   [安装PECL](https://www.php.net/manual/en/install.pecl.downloads.php)

## 安装C++依赖库

1.  执行以下命令切换到yum源配置目录/etc/yum.repos.d/。

    ```
    cd /etc/yum.repos.d/
    ```

2.  创建yum源配置文件confluent.repo。

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

3.  执行以下命令安装C++依赖库。

    ```
    yum install librdkafka-devel
    ```


## 安装PHP依赖库

1.  执行以下命令安装PHP依赖库。

    ```
    pecl install rdkafka
    ```

2.  在PHP的初始化文件php.ini中添加以下一行语句以开启扩展。

    ```
    extension=rdkafka.so
    ```


## 准备配置

1.  [下载SSL根证书](https://code.aliyun.com/alikafka/aliware-kafka-demos/raw/master/kafka-php-demo/vpc-ssl/ca-cert.pem)。

2.  创建消息队列Kafka版配置文件。

    ```
    <?php
    
    return [
        'sasl_plain_username' => 'xxx',
        'sasl_plain_password' => 'xxx',
        'bootstrap_servers' => "xxx:xx,xxx:xx",
        'topic_name' => 'xxx',
        'consumer_id' => 'xxx'
    ];
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

1.  创建发送消息程序kafka-producer.php。

    ```
    <?php
    
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

2.  执行以下命令发送消息。

    ```
    php kafka-producer.php
    ```


## 订阅消息

1.  创建订阅消息程序kafka-consumer.php。

    ```
    <?php
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
    
    ?>
    ```

2.  执行以下命令消费消息。

    ```
    php kafka-consumer.php
    ```


