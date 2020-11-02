---
keyword: [kafka, apache kafka, vpc, send and subscribe to messages]
---

# Access from a VPC

The topic uses Message Queue for Apache Kafka SDK for Java as an example to describe how to use the SDK to send and subscribe to messages in a virtual private cloud \(VPC\).

## Step 1: Make preparations and configurations

1.  Add Maven dependencies.

    The following code provides an example of Maven dependencies to be added:

    ```
    <dependency>
           <groupId>org.apache.kafka</groupId>
           <artifactId>kafka-clients</artifactId>
           <version>0.10.2.2</version>
    </dependency>                   
    ```

    **Note:** We recommend that you keep the broker and client versions consistent. For example, if your broker version is 0.10.2.2, we recommend that you use a client of version 0.10.2.2.

2.  Configure the client.

    1.  Prepare the configuration file kafka.properties.

        Example of kafka.properties:

        ```
        ## Set the endpoint to the default endpoint on the Instance Details page in the Message Queue for Apache Kafka console.
        bootstrap.servers=xxxxxxxxxxxxxxxxxxxxx
        ## Configure the topic. You can create the topic in the Message Queue for Apache Kafka console.
        topic=alikafka-topic-demo
        ## Configure the consumer group. You can create the consumer group in the Message Queue for Apache Kafka console.
        group.id=CID-consumer-group-demo
        ```

    2.  Load the configuration file.

        ```
        public class JavaKafkaConfigurer {
            private static Properties properties;
            public synchronized static Properties getKafkaProperties() {
               if (null ! = properties) {
                   return properties;
               }
               // Obtain the content of the kafka.properties file.
               Properties kafkaProperties = new Properties();
               try {
                   kafkaProperties.load(KafkaProducerDemo.class.getClassLoader().getResourceAsStream("kafka.properties"));
               } catch (Exception e) {
                   // If the file cannot be loaded, exit the program.
                   e.printStackTrace();
               }
               properties = kafkaProperties;
               return kafkaProperties;
            }
        }
        ```


## Step 2: Use SDK for Java to send messages

1.  The following code provides an example:

    ```
    // Load the kafka.properties file.
    Properties kafkaProperties =  JavaKafkaConfigurer.getKafkaProperties();
    Properties props = new Properties();
    // Set the endpoint to the default endpoint displayed on the Instance Details page in the console.
    props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, kafkaProperties.getProperty("bootstrap.servers"));
    // Specify the access protocol.
    props.put(CommonClientConfigs.SECURITY_PROTOCOL_CONFIG, "PLAINTEXT");
    // Specify the method for serializing Message Queue for Apache Kafka messages.
    props.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, "org.apache.kafka.common.serialization.StringSerializer");
    props.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, "org.apache.kafka.common.serialization.StringSerializer");
    // Specify the maximum time to wait for a request.
    props.put(ProducerConfig.MAX_BLOCK_MS_CONFIG, 30 * 1000);
    // Construct a thread-safe producer object. One producer object can serve one process.
    // To improve performance, you can construct more objects. We recommend that you construct no more than five objects.
    KafkaProducer<String, String> producer = new KafkaProducer<String, String>(props);
    // Construct a Message Queue for Apache Kafka message.
    String topic = kafkaProperties.getProperty("topic"); // The topic of the message. Enter the topic that you created in the Message Queue for Apache Kafka console.
    String value = "this is the message's value"; // The content of the message.
    ProducerRecord<String, String>  kafkaMessage =  new ProducerRecord<String, String>(topic, value);
    try {
      // Send the message and obtain a Future object.
      Future<RecordMetadata> metadataFuture = producer.send(kafkaMessage);
      // Synchronize the future object.
      RecordMetadata recordMetadata = metadataFuture.get();
      System.out.println("Produce ok:" + recordMetadata.toString());
    } catch (Exception e) {
      // Consider the retry scenario.
      System.out.println("error occurred");
      e.printStackTrace();
    }
    ```


## Step 3: Use SDK for Java to subscribe to messages

The following code provides an example:

```
// Load the kafka.properties file.
Properties kafkaProperties =  JavaKafkaConfigurer.getKafkaProperties();
Properties props = new Properties();
// Set the endpoint to the default endpoint displayed on the Instance Details page in the console.
props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, kafkaProperties.getProperty("bootstrap.servers"));
// Specify the maximum interval between two polling cycles.
//The value cannot exceed 30,000. Otherwise, the server kills idle connections.
props.put(ConsumerConfig.SESSION_TIMEOUT_MS_CONFIG, 25000);
// Specify the maximum number of messages that can be polled at a time.
// Do not set this parameter to an excessively large value. If polled messages are not all consumed before the next poll starts, load balancing is triggered and performance may deteriorate.
props.put(ConsumerConfig.MAX_POLL_RECORDS_CONFIG, 30);
// Specify the method for deserializing messages.
props.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, "org.apache.kafka.common.serialization.StringDeserializer");
props.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, "org.apache.kafka.common.serialization.StringDeserializer");
// Specify the consumer group of the current consumer instance. You must create the consumer group in the Message Queue for Apache Kafka console.
// The instances in a consumer group consume messages in load balancing mode.
props.put(ConsumerConfig.GROUP_ID_CONFIG, kafkaProperties.getProperty("group.id"));
// Construct a message object, which is a consumer instance.
KafkaConsumer<String, String> consumer = new org.apache.kafka.clients.consumer.KafkaConsumer<String, String>(props);
// Set one or more topics to which the consumer group subscribes.
// We recommend that you configure consumer instances with the same GROUP_ID_CONFIG value to subscribe to the same topics.
List<String> subscribedTopics =  new ArrayList<String>();
// If you want to subscribe to multiple topics, add them here.
// You must create the topics in the Message Queue for Apache Kafka console in advance.
subscribedTopics.add(kafkaProperties.getProperty("topic"));
consumer.subscribe(subscribedTopics);
// Consume messages in a loop.
while (true){
    try {
        ConsumerRecords<String, String> records = consumer.poll(1000);
        // All messages must be consumed before the next polling cycle starts. The total duration cannot exceed the timeout interval specified by SESSION_TIMEOUT_MS_CONFIG.
        // We recommend that you create a separate thread pool to consume messages and then asynchronously return the results.
        for (ConsumerRecord<String, String> record : records) {
            System.out.println(String.format("Consume partition:%d offset:%d", record.partition(), record.offset()));
        }
    }
    catch (Exception e) {
        try {
            Thread.sleep(1000);
        }
        catch (Throwable ignore) {
        }
        // See common errors.
        e.printStackTrace();
    }
}
```

## Multi-language sample code

Message Queue for Apache Kafka SDK for Java is used as an example in this topic. For more information about how to use Message Queue for Apache Kafka SDK for other languages to send and subscribe to messages, see [Message Queue for Apache Kafkademos](https://code.aliyun.com/alikafka/aliware-kafka-demos/tree/master).

