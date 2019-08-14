# Java SDK 接入和使用说明 {#concept_68325_zh .concept}

本文介绍如何通过 Java SDK 接入消息队列 Kafka 并进行消息收发，您也可以直接参考[消息队列 Kafka Demo 库](https://github.com/AliwareMQ/aliware-kafka-demos)中的 Demo 和说明。

## 前提条件 {#section_u9e_ac2_iqv .section}

请确保已做好[接入准备](cn.zh-CN/开发指南/SDK 参考/接入准备.md#)。

## 添加 Maven 依赖 {#section_1yg_yur_t22 .section}

``` {#codeblock_mei_s5r_gbg .language-java}
//消息队列 Kafka 服务端版本是 0.10.0.0，建议客户端版本也是 0.10.0.0
<dependency>
    <groupId>org.apache.kafka</groupId>
    <artifactId>kafka-clients</artifactId>
    <version>0.10.0.0</version>
</dependency>
```

## 2. 使用 SDK {#section_2nq_id7_0xh .section}

1.  准备配置文件 kafka.properties，可参考[消息队列 Kafka Demo 库](https://github.com/AliwareMQ/aliware-kafka-demos)中的 Demo 和说明进行修改。

    ``` {#codeblock_m7c_wwg_3kp}
    ## 您在控制台获取的接入点
    bootstrap.servers=xxx.xxx.xxx.xxx:9092
    
    ## 您在控制台创建的 Topic
    topic=alikafka-topic-demo
    
    ## 您在控制台创建的 Consumer Group
    group.id=CID-consumer-group-demo
    
    					
    ```

2.  加载配置文件 `kafka.properties`。

    ``` {#codeblock_nm9_yeg_867}
    public class JavaKafkaConfigurer {
    
        private static Properties properties;
    
        public synchronized static Properties getKafkaProperties() {
            if (null != properties) {
                return properties;
            }
            //获取配置文件 kafka.properties 的内容
            Properties kafkaProperties = new Properties();
            try {
                kafkaProperties.load(KafkaProducerDemo.class.getClassLoader().getResourceAsStream("kafka.properties"));
            } catch (Exception e) {
                //没加载到文件，程序要考虑退出
                e.printStackTrace();
            }
            properties = kafkaProperties;
            return kafkaProperties;
        }
    }
    					
    ```

3.  发送消息。

    ``` {#codeblock_y4p_ppa_shn}
    public class KafkaProducerDemo {
    
        public static void main(String args[]) {
    
            //加载 kafka.properties
            Properties kafkaProperties =  JavaKafkaConfigurer.getKafkaProperties();
    
            Properties props = new Properties();
            //设置接入点，请通过控制台获取对应 Topic 的接入点
            props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, kafkaProperties.getProperty("bootstrap.servers"));
            //消息队列 Kafka 消息的序列化方式
            props.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, "org.apache.kafka.common.serialization.StringSerializer");
            props.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, "org.apache.kafka.common.serialization.StringSerializer");
            //请求的最长等待时间
            props.put(ProducerConfig.MAX_BLOCK_MS_CONFIG, 30 * 1000);
    
            //构造 Producer 对象，注意，该对象是线程安全的。
            //一般来说，一个进程内一个 Producer 对象即可。如果想提高性能，可构造多个对象，但最好不要超过 5 个
            KafkaProducer<String, String> producer = new KafkaProducer<String, String>(props);
    
            //构造一个消息队列 Kafka 消息
            String topic = kafkaProperties.getProperty("topic"); //消息所属的 Topic，请在控制台创建后，填写在这里
            String value = "this is the message's value"; //消息的内容
    
            ProducerRecord<String, String>  kafkaMessage =  new ProducerRecord<String, String>(topic, value);
    
            try {
                //发送消息，并获得一个 Future 对象
                Future<RecordMetadata> metadataFuture = producer.send(kafkaMessage);
                //同步获得 Future 对象的结果
                RecordMetadata recordMetadata = metadataFuture.get();
                System.out.println("Produce ok:" + recordMetadata.toString());
            } catch (Exception e) {
                //要考虑重试，参见常见问题: https://help.aliyun.com/document_detail/124136.html
                System.out.println("error occurred");
                e.printStackTrace();
            }
        }
    }
    					
    ```

4.  消费消息。


``` {#codeblock_h48_19s_xxx .language-java}
public class KafkaConsumerDemo {

    public static void main(String args[]) {

        //加载 kafka.properties
        Properties kafkaProperties =  JavaKafkaConfigurer.getKafkaProperties();

        Properties props = new Properties();
        //设置接入点，请通过控制台获取对应 Topic 的接入点
        props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, kafkaProperties.getProperty("bootstrap.servers"));
        //默认值为 30000 ms，可根据自己业务场景调整此值，建议取值不要太小，防止在超时时间内没有发送心跳导致消费者再均衡
        props.put(ConsumerConfig.SESSION_TIMEOUT_MS_CONFIG, 25000);
        //每次 poll 的最大数量
        //注意该值不要改得太大，如果 poll 太多数据，而不能在下次 poll 之前消费完，则会触发一次负载均衡，产生卡顿
        props.put(ConsumerConfig.MAX_POLL_RECORDS_CONFIG, 30);
        //消息的反序列化方式
        props.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, "org.apache.kafka.common.serialization.StringDeserializer");
        props.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, "org.apache.kafka.common.serialization.StringDeserializer");
        //当前消费实例所属的 Consumer Group，请在控制台创建后填写
        //属于同一个 Consumer Group 的消费实例，会负载消费消息
        props.put(ConsumerConfig.GROUP_ID_CONFIG, kafkaProperties.getProperty("group.id"));
        //构造消息对象，即生成一个消费实例
        KafkaConsumer<String, String> consumer = new org.apache.kafka.clients.consumer.KafkaConsumer<String, String>(props);
        //设置  Consumer Group 订阅的 Topic，可订阅多个 Topic。如果 GROUP_ID_CONFIG 相同，那建议订阅的 Topic 设置也相同
        List<String> subscribedTopics =  new ArrayList<String>();
        //每个 Topic 需要先在控制台进行创建
        subscribedTopics.add(kafkaProperties.getProperty("topic"));
        consumer.subscribe(subscribedTopics);

        //循环消费消息
        while (true){
            try {
                ConsumerRecords<String, String> records = consumer.poll(1000);
                //必须在下次 poll 之前消费完这些数据, 且总耗时不得超过 SESSION_TIMEOUT_MS_CONFIG 的值
                //建议开一个单独的线程池来消费消息，然后异步返回结果
                for (ConsumerRecord<String, String> record : records) {
                    System.out.println(String.format("Consume partition:%d offset:%d", record.partition(), record.offset()));
                }
            } catch (Exception e) {
                try {
                    Thread.sleep(1000);
                } catch (Throwable ignore) {

                }
                //参见常见问题: https://help.aliyun.com/document_detail/124136.html
                e.printStackTrace();
            }
        }
    }
}
			
```

