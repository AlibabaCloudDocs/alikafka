# Send and subscribe to messages through a default endpoint

This topic describes how to use the Java SDK to gain access to Message Queue for Apache Kafka to send and subscribe to messages.

## Demo Library

For more information, see the demos and descriptions in the [demo library of Message Queue for Apache Kafka](https://github.com/AliwareMQ/aliware-kafka-demos).

## Prerequisites

You have completed the [Access prerequisites](/intl.en-US/SDK reference/Access prerequisites.md).

## Add the Maven dependency

```
// The version of the Message Queue for Apache Kafka broker is 0.10.2.2. We recommend that you use a client in the version of 0.10.2.2 as well. 
<dependency>
    <groupId>org.apache.kafka</groupId>
    <artifactId>kafka-clients</artifactId>
    <version>0.10.2.2</version>
</dependency>
```

## 2. Use the Java SDK

1.  Prepare the configuration file kafka.properties. You can modify the configuration file by referring to the demos and descriptions in the [demo library of Message Queue for Apache Kafka](https://github.com/AliwareMQ/aliware-kafka-demos).

    ```
    ## The endpoint you obtained in the console. 
    bootstrap.servers=xxxxxxxxxxxxxxx
    ## The topic you created in the console. 
    topic=alikafka-topic-demo
    ## The consumer group you created in the console. 
    group.id=CID-consumer-group-demo
    ```

2.  Load the configuration file `kafka.properties`.

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

3.  Send a message.

    ```
    public class KafkaProducerDemo {
        public static void main(String args[]) {
            // Load kafka.properties.
            Properties kafkaProperties =  JavaKafkaConfigurer.getKafkaProperties();
            Properties props = new Properties();
            // Specify the endpoint. We recommend that you obtain the endpoint of the corresponding topic in the console.
            props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, kafkaProperties.getProperty("bootstrap.servers"));
            // Specify the serialization format of a Message Queue for Apache Kafka message.
            props.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, "org.apache.kafka.common.serialization.StringSerializer");
            props.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, "org.apache.kafka.common.serialization.StringSerializer");
            // Specify the maximum waiting time for a request.
            props.put(ProducerConfig.MAX_BLOCK_MS_CONFIG, 30 * 1000);
            // Construct a thread-safe producer object.
            // Generally, one producer object is enough for one process. To improve performance, you can construct a maximum of five objects.
            KafkaProducer<String, String> producer = new KafkaProducer<String, String>(props);
            // Construct a Message Queue for Apache Kafka message.
            String topic = kafkaProperties.getProperty("topic");
            //Specify the topic to which the message belongs. Obtain the topic from the console. If no topic is available, create one first.
            String value = "this is the message's value";
            //Specify the content of the message.
            ProducerRecord<String, String>  kafkaMessage =  new ProducerRecord<String, String>(topic, value);
            try {
                // Send the message and obtain a future object.
                Future<RecordMetadata> metadataFuture = producer.send(kafkaMessage);
                // Synchronize the obtained future object.
                RecordMetadata recordMetadata = metadataFuture.get();
                System.out.println("Produce ok:" + recordMetadata.toString());
            }
            catch (Exception e) {
                // If a retry is required when an exception occurs, see FAQ.
                System.out.println("error occurred");
                e.printStackTrace();
            }
        }
    }     
    ```

4.  Consume the message.


```
public class KafkaConsumerDemo {
    public static void main(String args[]) {
        // Load kafka.properties.
        Properties kafkaProperties =  JavaKafkaConfigurer.getKafkaProperties();
        Properties props = new Properties();
        // Specify the endpoint. We recommend that you obtain the endpoint of the corresponding topic in the console.
        props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, kafkaProperties.getProperty("bootstrap.servers"));
        // The default value is 30000 ms. You can adjust the value based on your business scenarios. We recommend that you set a large value, to prevent load balancing between consumers when no heartbeat message is sent before timeout.
        props.put(ConsumerConfig.SESSION_TIMEOUT_MS_CONFIG, 25000);
        // Specify the maximum number of messages to be polled each time.
        // Do not set the number to an excessively large value. If too many messages are polled but fail to be completely consumed before the next poll starts, load balancing is triggered, which causes freezing.
        props.put(ConsumerConfig.MAX_POLL_RECORDS_CONFIG, 30);
        // Specify the deserialization format of the message.
        props.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, "org.apache.kafka.common.serialization.StringDeserializer");
        props.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, "org.apache.kafka.common.serialization.StringDeserializer");
        // Specify the consumer group to which the current consumption instance belongs. Obtain the consumer group in the console. If no consumer group is available, create one first.
        // Consumer instances in the same consumer group consume messages in a load balancing mode.
        props.put(ConsumerConfig.GROUP_ID_CONFIG, kafkaProperties.getProperty("group.id"));
        // Construct a message object. In other words, generate a consumer instance.
        KafkaConsumer<String, String> consumer = new org.apache.kafka.clients.consumer.KafkaConsumer<String, String>(props);
        // Configure one or more topics that are to be subscribed by the consumer group. If the values of GROUP_ID_CONFIG of different consumer instances are the same, we recommend that you specify the same topic subscription settings for these consumer instances.
        List<String> subscribedTopics =  new ArrayList<String>();
        // You must create the topics in the console before adding them.
        subscribedTopics.add(kafkaProperties.getProperty("topic"));
        consumer.subscribe(subscribedTopics);
        // Consume messages cyclically.
        while (true){
            try {
                ConsumerRecords<String, String> records = consumer.poll(1000);
                // The data must be completely consumed before the next poll starts, and the total time consumed cannot exceed the value of SESSION_TIMEOUT_MS_CONFIG.
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
                // For more information about troubleshooting for exceptions, see FAQ.
                e.printStackTrace();
            }
        }
    }
}
```

