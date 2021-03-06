---
keyword: [java, vpc, kafka, send and subscribe to messages, scram]
---

# Use the SCRAM mechanism to send and subscribe to messages through an SASL endpoint

This topic describes how a Java client connects to Message Queue for Apache Kafka through an SASL endpoint in a virtual private cloud \(VPC\) and uses the SCRAM mechanism to send and subscribe to messages.

-   [JDK 1.8 or later has been installed.](https://www.oracle.com/java/technologies/javase-downloads.html)
-   [Maven 2.5 or later has been installed.](http://maven.apache.org/download.cgi#)
-   [Authorize SASL users](/intl.en-US/Access control/Authorize SASL users.md)

## Install Java dependencies

1.  Add the following dependency to the pom.xml file.

    ```
    <dependency>
           <groupId>org.apache.kafka</groupId>
           <artifactId>kafka-clients</artifactId>
           <version>0.10.2.2</version>
    </dependency> 
    ```

    **Note:** We recommend that you keep the version of the client consistent with that of the broker. That is, the client library version must be consistent with the major version of the Message Queue for Apache Kafka instance. You can query the major version of the Message Queue for Apache Kafka instance on the **Instance Details** page of the Message Queue for Apache Kafka console.


## Prepare configurations

1.  Create a Log4j configuration file log4j.properties.

    ```
    log4j.rootLogger=INFO, STDOUT
    
    log4j.appender.STDOUT=org.apache.log4j.ConsoleAppender
    log4j.appender.STDOUT.layout=org.apache.log4j.PatternLayout
    log4j.appender.STDOUT.layout.ConversionPattern=[%d] %p %m (%c)%n
    ```

2.  Create a JAAS configuration file kafka\_client\_jaas\_scram.conf.

    ```
    KafkaClient {
      org.apache.kafka.common.security.scram.ScramLoginModule required
      username="xxxx"
      password="xxxx";
    };
    ```

3.  Create a Kafka configuration file kafka.properties.

    ```
    ## The SASL endpoint, which is obtained from the Message Queue for Apache Kafka console.
    bootstrap.servers=xxxx
    ## The topic, which is created in the Message Queue for Apache Kafka console.
    topic=xxxx
    ## The consumer group, which is created in the Message Queue for Apache Kafka console.
    group.id=xxxx
    ## The JAAS configuration file.
    java.security.auth.login.config.scram=/xxxx/kafka_client_jaas_scram.conf
    ```

4.  Create a profile loader JavaKafkaConfigurer.java.

    ```
    import java.util.Properties;
    
    public class JavaKafkaConfigurer {
    
        private static Properties properties;
    
        public static void configureSaslScram() {
            // If you have set the path by using methods such as -D, do not set it again.
            if (null == System.getProperty("java.security.auth.login.config")) {
                // Replace XXX with your own path.
                // Ensure that the path is readable by the file system. Do not compress the file into a JAR package.
                System.setProperty("java.security.auth.login.config", getKafkaProperties().getProperty("java.security.auth.login.config.scram"));
            }
        }
    
        public synchronized static Properties getKafkaProperties() {
            if (null ! = properties) {
                return properties;
            }
            // Obtain the content of the configuration file kafka.properties.
            Properties kafkaProperties = new Properties();
            try {
                kafkaProperties.load(KafkaProducerDemo.class.getClassLoader().getResourceAsStream("kafka.properties"));
            } catch (Exception e) {
                // Exit the program if the file loading failed.
                e.printStackTrace();
            }
            properties = kafkaProperties;
            return kafkaProperties;
        }
    }
    ```


## Send messages

1.  Create a message sender KafkaProducerDemo.java.

    ```
    import java.util.ArrayList;
    import java.util.List;
    import java.util.Properties;
    import java.util.concurrent.Future;
    
    import org.apache.kafka.clients.CommonClientConfigs;
    import org.apache.kafka.clients.producer.KafkaProducer;
    import org.apache.kafka.clients.producer.ProducerConfig;
    import org.apache.kafka.clients.producer.ProducerRecord;
    import org.apache.kafka.clients.producer.RecordMetadata;
    import org.apache.kafka.common.config.SaslConfigs;
    import org.apache.kafka.common.config.SslConfigs;
    
    public class KafkaProducerDemo {
    
        public static void main(String args[]) {
            // Set the path of the JAAS configuration file.
            JavaKafkaConfigurer.configureSaslScram();
    
            // Load kafka.properties.
            Properties kafkaProperties =  JavaKafkaConfigurer.getKafkaProperties();
    
            Properties props = new Properties();
            // Specify the endpoint. We recommend that you obtain the endpoint of the corresponding topic in the console.
            props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, kafkaProperties.getProperty("bootstrap.servers"));
    
            // Specify the access protocol.
            props.put(CommonClientConfigs.SECURITY_PROTOCOL_CONFIG, "SASL_PLAINTEXT");
            // Specify the SCRAM mechanism.
            props.put(SaslConfigs.SASL_MECHANISM, "SCRAM-SHA-256");     
    
            // Specify the method for serializing messages of Message Queue for Apache Kafka.
            props.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, "org.apache.kafka.common.serialization.StringSerializer");
            props.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, "org.apache.kafka.common.serialization.StringSerializer");
            // Specify the maximum waiting time for a request.
            props.put(ProducerConfig.MAX_BLOCK_MS_CONFIG, 30 * 1000);
            // Set the number of retries in the client.
            props.put(ProducerConfig.RETRIES_CONFIG, 5);
            // Set the interval of retries in the client.
            props.put(ProducerConfig.RECONNECT_BACKOFF_MS_CONFIG, 3000);
            // Construct a producer object, which is thread-safe. One producer object can serve a process.
            // To improve performance, you can construct a few more objects. We recommend that you construct no more than five objects.
            KafkaProducer<String, String> producer = new KafkaProducer<String, String>(props);
    
            // Construct a Message Queue for Apache Kafka message.
            String topic = kafkaProperties.getProperty("topic"); // The topic of the message. Enter the topic you have created in the console.
            String value = "this is the message's value"; //Specify the content of the message.
    
            try {
                // You can speed up the process by obtaining Future objects in batches. Note that the batch size must not be too large.
                List<Future<RecordMetadata>> futures = new ArrayList<Future<RecordMetadata>>(128);
                for (int i =0; i < 100; i++) {
                    // Send the message and obtain a Future object.
                    ProducerRecord<String, String> kafkaMessage =  new ProducerRecord<String, String>(topic, value + ": " + i);
                    Future<RecordMetadata> metadataFuture = producer.send(kafkaMessage);
                    futures.add(metadataFuture);
    
                }
                producer.flush();
                for (Future<RecordMetadata> future: futures) {
                    // Synchronize the obtained Future object.
                    try {
                        RecordMetadata recordMetadata = future.get();
                        System.out.println("Produce ok:" + recordMetadata.toString());
                    } catch (Throwable t) {
                        t.printStackTrace();
                    }
                }
            } catch (Exception e) {
                // Catch the error for handling if the client fails to send data after a retry.
                System.out.println("error occurred");
                e.printStackTrace();
            }
        }
    }
    ```

2.  Compile and run KafkaProducerDemo.java.


## Subscribe to messages

1.  Use either of the following methods to subscribe to messages:

    -   A single consumer subscribes to messages
        1.  Create a single-consumer subscription program KafkaConsumerDemo.java.

            ```
            import java.util.ArrayList;
            import java.util.List;
            import java.util.Properties;
            
            import org.apache.kafka.clients.CommonClientConfigs;
            import org.apache.kafka.clients.consumer.ConsumerConfig;
            import org.apache.kafka.clients.consumer.ConsumerRecord;
            import org.apache.kafka.clients.consumer.ConsumerRecords;
            import org.apache.kafka.clients.consumer.KafkaConsumer;
            import org.apache.kafka.clients.producer.ProducerConfig;
            import org.apache.kafka.common.config.SaslConfigs;
            
            public class KafkaConsumerDemo {
            
                public static void main(String args[]) {
                    // Set the path of the JAAS configuration file.
                    JavaKafkaConfigurer.configureSaslScram();
            
                    // Load kafka.properties.
                    Properties kafkaProperties =  JavaKafkaConfigurer.getKafkaProperties();
            
                    Properties props = new Properties();
                    // Specify the endpoint. We recommend that you obtain the endpoint of the corresponding topic in the console.
                    props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, kafkaProperties.getProperty("bootstrap.servers"));
            
                    // Specify the access protocol.
                    props.put(CommonClientConfigs.SECURITY_PROTOCOL_CONFIG, "SASL_PLAINTEXT");
                    // Specify the SCRAM mechanism.
                    props.put(SaslConfigs.SASL_MECHANISM, "SCRAM-SHA-256");
            
                    // Specify the session timeout period. If the consumer does not return a heartbeat before the session times out, the broker determines that the consumer is not alive. Then the broker removes the consumer from the consumer group and triggers rebalancing. The default value is 30s.
                    props.put(ConsumerConfig.SESSION_TIMEOUT_MS_CONFIG, 30000);
                    // The maximum number of messages that can be polled at a time.
                    // Do not set the number to an excessively large value. If excessive messages are polled but fail to be completely consumed before the next poll starts, load balancing is triggered, which causes freezing.
                    props.put(ConsumerConfig.MAX_POLL_RECORDS_CONFIG, 30);
                    // Specify the method for deserializing messages.
                    props.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, "org.apache.kafka.common.serialization.StringDeserializer");
                    props.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, "org.apache.kafka.common.serialization.StringDeserializer");
                    // The consumer group of the current consumer instance. Enter the consumer group you have created in the console.
                    // The consumer instances that belong to the same consumer group. These instances consume messages in load balancing mode.
                    props.put(ConsumerConfig.GROUP_ID_CONFIG, kafkaProperties.getProperty("group.id"));
                    // Construct a message object to generate a consumer instance.
                    KafkaConsumer<String, String> consumer = new org.apache.kafka.clients.consumer.KafkaConsumer<String, String>(props);
                    // Set the topics to which the consumer group subscribes. One consumer group can subscribe to multiple topics.
                    // We recommend that you set the topic to the same value for the consumer instances with the same GROUP_ID_CONFIG value.
                    List<String> subscribedTopics =  new ArrayList<String>();
                    // If you want to subscribe to multiple topics, add them here.
                    // You must create the topics in the console before you add them.
                    String topicStr = kafkaProperties.getProperty("topic");
                    String[] topics = topicStr.split(",");
                    for (String topic: topics) {
                        subscribedTopics.add(topic.trim());
                    }
                    consumer.subscribe(subscribedTopics);
            
                    // Cyclically consume messages.
                    while (true){
                        try {
                            ConsumerRecords<String, String> records = consumer.poll(1000);
                            // The messages must be completely consumed before the next polling cycle starts. The total duration cannot exceed the timeout interval specified by SESSION_TIMEOUT_MS_CONFIG.
                            // We recommend that you create a separate thread pool to consume messages and asynchronously return the results.
                            for (ConsumerRecord<String, String> record : records) {
                                System.out.println(String.format("Consume partition:%d offset:%d", record.partition(), record.offset()));
                            }
                        } catch (Exception e) {
                            try {
                                Thread.sleep(1000);
                            } catch (Throwable ignore) {
            
                            }
                            e.printStackTrace();
                        }
                    }
                }
            }
            ```

        2.  Compile and run KafkaConsumerDemo.java.
    -   Multiple consumers subscribe to messages
        1.  Create a multi-consumer subscription program KafkaMultiConsumerDemo.java.

            ```
            import java.util.ArrayList;
            import java.util.List;
            import java.util.Properties;
            import java.util.concurrent.atomic.AtomicBoolean;
            
            import org.apache.kafka.clients.CommonClientConfigs;
            import org.apache.kafka.clients.consumer.ConsumerConfig;
            import org.apache.kafka.clients.consumer.ConsumerRecord;
            import org.apache.kafka.clients.consumer.ConsumerRecords;
            import org.apache.kafka.clients.consumer.KafkaConsumer;
            import org.apache.kafka.clients.producer.ProducerConfig;
            import org.apache.kafka.common.config.SaslConfigs;
            import org.apache.kafka.common.errors.WakeupException;
            
            /**
             * This tutorial demonstrates how to enable multiple consumers to simultaneously consume topics in one process.
             * Ensure that the number of global consumers does not exceed the total number of partitions of the topics that the consumers subscribed to.
             */
            public class KafkaMultiConsumerDemo {
            
                public static void main(String args[]) throws InterruptedException {
                    // Set the path of the JAAS configuration file.
                    JavaKafkaConfigurer.configureSaslScram();
            
                    // Load kafka.properties.
                    Properties kafkaProperties = JavaKafkaConfigurer.getKafkaProperties();
            
                    Properties props = new Properties();
                    // Specify the endpoint. We recommend that you obtain the endpoint of the corresponding topic in the console.
                    props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, kafkaProperties.getProperty("bootstrap.servers"));
            
                    // Specify the access protocol.
                    props.put(CommonClientConfigs.SECURITY_PROTOCOL_CONFIG, "SASL_PLAINTEXT");
                    // Specify the SCRAM mechanism.
                    props.put(SaslConfigs.SASL_MECHANISM, "SCRAM-SHA-256");
            
                    // Specify the session timeout period. If the consumer does not return a heartbeat before the session times out, the broker determines that the consumer is not alive. Then the broker removes the consumer from the consumer group and triggers rebalancing. The default value is 30s.
                    props.put(ConsumerConfig.SESSION_TIMEOUT_MS_CONFIG, 30000);
                    // The maximum number of messages that can be polled at a time.
                    // Do not set the number to an excessively large value. If excessive messages are polled but fail to be completely consumed before the next poll starts, load balancing is triggered, which causes freezing.
                    props.put(ConsumerConfig.MAX_POLL_RECORDS_CONFIG, 30);
                    // Specify the method for deserializing messages.
                    props.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, "org.apache.kafka.common.serialization.StringDeserializer");
                    props.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, "org.apache.kafka.common.serialization.StringDeserializer");
                    // The consumer group of the current consumer instance. Enter the consumer group you have created in the console.
                    // The consumer instances that belong to the same consumer group. These instances consume messages in load balancing mode.
                    props.put(ConsumerConfig.GROUP_ID_CONFIG, kafkaProperties.getProperty("group.id"));
            
                    int consumerNum = 2;
                    Thread[] consumerThreads = new Thread[consumerNum];
                    for (int i = 0; i < consumerNum; i++) {
                        KafkaConsumer<String, String> consumer = new KafkaConsumer<String, String>(props);
            
                        List<String> subscribedTopics = new ArrayList<String>();
                        subscribedTopics.add(kafkaProperties.getProperty("topic"));
                        consumer.subscribe(subscribedTopics);
            
                        KafkaConsumerRunner kafkaConsumerRunner = new KafkaConsumerRunner(consumer);
                        consumerThreads[i] = new Thread(kafkaConsumerRunner);
                    }
            
                    for (int i = 0; i < consumerNum; i++) {
                        consumerThreads[i].start();
                    }
            
                    for (int i = 0; i < consumerNum; i++) {
                        consumerThreads[i].join();
                    }
                }
            
                static class KafkaConsumerRunner implements Runnable {
                    private final AtomicBoolean closed = new AtomicBoolean(false);
                    private final KafkaConsumer consumer;
            
                    KafkaConsumerRunner(KafkaConsumer consumer) {
                        this.consumer = consumer;
                    }
            
                    @Override
                    public void run() {
                        try {
                            while (! closed.get()) {
                                try {
                                    ConsumerRecords<String, String> records = consumer.poll(1000);
                                    // The messages must be completely consumed before the next polling cycle starts. The total duration cannot exceed the timeout interval specified by SESSION_TIMEOUT_MS_CONFIG.
                                    for (ConsumerRecord<String, String> record : records) {
                                        System.out.println(String.format("Thread:%s Consume partition:%d offset:%d", Thread.currentThread().getName(), record.partition(), record.offset()));
                                    }
                                } catch (Exception e) {
                                    try {
                                        Thread.sleep(1000);
                                    } catch (Throwable ignore) {
            
                                    }
                                    e.printStackTrace();
                                }
                            }
                        } catch (WakeupException e) {
                            // Ignore exceptions if it is disabled.
                            if (! closed.get()) {
                                throw e;
                            }
                        } finally {
                            consumer.close();
                        }
                    }
            
                    // The shutdown hook that can be called by another thread.
                    public void shutdown() {
                        closed.set(true);
                        consumer.wakeup();
                    }
                }
            }
            ```

        2.  Compile and run KafkaMultiConsumerDemo.java.

