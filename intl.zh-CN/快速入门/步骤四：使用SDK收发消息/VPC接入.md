---
keyword: [kafka, apache kafka, vpc, 收发消息]
---

# VPC接入

本文以Java SDK为例说明如何在VPC环境下通过SDK向消息队列Kafka版收发消息。

## 步骤一：准备配置

1.  添加Maven依赖。

    需添加的Maven依赖示例如下：

    ```
    <dependency>
           <groupId>org.apache.kafka</groupId>
           <artifactId>kafka-clients</artifactId>
           <version>0.10.2.2</version>
    </dependency>                   
    ```

    **说明：** 建议您保持服务端和客户端版本一致。例如，您的服务端版本为0.10.2.2，则建议客户端版本也为0.10.2.2。

2.  配置客户端。

    1.  准备配置文件kafka.properties。

        kafka.properties示例如下：

        ```
        ## 配置接入点，即控制台的实例详情页面显示的默认接入点。
        bootstrap.servers=xxxxxxxxxxxxxxxxxxxxx
        ## 配置Topic，可以在控制台上创建Topic。
        topic=alikafka-topic-demo
        ## 配置Consumer Group，可以在控制台创建Consumer Group。
        group.id=CID-consumer-group-demo
        ```

    2.  加载配置文件。

        ```
        public class JavaKafkaConfigurer {
            private static Properties properties;
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


## 步骤二：使用Java SDK发送消息

1.  示例代码如下：

    ```
    //加载kafka.properties。
    Properties kafkaProperties =  JavaKafkaConfigurer.getKafkaProperties();
    Properties props = new Properties();
    //设置接入点，即控制台的实例详情页面显示的“默认接入点”。
    props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, kafkaProperties.getProperty("bootstrap.servers"));
    //接入协议。
    props.put(CommonClientConfigs.SECURITY_PROTOCOL_CONFIG, "PLAINTEXT");
    //Kafka消息的序列化方式。
    props.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, "org.apache.kafka.common.serialization.StringSerializer");
    props.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, "org.apache.kafka.common.serialization.StringSerializer");
    //请求的最长等待时间。
    props.put(ProducerConfig.MAX_BLOCK_MS_CONFIG, 30 * 1000);
    //构造Producer对象，注意，该对象是线程安全的，一般来说，一个进程内一个Producer对象即可。
    //如果想提高性能，可以多构造几个对象，但不要太多，最好不要超过5个。
    KafkaProducer<String, String> producer = new KafkaProducer<String, String>(props);
    //构造一个Kafka消息。
    String topic = kafkaProperties.getProperty("topic"); //消息所属的Topic，请在控制台申请之后，填写在这里。
    String value = "this is the message's value"; //消息的内容。
    ProducerRecord<String, String>  kafkaMessage =  new ProducerRecord<String, String>(topic, value);
    try {
      //发送消息，并获得一个Future对象。
      Future<RecordMetadata> metadataFuture = producer.send(kafkaMessage);
      //同步获得Future对象的结果。
      RecordMetadata recordMetadata = metadataFuture.get();
      System.out.println("Produce ok:" + recordMetadata.toString());
    } catch (Exception e) {
      //要考虑重试。
      System.out.println("error occurred");
      e.printStackTrace();
    }
    ```


## 步骤三：使用Java SDK订阅消息

示例代码如下：

```
//加载kafka.properties。
Properties kafkaProperties =  JavaKafkaConfigurer.getKafkaProperties();
Properties props = new Properties();
//设置接入点，即控制台的实例详情页面显示的默认接入点。
props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, kafkaProperties.getProperty("bootstrap.servers"));
//两次poll之间的最大允许间隔。
//请不要改得太大，服务器会掐掉空闲连接，不要超过30000。
props.put(ConsumerConfig.SESSION_TIMEOUT_MS_CONFIG, 25000);
//每次poll的最大数量。
//注意该值不要改得太大，如果poll太多数据，而不能在下次poll之前消费完，则会触发一次负载均衡，产生卡顿。
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
//如果需要订阅多个Topic，则在这里add进去即可。
//每个Topic需要先在控制台进行创建。
subscribedTopics.add(kafkaProperties.getProperty("topic"));
consumer.subscribe(subscribedTopics);
//循环消费消息。
while (true){
    try {
        ConsumerRecords<String, String> records = consumer.poll(1000);
        //必须在下次poll之前消费完这些数据, 且总耗时不得超过SESSION_TIMEOUT_MS_CONFIG。
        //建议开一个单独的线程池来消费消息，然后异步返回结果。
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
        //参考常见报错。
        e.printStackTrace();
    }
}
```

## 多语言示例

本文以Java SDK为例，其他语言示例请参见[消息队列Kafka版Demo库](https://code.aliyun.com/alikafka/aliware-kafka-demos/tree/master)。

