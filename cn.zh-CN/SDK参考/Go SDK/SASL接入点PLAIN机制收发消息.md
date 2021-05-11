---
keyword: [go, VPC, 收发消息, PLAIN]
---

# SASL接入点PLAIN机制收发消息

本文介绍如何在VPC环境下通过SASL接入点接入消息队列Kafka版并使用PLAIN机制收发消息。

您已安装Go。更多信息，请参见[安装Go](https://golang.org/dl/)。

**说明：** 该kafka-confluent-go-demo不支持Windows系统。

## 准备配置

1.  访问[aliware-kafka-demos](https://github.com/AliwareMQ/aliware-kafka-demos)，单击![code](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7278540261/p271734.png)图标，然后在下拉框选择**Download ZIP**，下载Demo包并解压。

2.  在解压的Demo包中，找到kafka-confluent-go-demo文件夹，将此文件夹上传在Linux系统的/home路径下。

3.  登录Linux系统，进入/home/kafka-confluent-go-demo路径，修改配置文件conf/kafka.json。

    ```
    {
      "topic": "XXX",
      "group.id": "XXX",
      "bootstrap.servers" : "XXX:XX,XXX:XX,XXX:XX",
      "security.protocol" : "sasl_plaintext",
      "sasl.mechanism" : "PLAIN",
      "sasl.username" : "XXX",
      "sasl.password" : "XXX"
    }
    ```

    |参数|描述|是否必须|
    |--|--|----|
    |topic|实例的Topic名称。您可在[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)的**Topic管理**页面获取。|是|
    |group.id|实例的Consumer Group。您可在[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)的**Consumer Group管理**页面获取。|否**说明：** 如果应用运行producer.go发送消息，该参数可以不配置；如果应用运行consumer.go订阅消息，该参数必须配置。 |
    |bootstrap.servers|SASL接入点的IP地址以及端口。您可在[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)的**实例详情**页面的**基本信息**区域获取。|是|
    |security.protocol|SASL用户认证协议，默认为plaintext，需配置为sasl\_plaintext。|是|
    |sasl.mechanism|消息收发的机制，默认为PLAIN。|是|
    |sasl.username|SASL用户名。您可以在[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)**实例详情**页面的**SASL用户**页签获取。**说明：** 如果实例已开启ACL，请确保要接入的SASL用户为PLAIN类型且已授权收发消息的权限。具体操作，请参见[SASL用户授权](/cn.zh-CN/权限控制/SASL用户授权.md)。

|是|
    |sasl.password|SASL用户密码。您可以在[消息队列Kafka版控制台](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm)**实例详情**页面的**SASL用户**页签获取。**说明：** 如果实例已开启ACL，请确保要接入的SASL用户为PLAIN类型且已授权收发消息的权限。具体操作，请参见[SASL用户授权](/cn.zh-CN/权限控制/SASL用户授权.md)。

|是|


## 发送消息

1.  执行以下命令运行producer.go发送消息。

    ```
    go run -mod=vendor producer/producer.go
    ```

    producer.go示例代码如下所示。

    ```
    package main
    
    import (
        "encoding/json"
        "fmt"
        "github.com/confluentinc/confluent-kafka-go/kafka"
        "os"
        "path/filepath"
    )
    
    type KafkaConfig struct {
        Topic      string `json:"topic"`
        GroupId    string `json:"group.id"`
        BootstrapServers    string `json:"bootstrap.servers"`
        SecurityProtocol string `json:"security.protocol"`
        SslCaLocation string `json:"ssl.ca.location"`
        SaslMechanism string `json:"sasl.mechanism"`
        SaslUsername string `json:"sasl.username"`
        SaslPassword string `json:"sasl.password"`
    }
    
    // config should be a pointer to structure, if not, panic
    func loadJsonConfig() *KafkaConfig {
        workPath, err := os.Getwd()
        if err != nil {
            panic(err)
        }
        configPath := filepath.Join(workPath, "conf")
        fullPath := filepath.Join(configPath, "kafka.json")
        file, err := os.Open(fullPath);
        if (err != nil) {
            msg := fmt.Sprintf("Can not load config at %s. Error: %v", fullPath, err)
            panic(msg)
        }
    
        defer file.Close()
    
        decoder := json.NewDecoder(file)
        var config = &KafkaConfig{}
        err = decoder.Decode(config);
        if (err != nil) {
            msg := fmt.Sprintf("Decode json fail for config file at %s. Error: %v", fullPath, err)
            panic(msg)
        }
        json.Marshal(config)
        return  config
    }
    
    func doInitProducer(cfg *KafkaConfig) *kafka.Producer {
        fmt.Print("init kafka producer, it may take a few seconds to init the connection\n")
        //common arguments
        var kafkaconf = &kafka.ConfigMap{
            "api.version.request": "true",
            "message.max.bytes": 1000000,
            "linger.ms": 10,
            "retries": 30,
            "retry.backoff.ms": 1000,
            "acks": "1"}
        kafkaconf.SetKey("bootstrap.servers", cfg.BootstrapServers)
    
        switch cfg.SecurityProtocol {
            case "plaintext" :
                kafkaconf.SetKey("security.protocol", "plaintext");
            case "sasl_ssl":
                kafkaconf.SetKey("security.protocol", "sasl_ssl");
                kafkaconf.SetKey("ssl.ca.location", "conf/ca-cert.pem");
                kafkaconf.SetKey("sasl.username", cfg.SaslUsername);
                kafkaconf.SetKey("sasl.password", cfg.SaslPassword);
                kafkaconf.SetKey("sasl.mechanism", cfg.SaslMechanism)
        case "sasl_plaintext":
                kafkaconf.SetKey("sasl.mechanism", "PLAIN")
                kafkaconf.SetKey("security.protocol", "sasl_plaintext");
                kafkaconf.SetKey("sasl.username", cfg.SaslUsername);
                kafkaconf.SetKey("sasl.password", cfg.SaslPassword);
                kafkaconf.SetKey("sasl.mechanism", cfg.SaslMechanism)
        default:
                panic(kafka.NewError(kafka.ErrUnknownProtocol, "unknown protocol", true))
        }
    
        producer, err := kafka.NewProducer(kafkaconf)
        if err != nil {
            panic(err)
        }
        fmt.Print("init kafka producer success\n")
        return producer;
    }
    
    func main() {
        // Choose the correct protocol
        // 9092 for PLAINTEXT
        // 9093 for SASL_SSL, need to provide sasl.username and sasl.password
        // 9094 for SASL_PLAINTEXT, need to provide sasl.username and sasl.password
        cfg := loadJsonConfig();
        producer := doInitProducer(cfg)
    
        defer producer.Close()
    
        // Delivery report handler for produced messages
        go func() {
            for e := range producer.Events() {
                switch ev := e.(type) {
                case *kafka.Message:
                    if ev.TopicPartition.Error != nil {
                        fmt.Printf("Delivery failed: %v\n", ev.TopicPartition)
                    } else {
                        fmt.Printf("Delivered message to %v\n", ev.TopicPartition)
                    }
                }
            }
        }()
    
        // Produce messages to topic (asynchronously)
        topic := cfg.Topic
        for _, word := range []string{"Welcome", "to", "the", "Confluent", "Kafka", "Golang", "client"} {
            producer.Produce(&kafka.Message{
                TopicPartition: kafka.TopicPartition{Topic: &topic, Partition: kafka.PartitionAny},
                Value:          []byte(word),
            }, nil)
        }
    
        // Wait for message deliveries before shutting down
        producer.Flush(15 * 1000)
    }
                            
    ```


## 订阅消息

1.  执行以下命令运行consumer.go订阅消息。

    ```
    go run -mod=vendor consumer/consumer.go
    ```

    consumer.go示例代码如下所示。

    ```
    package main
    
    import (
        "encoding/json"
        "fmt"
        "github.com/confluentinc/confluent-kafka-go/kafka"
        "os"
        "path/filepath"
    )
    type KafkaConfig struct {
        Topic      string `json:"topic"`
        GroupId    string `json:"group.id"`
        BootstrapServers    string `json:"bootstrap.servers"`
        SecurityProtocol string `json:"security.protocol"`
        SaslMechanism string `json:"sasl.mechanism"`
        SaslUsername string `json:"sasl.username"`
        SaslPassword string `json:"sasl.password"`
    }
    
    // config should be a pointer to structure, if not, panic
    func loadJsonConfig() *KafkaConfig {
        workPath, err := os.Getwd()
        if err != nil {
            panic(err)
        }
        configPath := filepath.Join(workPath, "conf")
        fullPath := filepath.Join(configPath, "kafka.json")
        file, err := os.Open(fullPath);
        if (err != nil) {
            msg := fmt.Sprintf("Can not load config at %s. Error: %v", fullPath, err)
            panic(msg)
        }
    
        defer file.Close()
    
        decoder := json.NewDecoder(file)
        var config = &KafkaConfig{}
        err = decoder.Decode(config);
        if (err != nil) {
            msg := fmt.Sprintf("Decode json fail for config file at %s. Error: %v", fullPath, err)
            panic(msg)
        }
        json.Marshal(config)
        return  config
    }
    
    
    func doInitConsumer(cfg *KafkaConfig) *kafka.Consumer {
        fmt.Print("init kafka consumer, it may take a few seconds to init the connection\n")
        //common arguments
        var kafkaconf = &kafka.ConfigMap{
            "api.version.request": "true",
            "auto.offset.reset": "latest",
            "heartbeat.interval.ms": 3000,
            "session.timeout.ms": 30000,
            "max.poll.interval.ms": 120000,
            "fetch.max.bytes": 1024000,
            "max.partition.fetch.bytes": 256000}
        kafkaconf.SetKey("bootstrap.servers", cfg.BootstrapServers);
        kafkaconf.SetKey("group.id", cfg.GroupId)
    
        switch cfg.SecurityProtocol {
        case "plaintext" :
            kafkaconf.SetKey("security.protocol", "plaintext");
        case "sasl_ssl":
            kafkaconf.SetKey("security.protocol", "sasl_ssl");
            kafkaconf.SetKey("ssl.ca.location", "./conf/ca-cert.pem");
            kafkaconf.SetKey("sasl.username", cfg.SaslUsername);
            kafkaconf.SetKey("sasl.password", cfg.SaslPassword);
            kafkaconf.SetKey("sasl.mechanism", cfg.SaslMechanism)
        case "sasl_plaintext":
            kafkaconf.SetKey("security.protocol", "sasl_plaintext");
            kafkaconf.SetKey("sasl.username", cfg.SaslUsername);
            kafkaconf.SetKey("sasl.password", cfg.SaslPassword);
            kafkaconf.SetKey("sasl.mechanism", cfg.SaslMechanism)
    
        default:
            panic(kafka.NewError(kafka.ErrUnknownProtocol, "unknown protocol", true))
        }
    
        consumer, err := kafka.NewConsumer(kafkaconf)
        if err != nil {
            panic(err)
        }
        fmt.Print("init kafka consumer success\n")
        return consumer;
    }
    
    func main() {
    
        // Choose the correct protocol
        // 9092 for PLAINTEXT
        // 9093 for SASL_SSL, need to provide sasl.username and sasl.password
        // 9094 for SASL_PLAINTEXT, need to provide sasl.username and sasl.password
        cfg := loadJsonConfig();
        consumer := doInitConsumer(cfg)
    
        consumer.SubscribeTopics([]string{cfg.Topic}, nil)
    
        for {
            msg, err := consumer.ReadMessage(-1)
            if err == nil {
                fmt.Printf("Message on %s: %s\n", msg.TopicPartition, string(msg.Value))
            } else {
                // The client will automatically try to recover from all errors.
                fmt.Printf("Consumer error: %v (%v)\n", err, msg)
            }
        }
    
        consumer.Close()
    }
    ```


