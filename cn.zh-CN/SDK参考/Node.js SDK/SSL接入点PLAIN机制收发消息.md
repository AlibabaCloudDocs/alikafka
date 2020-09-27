# SSL接入点PLAIN机制收发消息

本文介绍如何在公网环境下使用Node.js SDK接入消息队列Kafka版的SSL接入点并使用PLAIN机制收发消息。

## 准备配置

1.  [下载SSL根证书](https://code.aliyun.com/alikafka/aliware-kafka-demos/raw/master/kafka-nodejs-demo/vpc-ssl/ca-cert.pem)。

2.  创建Kafka配置文件setting.js。

    ```
    module.exports = {
        'sasl_plain_username': 'XXX',
        'sasl_plain_password': 'XXX',
        'bootstrap_servers': ["XXX"],
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
    |bootstrap\_servers|默认接入点。您可在消息队列Kafka版控制台的**实例详情**页面的**基本信息**区域获取。|
    |topic\_name|Topic名称。您可在消息队列Kafka版控制台的**Topic管理**页面获取。|
    |consumer\_id|Consumer Group名称。您可在消息队列Kafka版控制台的**Consumer Group管理**页面获取。|


## 发送消息

1.  创建发送消息程序producer.js。

    ```
    const Kafka = require('node-rdkafka');
    const config = require('./setting');
    console.log("features:" + Kafka.features);
    console.log(Kafka.librdkafkaVersion);
    
    var producer = new Kafka.Producer({
        /*'debug': 'all', */
        'api.version.request': 'true',
        'bootstrap.servers': config['bootstrap_servers'],
        'dr_cb': true,
        'dr_msg_cb': true,
        'security.protocol' : 'sasl_ssl',
        'ssl.ca.location' : './ca-cert.pem',
        'sasl.mechanisms' : 'PLAIN',
        'sasl.username' : config['sasl_plain_username'],
        'sasl.password' : config['sasl_plain_password']
    });
    
    var connected = false
    
    producer.setPollInterval(100);
    
    producer.connect();
    
    producer.on('ready', function() {
      connected = true
      console.log("connect ok")
    
      });
    
    function produce() {
      try {
        producer.produce(
          config['topic_name'],
          new Buffer('Hello Ali Kafka'),
          null,
          Date.now()
        );
      } catch (err) {
        console.error('A problem occurred when sending our message');
        console.error(err);
      }
    }
    
    producer.on("disconnected", function() {
      connected = false;
      producer.connect();
    })
    
    producer.on('event.log', function(event) {
          console.log("event.log", event);
    });
    
    producer.on("error", function(error) {
        console.log("error:" + error);
    });
    
    producer.on('delivery-report', function(err, report) {
        console.log("delivery-report: producer ok");
    });
    // Any errors we encounter, including connection errors
    producer.on('event.error', function(err) {
        console.error('event.error:' + err);
    })
    
    setInterval(produce,1000,"Interval");
    ```

2.  执行以下命令发送消息。

    ```
    node producer.js
    ```


## 订阅消息

1.  创建订阅消息程序fileconsumer.js。

    ```
    const Kafka = require('node-rdkafka');
    const config = require('./setting');
    console.log(Kafka.features);
    console.log(Kafka.librdkafkaVersion);
    console.log(config)
    
    var consumer = new Kafka.KafkaConsumer({
        /*'debug': 'all',*/
        'api.version.request': 'true',
        'bootstrap.servers': config['bootstrap_servers'],
        'security.protocol' : 'sasl_ssl',
        'ssl.ca.location' : './ca-cert.pem',
        'sasl.mechanisms' : 'PLAIN',
        'message.max.bytes': 32000,
        'fetch.max.bytes' : 32000,
        'fetch.message.max.bytes': 32000,
        'max.partition.fetch.bytes': 32000,
        'sasl.username' : config['sasl_plain_username'],
        'sasl.password' : config['sasl_plain_password'],
        'group.id' : config['consumer_id']
    });
    
    consumer.connect();
    
    consumer.on('ready', function() {
      console.log("connect ok");
      consumer.subscribe([config['topic_name']]);
      consumer.consume();
    })
    
    consumer.on('data', function(data) {
      console.log(data);
    });
    
    
    consumer.on('event.log', function(event) {
          console.log("event.log", event);
    });
    
    consumer.on('error', function(error) {
        console.log("error:" + error);
    });
    
    consumer.on('event', function(event) {
            console.log("event:" + event);
    });
    ```

2.  执行以下命令消费消息。

    ```
    node consumer.js
    ```


