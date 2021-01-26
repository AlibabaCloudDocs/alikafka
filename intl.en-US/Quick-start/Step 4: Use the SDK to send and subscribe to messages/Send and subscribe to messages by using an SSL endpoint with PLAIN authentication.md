---
keyword: [apache kafka, kafka, internet, send and subscribe to messages, 9093]
---

# Send and subscribe to messages by using an SSL endpoint with PLAIN authentication

This topic describes how to use Message Queue for Apache Kafka SDK to access the Secure Sockets Layer \(SSL\) endpoint of a Message Queue for Apache Kafka instance over the Internet and use the PLAIN mechanism to send and subscribe to messages. SDK for Java is used in the example.

-   [Step 3: Create resources](/intl.en-US/Quick-start/Step 3: Create resources.md)
-   JDK 1.8 or later is installed. For more information, see [Java SE Downloads](https://www.oracle.com/java/technologies/javase-downloads.html).
-   Maven 2.5 or later is installed. For more information, see [Download Apache Maven](http://maven.apache.org/download.cgi#).

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

2.  [Download an SSL root certificate.](https://code.aliyun.com/alikafka/aliware-kafka-demos/raw/master/kafka-java-demo/vpc-ssl/src/main/resources/kafka.client.truststore.jks)

3.  Create a JAAS configuration file named kafka\_client\_jaas.conf.

    ```
    KafkaClient {
      org.apache.kafka.common.security.plain.PlainLoginModule required
      username="xxxx"
      password="xxxx";
    };                       
    ```

    **Note:**

    -   If access control list \(ACL\) is disabled for the Message Queue for Apache Kafka instance, you can obtain the default username and password on the **Instance Details** page in the Message Queue for Apache Kafka console.
    -   If ACL is enabled for the Message Queue for Apache Kafka instance, make sure that the Simple Authentication and Security Layer \(SASL\) user to be used is of the PLAIN type and that the user is authorized to send and subscribe to messages. For more information, see [Authorize SASL users](/intl.en-US/Access control/Authorize SASL users.md).
4.  Create a Message Queue for Apache Kafka configuration file named kafka.properties.

    ```
    ## The SSL endpoint, which is obtained from the Message Queue for Apache Kafka console.
    bootstrap.servers=xxxx
    ## The topic, which is created in the Message Queue for Apache Kafka console.
    topic=xxxx
    ## The consumer group, which is created in the Message Queue for Apache Kafka console.
    group.id=xxxx
    ## The SSL root certificate.
    ssl.truststore.location=/xxxx/kafka.client.truststore.jks
    ## The JAAS configuration file.
    java.security.auth.login.config=/xxxx/kafka_client_jaas.conf                       
    ```

5.  Create a configuration file loader named JavaKafkaConfigurer.java.

    ```
    import java.util.Properties;
    
    public class JavaKafkaConfigurer {
    
        private static Properties properties;
    
        public static void configureSasl() {
            // If you have used the -D parameter or another method to set the path, do not set it again in this section.
            if (null == System.getProperty("java.security.auth.login.config")) {
                // Replace XXX with your path.
                // Make sure that the path is readable by the file system. Do not compress the file into a JAR package.
                System.setProperty("java.security.auth.login.config", getKafkaProperties().getProperty("java.security.auth.login.config"));
            }
        }
    
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


## Send messages

1.  Create a message sender named KafkaProducerDemo.java.

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
            JavaKafkaConfigurer.configureSasl();
    
            // Load the kafka.properties file.
            Properties kafkaProperties =  JavaKafkaConfigurer.getKafkaProperties();
    
            Properties props = new Properties();
            // Set the endpoint. Obtain the endpoint of the corresponding topic in the Message Queue for Apache Kafka console.
            props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, kafkaProperties.getProperty("bootstrap.servers"));
            // Set the path of the SSL root certificate. Replace XXX with your path.
            // Do not compress this file into a JAR package.
            props.put(SslConfigs.SSL_TRUSTSTORE_LOCATION_CONFIG, kafkaProperties.getProperty("ssl.truststore.location"));
            // The password of the root certificate store. Use the default value.
            props.put(SslConfigs.SSL_TRUSTSTORE_PASSWORD_CONFIG, "KafkaOnsClient");
            // Specify the access protocol. Set the value to SASL_SSL.
            props.put(CommonClientConfigs.SECURITY_PROTOCOL_CONFIG, "SASL_SSL");
            // Specify the SASL authentication method. Use the default value.
            props.put(SaslConfigs.SASL_MECHANISM, "PLAIN");
            // Set the method for deserializing  Message Queue for Apache Kafka  messages.
            props.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, "org.apache.kafka.common.serialization.StringSerializer");
            props.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, "org.apache.kafka.common.serialization.StringSerializer");
            // Set the maximum time to wait for a request.
            props.put(ProducerConfig.MAX_BLOCK_MS_CONFIG, 30 * 1000);
            // Set the number of retries for the client.
            props.put(ProducerConfig.RETRIES_CONFIG, 5);
            // Set the interval between retries for the client.
            props.put(ProducerConfig.RECONNECT_BACKOFF_MS_CONFIG, 3000);
    
            // Set hostname verification to an empty value.
            props.put(SslConfigs.SSL_ENDPOINT_IDENTIFICATION_ALGORITHM_CONFIG, "");
    
            // Construct a thread-safe producer object. One producer object can serve one process.
            // To improve performance, you can construct more objects. We recommend that you construct no more than five objects.
            KafkaProducer<String, String> producer = new KafkaProducer<String, String>(props);
    
            // Construct a  Message Queue for Apache Kafka  message.
            String topic = kafkaProperties.getProperty("topic"); // The topic of the message. Enter the topic that you created in the Message Queue for Apache Kafka console.
            String value = "this is the message's value"; // The content of the message.
    
            try {
                // The operation to batch obtain future objects can increase speed, but the batch size cannot be too large.
                List<Future<RecordMetadata>> futures = new ArrayList<Future<RecordMetadata>>(128);
                for (int i =0; i < 100; i++) {
                    // Send the message and obtain a future object.
                    ProducerRecord<String, String> kafkaMessage =  new ProducerRecord<String, String>(topic, value + ": " + i);
                    Future<RecordMetadata> metadataFuture = producer.send(kafkaMessage);
                    futures.add(metadataFuture);
    
                }
                producer.flush();
                for (Future<RecordMetadata> future: futures) {
                    // Synchronize the future object.
                    try {
                        RecordMetadata recordMetadata = future.get();
                        System.out.println("Produce ok:" + recordMetadata.toString());
                    } catch (Throwable t) {
                        t.printStackTrace();
                    }
                }
            } catch (Exception e) {
                // If the client does not send data even after retries, catch and handle the error.
                System.out.println("error occurred");
                e.printStackTrace();
            }
        }
    }  
    ```

2.  Compile and run KafkaProducerDemo.java to send a message.


## Subscribe to messages

1.  You can subscribe to messages by using one of the following methods:

    -   Enable only one consumer to subscribe to messages
        1.  Create a single-consumer subscription program named KafkaConsumerDemo.java.

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
            import org.apache.kafka.common.config.SslConfigs;
            
            public class KafkaConsumerDemo {
            
                public static void main(String args[]) {
                    // Set the path of the JAAS configuration file.
                    JavaKafkaConfigurer.configureSasl();
            
                    // Load the kafka.properties file.
                    Properties kafkaProperties =  JavaKafkaConfigurer.getKafkaProperties();
            
                    Properties props = new Properties();
                    // Set the endpoint. Obtain the endpoint of the corresponding topic in the Message Queue for Apache Kafka console.
                    props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, kafkaProperties.getProperty("bootstrap.servers"));
                    // Set the path of the SSL root certificate. Replace XXX with your path.
                    // Do not compress this file into a JAR package.
                    props.put(SslConfigs.SSL_TRUSTSTORE_LOCATION_CONFIG, kafkaProperties.getProperty("ssl.truststore.location"));
                    // Specify the password of the root certificate store. Use the default value.
                    props.put(SslConfigs.SSL_TRUSTSTORE_PASSWORD_CONFIG, "KafkaOnsClient");
                    // Specify the access protocol. Set the value to SASL_SSL.
                    props.put(CommonClientConfigs.SECURITY_PROTOCOL_CONFIG, "SASL_SSL");
                    // Specify the SASL authentication method. Use the default value.
                    props.put(SaslConfigs.SASL_MECHANISM, "PLAIN");
                    // Set the maximum interval between two polling cycles.
                    // The default value is 30s. If the consumer does not return a heartbeat message within the interval, the broker determines that the consumer is not alive. The broker removes the consumer from the consumer group and triggers rebalancing.
                    props.put(ConsumerConfig.SESSION_TIMEOUT_MS_CONFIG, 30000);
                    // Specify the size of a poll. This parameter has a significant effect on Internet data.
                    props.put(ConsumerConfig.MAX_PARTITION_FETCH_BYTES_CONFIG, 32000);
                    props.put(ConsumerConfig.FETCH_MAX_BYTES_CONFIG, 32000);
                    // Set the maximum number of messages that can be polled at a time.
                    // Do not set this parameter to an excessively large value. If polled messages are not all consumed before the next poll starts, load balancing is triggered and performance may deteriorate.
                    props.put(ConsumerConfig.MAX_POLL_RECORDS_CONFIG, 30);
                    // Set the method for deserializing messages.
                    props.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, "org.apache.kafka.common.serialization.StringDeserializer");
                    props.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, "org.apache.kafka.common.serialization.StringDeserializer");
                    // Set the consumer group of the current consumer instance. You must create the consumer group in the Message Queue for Apache Kafka console.
                    // The instances in a consumer group consume messages in load balancing mode.
                    props.put(ConsumerConfig.GROUP_ID_CONFIG, kafkaProperties.getProperty("group.id"));
                    // Set hostname verification to an empty value.
                    props.put(SslConfigs.SSL_ENDPOINT_IDENTIFICATION_ALGORITHM_CONFIG, "");
            
                    // Construct a consumer object, which is a consumer instance.
                    KafkaConsumer<String, String> consumer = new org.apache.kafka.clients.consumer.KafkaConsumer<String, String>(props);
                    // Set one or more topics to which the consumer group subscribes.
                    // We recommend that you configure consumer instances with the same GROUP_ID_CONFIG value to subscribe to the same topics.
                    List<String> subscribedTopics =  new ArrayList<String>();
                    // If you want to subscribe to multiple topics, add the topics here.
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

        2.  Compile and run KafkaConsumerDemo.java to consume messages.
    -   Enable multiple consumers to subscribe to messages
        1.  Create a multi-consumer subscription program named KafkaMultiConsumerDemo.java.

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
            import org.apache.kafka.common.config.SslConfigs;
            import org.apache.kafka.common.errors.WakeupException;
            
            /**
             * This tutorial shows you how to enable multiple consumers to simultaneously consume messages of the same topic in one process.
             * Make sure that the total number of consumers in the environment does not exceed the number of partitions of the topics to which the consumers subscribed.
             */
            public class KafkaMultiConsumerDemo {
            
                public static void main(String args[]) throws InterruptedException {
                    // Set the path of the JAAS configuration file.
                    JavaKafkaConfigurer.configureSasl();
            
                    // Load the kafka.properties file.
                    Properties kafkaProperties = JavaKafkaConfigurer.getKafkaProperties();
            
                    Properties props = new Properties();
                    // Set the endpoint. Obtain the endpoint of the corresponding topic in the Message Queue for Apache Kafka console.
                    props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, kafkaProperties.getProperty("bootstrap.servers"));
                    // Set the path of the SSL root certificate. Replace XXX with your path.
                    // Do not compress this file into a JAR package.
                    props.put(SslConfigs.SSL_TRUSTSTORE_LOCATION_CONFIG, kafkaProperties.getProperty("ssl.truststore.location"));
                    // Specify the password of the root certificate store. Use the default value.
                    props.put(SslConfigs.SSL_TRUSTSTORE_PASSWORD_CONFIG, "KafkaOnsClient");
                    // Specify the access protocol. Set the value to SASL_SSL.
                    props.put(CommonClientConfigs.SECURITY_PROTOCOL_CONFIG, "SASL_SSL");
                    // Specify the SASL authentication method. Use the default value.
                    props.put(SaslConfigs.SASL_MECHANISM, "PLAIN");
                    // Set the maximum interval between two polling cycles.
                    // The default value is 30s. If the consumer does not return a heartbeat message within the interval, the broker determines that the consumer is not alive. The broker removes the consumer from the consumer group and triggers rebalancing.
                    props.put(ConsumerConfig.SESSION_TIMEOUT_MS_CONFIG, 30000);
                    // Set the maximum number of messages that can be polled at a time.
                    // Do not set this parameter to an excessively large value. If polled messages are not all consumed before the next poll starts, load balancing is triggered and performance may deteriorate.
                    props.put(ConsumerConfig.MAX_POLL_RECORDS_CONFIG, 30);
                    // Set the method for deserializing messages.
                    props.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, "org.apache.kafka.common.serialization.StringDeserializer");
                    props.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, "org.apache.kafka.common.serialization.StringDeserializer");
                    // Set the consumer group of the current consumer instance. You must create the consumer group in the Message Queue for Apache Kafka console.
                    // The instances in a consumer group consume messages in load balancing mode.
                    props.put(ConsumerConfig.GROUP_ID_CONFIG, kafkaProperties.getProperty("group.id"));
                    // Construct a consumer object, which is a consumer instance.
            
                    // Set hostname verification to an empty value.
                    props.put(SslConfigs.SSL_ENDPOINT_IDENTIFICATION_ALGORITHM_CONFIG, "");
            
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
                                    // All messages must be consumed before the next polling cycle starts. The total duration cannot exceed the timeout interval specified by SESSION_TIMEOUT_MS_CONFIG.
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
                            // If the consumer is shut down, ignore exceptions.
                            if (! closed.get()) {
                                throw e;
                            }
                        } finally {
                            consumer.close();
                        }
                    }
            
                    // Implement a shutdown hook that can be called by another thread.
                    public void shutdown() {
                        closed.set(true);
                        consumer.wakeup();
                    }
                }
            }
            ```

        2.  Compile and run KafkaMultiConsumerDemo.java to consume messages.

## Use SDKs for other languages

For information about how to use SDKs for other languages, see [SDK overview](/intl.en-US/SDK reference/SDK overview.md).

