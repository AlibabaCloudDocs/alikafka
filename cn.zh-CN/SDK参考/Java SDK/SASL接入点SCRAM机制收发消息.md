---
keyword: [java, vpc, kafka, 收发消息, scram]
---

# SASL接入点SCRAM机制收发消息

本文介绍Java客户端如何在VPC环境下通过SASL接入点接入消息队列Kafka版并使用SCRAM机制收发消息。

-   [安装1.8或以上版本JDK](https://www.oracle.com/java/technologies/javase-downloads.html)
-   [安装2.5或以上版本Maven](http://maven.apache.org/download.cgi#)
-   [SASL用户授权](/cn.zh-CN/权限控制/SASL用户授权.md)

## 安装Java依赖库

1.  在pom.xml中添加以下依赖。

    ```
    <dependency>
           <groupId>org.apache.kafka</groupId>
           <artifactId>kafka-clients</artifactId>
           <version>0.10.2.2</version>
    </dependency> 
    ```

    **说明：** 建议您保持服务端和客户端版本一致，即保持客户端库版本和消息队列Kafka版实例的大版本一致。您可以消息队列Kafka版控制台的**实例详情**页面获取消息队列Kafka版实例的大版本。


## 准备配置

1.  创建Log4j配置文件log4j.properties。

    ```
    # Licensed to the Apache Software Foundation (ASF) under one or more
    # contributor license agreements.  See the NOTICE file distributed with
    # this work for additional information regarding copyright ownership.
    # The ASF licenses this file to You under the Apache License, Version 2.0
    # (the "License"); you may not use this file except in compliance with
    # the License.  You may obtain a copy of the License at
    #
    #    http://www.apache.org/licenses/LICENSE-2.0
    #
    # Unless required by applicable law or agreed to in writing, software
    # distributed under the License is distributed on an "AS IS" BASIS,
    # WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    # See the License for the specific language governing permissions and
    # limitations under the License.
    
    log4j.rootLogger=INFO, STDOUT
    
    log4j.appender.STDOUT=org.apache.log4j.ConsoleAppender
    log4j.appender.STDOUT.layout=org.apache.log4j.PatternLayout
    log4j.appender.STDOUT.layout.ConversionPattern=[%d] %p %m (%c)%n
    ```

2.  创建JAAS配置文件kafka\_client\_jaas\_scram.conf。

    ```
    KafkaClient {
      org.apache.kafka.common.security.scram.ScramLoginModule required
      username="xxxx"
      password="xxxx";
    };
    ```

3.  创建Kafka配置文件kafka.properties。

    ```
    ##SASL接入点，通过控制台获取。
    bootstrap.servers=xxxx
    ##Topic，通过控制台创建。
    topic=xxxx
    ##Consumer Group，通过控制台创建。
    group.id=xxxx
    ##JAAS配置文件。
    java.security.auth.login.config.scram=/xxxx/kafka_client_jaas_scram.conf
    ```

4.  创建配置文件加载程序JavaKafkaConfigurer.java。

    ```
    import java.util.Properties;
    
    public class JavaKafkaConfigurer {
    
        private static Properties properties;
    
        public static void configureSaslScram() {
            //如果用-D或者其它方式设置过，这里不再设置。
            if (null == System.getProperty("java.security.auth.login.config")) {
                //请注意将XXX修改为自己的路径。
                //这个路径必须是一个文件系统可读的路径，不能被打包到JAR中。
                System.setProperty("java.security.auth.login.config", getKafkaProperties().getProperty("java.security.auth.login.config.scram"));
            }
        }
    
        public synchronized static Properties getKafkaProperties() {
            if (null != properties) {
                return properties;
            }
            //获取配置文件kafka.properties的内容。
            Properties kafkaProperties = new Properties();
            try {
                kafkaProperties.load(KafkaProducerDemo.class.getClassLoader().getResourceAsStream("kafka.properties"));
            } catch (Exception e) {
                //没加载到文件，程序要考虑退出。
                e.printStackTrace();
            }
            properties = kafkaProperties;
            return kafkaProperties;
        }
    }
    ```


## 发送消息

1.  创建发送消息程序KafkaProducerDemo.java。

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
            //设置JAAS配置文件的路径。
            JavaKafkaConfigurer.configureSaslScram();
    
            //加载kafka.properties。
            Properties kafkaProperties =  JavaKafkaConfigurer.getKafkaProperties();
    
            Properties props = new Properties();
            //设置接入点，请通过控制台获取对应Topic的接入点。
            props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, kafkaProperties.getProperty("bootstrap.servers"));
    
            //接入协议。
            props.put(CommonClientConfigs.SECURITY_PROTOCOL_CONFIG, "SASL_PLAINTEXT");
            //SCRAM方式。
            props.put(SaslConfigs.SASL_MECHANISM, "SCRAM-SHA-256");     
    
            //Kafka消息的序列化方式。
            props.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, "org.apache.kafka.common.serialization.StringSerializer");
            props.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, "org.apache.kafka.common.serialization.StringSerializer");
            //请求的最长等待时间。
            props.put(ProducerConfig.MAX_BLOCK_MS_CONFIG, 30 * 1000);
            //设置客户端内部重试次数。
            props.put(ProducerConfig.RETRIES_CONFIG, 5);
            //设置客户端内部重试间隔。
            props.put(ProducerConfig.RECONNECT_BACKOFF_MS_CONFIG, 3000);
            //构造Producer对象，注意，该对象是线程安全的，一般来说，一个进程内一个Producer对象即可。
            //如果想提高性能，可以多构造几个对象，但不要太多，最好不要超过5个。
            KafkaProducer<String, String> producer = new KafkaProducer<String, String>(props);
    
            //构造一个Kafka消息。
            String topic = kafkaProperties.getProperty("topic"); //消息所属的Topic，请在控制台申请之后，填写在这里。
            String value = "this is the message's value"; //消息的内容。
    
            try {
                //批量获取Future对象可以加快速度。但注意，批量不要太大。
                List<Future<RecordMetadata>> futures = new ArrayList<Future<RecordMetadata>>(128);
                for (int i =0; i < 100; i++) {
                    //发送消息，并获得一个Future对象。
                    ProducerRecord<String, String> kafkaMessage =  new ProducerRecord<String, String>(topic, value + ": " + i);
                    Future<RecordMetadata> metadataFuture = producer.send(kafkaMessage);
                    futures.add(metadataFuture);
    
                }
                producer.flush();
                for (Future<RecordMetadata> future: futures) {
                    //同步获得Future对象的结果。
                    try {
                        RecordMetadata recordMetadata = future.get();
                        System.out.println("Produce ok:" + recordMetadata.toString());
                    } catch (Throwable t) {
                        t.printStackTrace();
                    }
                }
            } catch (Exception e) {
                //客户端内部重试之后，仍然发送失败，业务要应对此类错误。
                System.out.println("error occurred");
                e.printStackTrace();
            }
        }
    }
    ```

2.  编译并运行KafkaProducerDemo.java发送消息。


## 订阅消息

1.  选择以下任意一种方式订阅消息。

    -   单Consumer订阅消息。
        1.  创建单Consumer订阅消息程序KafkaConsumerDemo.java。

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
                    //设置JAAS配置文件的路径。
                    JavaKafkaConfigurer.configureSaslScram();
            
                    //加载kafka.properties。
                    Properties kafkaProperties =  JavaKafkaConfigurer.getKafkaProperties();
            
                    Properties props = new Properties();
                    //设置接入点，请通过控制台获取对应Topic的接入点。
                    props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, kafkaProperties.getProperty("bootstrap.servers"));
            
                    //接入协议。
                    props.put(CommonClientConfigs.SECURITY_PROTOCOL_CONFIG, "SASL_PLAINTEXT");
                    //SCRAM方式。
                    props.put(SaslConfigs.SASL_MECHANISM, "SCRAM-SHA-256");
            
                    //可更加实际拉去数据和客户的版本等设置此值，默认30s。
                    props.put(ConsumerConfig.SESSION_TIMEOUT_MS_CONFIG, 30000);
                    //每次Poll的最大数量。
                    //注意该值不要改得太大，如果Poll太多数据，而不能在下次Poll之前消费完，则会触发一次负载均衡，产生卡顿。
                    props.put(ConsumerConfig.MAX_POLL_RECORDS_CONFIG, 30);
                    //消息的反序列化方式。
                    props.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, "org.apache.kafka.common.serialization.StringDeserializer");
                    props.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, "org.apache.kafka.common.serialization.StringDeserializer");
                    //当前消费实例所属的消费组，请在控制台申请之后填写。
                    //属于同一个组的消费实例，会负载消费消息。
                    props.put(ConsumerConfig.GROUP_ID_CONFIG, kafkaProperties.getProperty("group.id"));
                    //构造消息对象，也即生成一个消费实例。
                    KafkaConsumer<String, String> consumer = new org.apache.kafka.clients.consumer.KafkaConsumer<String, String>(props);
                    //设置消费组订阅的Topic，可以订阅多个。
                    //如果GROUP_ID_CONFIG是一样，则订阅的Topic也建议设置成一样。
                    List<String> subscribedTopics =  new ArrayList<String>();
                    //如果需要订阅多个Topic，则在这里添加进去即可。
                    //每个Topic需要先在控制台进行创建。
                    String topicStr = kafkaProperties.getProperty("topic");
                    String[] topics = topicStr.split(",");
                    for (String topic: topics) {
                        subscribedTopics.add(topic.trim());
                    }
                    consumer.subscribe(subscribedTopics);
            
                    //循环消费消息。
                    while (true){
                        try {
                            ConsumerRecords<String, String> records = consumer.poll(1000);
                            //必须在下次Poll之前消费完这些数据, 且总耗时不得超过SESSION_TIMEOUT_MS_CONFIG。
                            //建议开一个单独的线程池来消费消息，然后异步返回结果。
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

        2.  编译并运行KafkaConsumerDemo.java消费消息。
    -   多Consumer订阅消息。
        1.  创建多Consumer订阅消息程序KafkaMultiConsumerDemo.java。

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
             * 本教程演示如何在一个进程内开启多个Consumer同时消费Topic。
             * 注意全局Consumer数量不要超过订阅的Topic总分区数。
             */
            public class KafkaMultiConsumerDemo {
            
                public static void main(String args[]) throws InterruptedException {
                    //设置JAAS配置文件的路径。
                    JavaKafkaConfigurer.configureSaslScram();
            
                    //加载kafka.properties。
                    Properties kafkaProperties = JavaKafkaConfigurer.getKafkaProperties();
            
                    Properties props = new Properties();
                    //设置接入点，请通过控制台获取对应Topic的接入点。
                    props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, kafkaProperties.getProperty("bootstrap.servers"));
            
                    //接入协议。
                    props.put(CommonClientConfigs.SECURITY_PROTOCOL_CONFIG, "SASL_PLAINTEXT");
                    //SCRAM方式。
                    props.put(SaslConfigs.SASL_MECHANISM, "SCRAM-SHA-256");
            
                    //可更加实际拉去数据和客户的版本等设置此值，默认30s。
                    props.put(ConsumerConfig.SESSION_TIMEOUT_MS_CONFIG, 30000);
                    //每次Poll的最大数量。
                    //注意该值不要改得太大，如果Poll太多数据，而不能在下次Poll之前消费完，则会触发一次负载均衡，产生卡顿。
                    props.put(ConsumerConfig.MAX_POLL_RECORDS_CONFIG, 30);
                    //消息的反序列化方式。
                    props.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, "org.apache.kafka.common.serialization.StringDeserializer");
                    props.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, "org.apache.kafka.common.serialization.StringDeserializer");
                    //当前消费实例所属的消费组，请在控制台申请之后填写。
                    //属于同一个组的消费实例，会负载消费消息。
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
                            while (!closed.get()) {
                                try {
                                    ConsumerRecords<String, String> records = consumer.poll(1000);
                                    //必须在下次Poll之前消费完这些数据, 且总耗时不得超过SESSION_TIMEOUT_MS_CONFIG。
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
                            //如果关闭忽略异常。
                            if (!closed.get()) {
                                throw e;
                            }
                        } finally {
                            consumer.close();
                        }
                    }
            
                    //可以被另一个线程调用的关闭Hook。
                    public void shutdown() {
                        closed.set(true);
                        consumer.wakeup();
                    }
                }
            }
            ```

        2.  编译并运行KafkaMultiConsumerDemo.java消费消息。

