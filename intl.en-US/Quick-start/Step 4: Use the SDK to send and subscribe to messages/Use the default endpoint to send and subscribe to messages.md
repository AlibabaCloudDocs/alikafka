---
keyword: [kafka, message queue for apache kafka, vpc, aend and subscribe to messages, 9092]
---

# Use the default endpoint to send and subscribe to messages

This topic describes how a Java client uses SDK for Java to connect to the default endpoint of Message Queue for Apache Kafka and send and subscribe to messages in a virtual private cloud \(VPC\).

-   JDK 1.8 or later is installed. For more information, see [Download JDK](https://www.oracle.com/java/technologies/javase-downloads.html).
-   Maven 2.5 or later is installed. For more information, see [Download Maven](http://maven.apache.org/download.cgi#).

## Install Java dependencies

1.  Add the following dependencies to the pom.xml file:

    ```
    <dependency>
        <groupId>org.apache.kafka</groupId>
        <artifactId>kafka-clients</artifactId>
        <version>0.10.2.2</version>
    </dependency>
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-log4j12</artifactId>
        <version>1.7.6</version>
    </dependency>
    ```

    **Note:** We recommend that you keep the version of the client consistent with that of the broker. That is, the client library version must be consistent with the major version of the Message Queue for Apache Kafka instance. You can obtain the major version of the Message Queue for Apache Kafka instance on the **Instance Details** page in the Message Queue for Apache Kafka console.


## Prepare configurations

1.  Create a Log4j configuration file log4j.properties.

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

2.  Create a Kafka configuration file.

    ```
    ## Configure the endpoint. Set it to the default endpoint displayed on the Instance Details page in the Message Queue for Apache Kafka console.
    bootstrap.servers=xxxxxxxxxxxxxxxxxxxxx
    ## Configure the topic. You can create the topic in the Message Queue for Apache Kafka console.
    topic=alikafka-topic-demo
    ## Configure the consumer group. You can create the consumer group in the Message Queue for Apache Kafka console.
    group.id=CID-consumer-group-demo
    ```

3.  Create a profile loader named JavaKafkaConfigurer.java.

    ```
    public class JavaKafkaConfigurer {
        private static Properties properties;
        public synchronized static Properties getKafkaProperties() {
           if (null ! = properties) {
               return properties;
           }
           // Obtain the content of the configuration file kafka.properties.
           Properties kafkaProperties = new Properties();
           try {
               kafkaProperties.load(KafkaProducerDemo.class.getClassLoader().getResourceAsStream("kafka.properties"));
           } catch (Exception e) {
               // Exit the program if the file fails to be loaded.
               e.printStackTrace();
           }
           properties = kafkaProperties;
           return kafkaProperties;
        }
    }
    ```


## SDKs for multiple languages

For more information about how to use SDKs for other languages, see [Overview](/intl.en-US/SDK reference/Overview.md).

