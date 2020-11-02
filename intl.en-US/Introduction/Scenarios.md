# Scenarios

This topic describes typical scenarios of Message Queue for Apache Kafka, including website action tracking, log aggregation, stream computing, and data transfer hub.

## Website activity tracking

Successful website operation requires analysis of user actions on the website. You can use the publish/subscribe pattern of Message Queue for Apache Kafka to collect user action data on a website in real time, publish messages to different topics based on the type of business data, and then use message streams generated based on the real-time delivery of subscribed messages for real-time processing or real-time monitoring, or load the message streams to offline data warehouse systems such as Hadoop and MaxCompute for offline processing. The user action data of a website includes sign-in, logon, recharge, payment, and purchase.

![web tracking](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1667824061/p98246.jpg)

Message Queue for Apache Kafka has the following advantages in terms of website activity tracking:

-   High throughput: High throughput is required to support the large amount of user action information on the website.
-   Scalability: Website activity causes a sharp increase in user action data, and the number of brokers can be scaled out on demand.
-   Big data analysis: Message Queue for Apache Kafka can connect to real-time stream computing engines such as Storm and Spark, as well as offline data warehouse systems such as Hadoop.

## Log aggregation

Many platforms, such as Taobao and Tmall, generate a large number of logs every day, usually streaming data, such as page views and queries. Compared with log-centered systems, such as Scribe and Flume, Message Queue for Apache Kafka has higher performance and can achieve stronger data persistence and shorter end-to-end response time. This feature makes Message Queue for Apache Kafka suitable as a log collection center. In Message Queue for Apache Kafka, file details are ignored, and logs of multiple hosts or applications are abstracted as log or event message streams and then asynchronously sent to the Message Queue for Apache Kafka cluster, greatly reducing the response time. The Message Queue for Apache Kafka client submits and compresses messages in batches, without increasing the performance overhead of the producer. Consumers can use offline warehouse systems such as Hadoop and MaxCompute and real-time online analysis systems such as Storm and Spark to perform statistical analysis on logs.

![Log aggregation](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1667824061/p98252.jpg)

Message Queue for Apache Kafka has the following advantages for data aggregation:

-   System decoupling: Message Queue for Apache Kafka serves as a bridge between an application system and an analysis system and decouples the two systems.
-   High scalability: Message Queue for Apache Kafka is scalable. It allows rapid scale-out by adding nodes when the data volume increases.
-   Online and offline analysis systems: Message Queue for Apache Kafka supports real-time online analysis systems and offline analysis systems such as Hadoop.

## Stream computing

In many fields, such as stock market trend analysis, meteorological data monitoring and management, and website user action analysis, due to the huge amount of data generated in real time, it is difficult to collect and store all the data in the database before processing it. Therefore, traditional data processing architectures cannot meet user needs. Different from traditional architectures, Message Queue for Apache Kafka and stream computing engines such as Storm, Samza, and Spark can efficiently solve the preceding problems. The stream computing model captures and processes data in real time during data flow, computes and analyzes the data based on service requirements, and then saves or distributes the results to relevant components.

![Stream computing](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1667824061/p98254.jpg)

Message Queue for Apache Kafka has the following advantages for stream computing:

-   System decoupling: Message Queue for Apache Kafka serves as a bridge between an application system and an analysis system and decouples the two systems.
-   High scalability: Message Queue for Apache Kafka is highly scalable to deal with the huge amount of data generated in real time.
-   Connection to stream computing engines: Message Queue for Apache Kafka can connect to open-source Storm, Samza, and Spark, and Alibaba Cloud services such as E-MapReduce, Blink, and Realtime Compute.

## Data transfer hub

Over the past 10 years, dedicated systems such as key-value storage \(HBase\), search \(Elasticsearch\), stream processing \(Storm, Spark, and Samza\), and time series database \(OpenTSDB\) have emerged. These systems are designed for a single goal, and their simplicity makes it easier and more cost-effective to build distributed systems on commercial hardware. In most cases, the same dataset needs to be injected into multiple dedicated systems. For example, when application logs are used for offline analysis, searching for a single log is also required. However, it is impractical to construct independent workflows to collect data of each type and then import the data to their own dedicated systems. In this case, you can use Message Queue for Apache Kafka as a data transfer hub to import the same data record to different dedicated systems.

![Data transfer hub](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1667824061/p98253.jpg)

Message Queue for Apache Kafka has the following advantages when it serves as a data transfer hub:

-   High-capacity storage: Message Queue for Apache Kafka can store a large amount of data on commercial hardware and implement a horizontally scalable distributed system.
-   One-to-many consumption model: Based on the publish/subscribe pattern, the same dataset can be consumed multiple times.
-   Real-time and batch processing: Message Queue for Apache Kafka supports local data persistence and page cache, and transmits messages to consumers for real-time and batch processing at the same time without performance loss.

