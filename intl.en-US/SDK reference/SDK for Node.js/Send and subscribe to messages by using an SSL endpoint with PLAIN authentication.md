# Send and subscribe to messages by using an SSL endpoint with PLAIN authentication

This topic describes how a Node.js client uses SDK for Node.js to connect to the Secure Sockets Layer \(SSL\) endpoint of a Message Queue for Apache Kafka instance over the Internet and uses the PLAIN mechanism to send and subscribe to messages.

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


## Prepare configurations

1.  [Download an SSL root certificate](https://code.aliyun.com/alikafka/aliware-kafka-demos/raw/master/kafka-nodejs-demo/vpc-ssl/ca-cert.pem).

2.  Create a Message Queue for Apache Kafka configuration file named setting.js.

    ```
    module.exports = {
        'sasl_plain_username': 'XXX',
        'sasl_plain_password': 'XXX',
        'bootstrap_servers': ["XXX"],
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

2.  Run the following command to consume messages:

    ```
    node consumer.js
    ```


