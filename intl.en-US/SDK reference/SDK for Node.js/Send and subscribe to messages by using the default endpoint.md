# Send and subscribe to messages by using the default endpoint

This topic describes how a Node.js client uses SDK for Node.js to connect to the default endpoint of Message Queue for Apache Kafka in a virtual private cloud \(VPC\) and send and subscribe to messages.

-   [Install GCC](https://gcc.gnu.org/install/)
-   [Install Node.js.](https://nodejs.org/en/download/)

    **Note:** The version of Node.js must be 4.0.0 or later.

-   [Install OpenSSL](https://www.openssl.org/source/)

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


## Install the Node.js library

1.  Run the following command to specify the file path of the OpenSSL header for the preprocessor:

    ```
    
            export CPPFLAGS=-I/usr/local/opt/openssl/include 
          
    ```

2.  Run the following command to specify the path of the OpenSSL library for the connector:

    ```
    
            export LDFLAGS=-L/usr/local/opt/openssl/lib 
          
    ```

3.  Run the following command to install the Node.js library:

    ```
    
            npm install i --unsafe-perm node-rdkafka 
          
    ```


## Preparations

1.  Create a Message Queue for Apache Kafka configuration file named setting.js.

    ```
    module.exports = {
        'bootstrap_servers': ["kafka-ons-internet.aliyun.com:8080"],
        'topic_name': 'xxx',
        'consumer_id': 'xxx'
    }
    ```

    |Parameter|Description|
    |---------|-----------|
    |bootstrap\_servers|The default endpoint. You can obtain the default endpoint in the **Basic Information** section of the **Instance Details** page in the Message Queue for Apache Kafka console.|
    |topic\_name|The name of the topic. You can obtain the name of the topic on the **Topics** page in the Message Queue for Apache Kafka console.|
    |consumer\_id|The name of the consumer group. You can obtain the name of the consumer group on the **Consumer Groups** page in the Message Queue for Apache Kafka console.|


## Send messages

1.  Create a message sender named producer.js.

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
        'dr_msg_cb': true
    });
    
    var connected = false
    
    producer.setPollInterval(100);
    
    producer.connect();
    
    
    producer.on('ready', function() {
      connected = true
      console.log("connect ok")
    });
    
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
    
    function produce() {
      try {
        producer.produce(
          config['topic_name'],   
          null,      
          new Buffer('Hello Ali Kafka'),      
          null,   
          Date.now()
        );
      } catch (err) {
        console.error('A problem occurred when sending our message');
        console.error(err);
      }
    
    }
    
    producer.on('delivery-report', function(err, report) {
        console.log("delivery-report: producer ok");
    });
    
    producer.on('event.error', function(err) {
        console.error('event.error:' + err);
    })
    
    setInterval(produce,1000,"Interval");
    ```

2.  Run the following command to send messages:

    ```
    node producer.js
    ```


## Subscribe to messages

1.  Create a subscription program named consumer.js.

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

2.  Run the following command to consume messages:

    ```
    node consumer.js
    ```


