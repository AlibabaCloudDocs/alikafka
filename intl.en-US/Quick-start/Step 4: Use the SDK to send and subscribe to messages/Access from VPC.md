# Access from VPC

This topic takes Java as an example to describe how to use the Message Queue for Apache Kafka SDK to subscribe to and receive messages in a Virtual Private Cloud \(VPC\) environment.

## Step 1: Make preparations and configurations

The sample code for adding Maven dependencies is as follows:

1.  Add Maven dependencies

    The sample code for adding Maven dependencies is as follows:

    ```
    // The version of the Message Queue for Apache Kafka broker is 0.10.2.2, and the client version is 0.10.2.2. <dependency>
        <groupId>org.apache.kafka</groupId>
        <artifactId>kafka-clients</artifactId>
        <version>0.10.2.2</version>
    </dependency>
    ```

2.  Configure the client
    1.  Prepare the kafka.properties configuration file.

        Modify the file according to the following code:

        ```
        ## Set the endpoint to the default endpoint displayed on the Instance Details page in the console. 
        bootstrap.servers=xxxxxxxxxxxxxxxxxxxxx 
        ## Set the topic to the one you created in the console. 
        topic=alikafka-topic-demo 
        ## Set the consumer group to the one you created in the console. 
        group.id=CID-consumer-group-demo
        ```

    2.  Load the configuration file.

        Modify the file according to the following code:

        ```
        public class JavaKafkaConfigurer {
            private static Properties properties;
            public synchronized static Properties getKafkaProperties() {
                if (null ! = properties) {
                    return properties;
                }
                // Obtain the content of the kafka.properties configuration file.
                Properties kafkaProperties = new Properties();
                try {
                    kafkaProperties.load(KafkaProducerDemo.class.getClassLoader().getResourceAsStream("kafka.properties"));
                }
                catch (Exception e) {
                    // Exit the program if the file loading failed.
                    e.printStackTrace();
                }
                properties = kafkaProperties;
                return kafkaProperties;
            }
        }
        ```


## Step 2: Use the Java SDK to send messages

**Note:** The Java SDK is used in the following example. For more information about how to use SDKs in other languages to send messages, see [Message Queue for Apache Kafka demos](https://github.com/AliwareMQ/aliware-kafka-demos/?spm=a2c4g.11186623.2.13.56e26972EpvyM9).

The sample code is as follows:

```
// Load kafka.properties.
Properties kafkaProperties =  JavaKafkaConfigurer.getKafkaProperties();
Properties props = new Properties();
// Set the endpoint to the default endpoint displayed on the Instance Details page in the console.
props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, kafkaProperties.getProperty("bootstrap.servers"));
// The access protocol.
props.put(CommonClientConfigs.SECURITY_PROTOCOL_CONFIG, "PLAINTEXT");
// The method for serializing messages of Message Queue for Apache Kafka.
props.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, "org.apache.kafka.common.serialization.StringSerializer");
props.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, "org.apache.kafka.common.serialization.StringSerializer");
// The maximum wait time for a request. 
props.put(ProducerConfig.MAX_BLOCK_MS_CONFIG, 30 * 1000);
// Construct a producer object, which is thread-safe. One producer object can serve a process.
// To improve performance, you can construct a few more objects. We recommend that you construct no more than five objects.
KafkaProducer<String, String> producer = new KafkaProducer<String, String>(props);
// Construct a Message Queue for Apache Kafka message.
String topic = kafkaProperties.getProperty("topic"); // The topic of the message. Enter the topic you have created in the console.
String value = "this is the message's value"; // The message content.
ProducerRecord<String, String>  kafkaMessage =  new ProducerRecord<String, String>(topic, value);
try {
    // Send the message and obtain a future object.
    Future<RecordMetadata> metadataFuture = producer.send(kafkaMessage);
    // Synchronize the obtained future object.
    RecordMetadata recordMetadata = metadataFuture.get();
    System.out.println("Produce ok:" + recordMetadata.toString());
}
catch (Exception e) {
    // Consider the retry scenario.
    System.out.println("error occurred");
    e.printStackTrace();
}
```

## Step 3: Use the Java SDK to subscribe to messages

**Note:** The Java SDK is used in the following example. For more information about how to use SDKs in other languages to subscribe to messages, see [Message Queue for Apache Kafka demos](https://github.com/AliwareMQ/aliware-kafka-demos/?spm=a2c4g.11186623.2.14.56e26972EpvyM9).

The sample code is as follows:

```
// Load kafka.properties.
Properties kafkaProperties =  JavaKafkaConfigurer.getKafkaProperties();
Properties props = new Properties();
// Set the endpoint to the default endpoint displayed on the Instance Details page in the console.
props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, kafkaProperties.getProperty("bootstrap.servers"));
// The maximum allowed interval between two polling cycles.
//The value cannot exceed 30,000. Otherwise, the server kills idle connections.
props.put(ConsumerConfig.SESSION_TIMEOUT_MS_CONFIG, 25000);
// The maximum number of messages that can be polled at a time.
// Do not set the number to an excessively large value. If too many messages are polled but fail to be completely consumed before the next poll starts, load balancing is triggered, which causes freezing.
props.put(ConsumerConfig.MAX_POLL_RECORDS_CONFIG, 30);
// The method for deserializing messages.
props.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, "org.apache.kafka.common.serialization.StringDeserializer");
props.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, "org.apache.kafka.common.serialization.StringDeserializer");
// The consumer group of the current consumer instance. Enter the consumer group you have created in the console.
// The consumer instances that belong to the same consumer group. These instances consume messages in load balancing mode.
props.put(ConsumerConfig.GROUP_ID_CONFIG, kafkaProperties.getProperty("group.id"));
// Construct a message object, that is, generate a consumer instance.
KafkaConsumer<String, String> consumer = new org.apache.kafka.clients.consumer.KafkaConsumer<String, String>(props);
// Set the topics to which the consumer group subscribes. One consumer group can subscribe to multiple topics.
// We recommend that you set this parameter to the same value for the consumer instances with the same GROUP_ID_CONFIG value.
List<String> subscribedTopics =  new ArrayList<String>();
// If you want to subscribe to multiple topics, add them here.
// You must create the topics in the console before adding them.
subscribedTopics.add(kafkaProperties.getProperty("topic"));
consumer.subscribe(subscribedTopics);
// Consume messages cyclically.
while (true){
    try {
        ConsumerRecords<String, String> records = consumer.poll(1000);
        // The messages must be completely consumed before the next polling cycle starts. The total duration cannot exceed the timeout interval specified by SESSION_TIMEOUT_MS_CONFIG.
        // We recommend that you create a separate thread pool to consume messages and return the results asynchronously.
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
        // For more information about common errors, see FAQ.
        e.printStackTrace();
    }
}
```

