---
keyword: [java, vpc, kafka, send and receive messages, plain]
---

# Use the PLAIN mechanism to send and subscribe to messages through an SASL endpoint

This topic describes how a Java client connects to Message Queue for Apache Kafka through an SASL endpoint in a virtual private cloud \(VPC\) and uses the PLAIN mechanism to send and subscribe to messages.

-   [Install JDK 1.8 or later](https://www.oracle.com/java/technologies/javase-downloads.html)
-   [Install Maven 2.5 or later](http://maven.apache.org/download.cgi#)
-   [Authorize SASL users](/intl.en-US/Access control/Authorize SASL users.md)

## Install Java dependencies

1.  In the pom.xmlAnd add the following dependencies.

    ```
    
            <dependency> <groupId>org.apache.kafka <artifactId>kafka-clients</artifactId> <version>0.10.2.2</version> </dependency> <dependency> <groupId>org.slf4j <artifactId>slf4j-log4j12</artifactId> <version>1.7.6</version> </dependency> 
          
    ```

    **Note:** We recommend that you keep the version of the provider and client consistent, that is, keep the client library version consistent with Message Queue for Apache KafkaThe major version must be the same for all instances. You can go to the Message Queue for Apache KafkaThe console's **Instance Details page**Page acquisition Message Queue for Apache KafkaThe major version of the instance.


## Prepare configurations

1.  Create a Log4j configuration file log4j.properties.

    ```
    
            # Licensed to the Apache Software Foundation (ASF) under one or more # contributor license agreements. See the NOTICE file distributed with # this work for additional information regarding copyright ownership. # The ASF licenses this file to You under the Apache License, Version 2.0 # (the "License"); you may not use this file except in compliance with # the License. You may obtain a copy of the License at # # http://www.apache.org/licenses/LICENSE-2.0 # # Unless required by applicable law or agreed to in writing, software # distributed under the License is distributed on an "AS IS" BASIS, # WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. # See the License for the specific language governing permissions and # limitations under the License. log4j.rootLogger=INFO, STDOUT log4j.appender.STDOUT=org.apache.log4j.ConsoleAppender log4j.appender.STDOUT.layout=org.apache.log4j.PatternLayout log4j.appender.STDOUT.layout.ConversionPattern=[%d] %p %m (%c)%n 
          
    ```

2.  Create a JAAS configuration file kafka\_client\_jaas\_plain.conf.

    ```
    KafkaClient {
      org.apache.kafka.common.security.plain.PlainLoginModule required
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
    java.security.auth.login.config.plain=/xxxx/kafka_client_jaas_plain.conf
    ```

4.  Create a profile loader JavaKafkaConfigurer.java.

    ```
    import java.util.Properties;
    
    public class JavaKafkaConfigurer {
    
        private static Properties properties;
    
        public static void configureSaslPlain() {
            // If you have set the path by using methods such as -D, do not set it again.
            if (null == System.getProperty("java.security.auth.login.config")) {
                // Replace XXX with your own path.
                // Ensure that the path is readable by the file system. Do not compress the file into a JAR package.
                System.setProperty("java.security.auth.login.config", getKafkaProperties().getProperty("java.security.auth.login.config.plain"));
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
            JavaKafkaConfigurer.configureSaslPlain();
    
            // Load kafka.properties.
            Properties kafkaProperties =  JavaKafkaConfigurer.getKafkaProperties();
    
            Properties props = new Properties();
            // Specify the endpoint. We recommend that you obtain the endpoint of the corresponding topic in the console.
            props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, kafkaProperties.getProperty("bootstrap.servers"));
    
            // Specify the access protocol.
            props.put(CommonClientConfigs.SECURITY_PROTOCOL_CONFIG, "SASL_PLAINTEXT");
            // Specify the PLAIN mechanism.
            props.put(SaslConfigs.SASL_MECHANISM, "PLAIN");     
    
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
                    JavaKafkaConfigurer.configureSaslPlain();
            
                    // Load kafka.properties.
                    Properties kafkaProperties =  JavaKafkaConfigurer.getKafkaProperties();
            
                    Properties props = new Properties();
                    // Specify the endpoint. We recommend that you obtain the endpoint of the corresponding topic in the console.
                    props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, kafkaProperties.getProperty("bootstrap.servers"));
            
                    // Specify the access protocol.
                    props.put(CommonClientConfigs.SECURITY_PROTOCOL_CONFIG, "SASL_PLAINTEXT");
                    // Specify the PLAIN mechanism.
                    props.put(SaslConfigs.SASL_MECHANISM, "PLAIN");
            
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
                    JavaKafkaConfigurer.configureSaslPlain();
            
                    // Load kafka.properties.
                    Properties kafkaProperties = JavaKafkaConfigurer.getKafkaProperties();
            
                    Properties props = new Properties();
                    // Specify the endpoint. We recommend that you obtain the endpoint of the corresponding topic in the console.
                    props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, kafkaProperties.getProperty("bootstrap.servers"));
            
                    // Specify the access protocol.
                    props.put(CommonClientConfigs.SECURITY_PROTOCOL_CONFIG, "SASL_PLAINTEXT");
                    // Specify the PLAIN mechanism.
                    props.put(SaslConfigs.SASL_MECHANISM, "PLAIN");
            
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

