---
keyword: [go, internet, send and subscribe to messages, plain]
---

# Send and subscribe to messages by using an SSL endpoint with PLAIN authentication

This topic describes how a Go client uses a Secure Sockets Layer \(SSL\) endpoint to connect to Message Queue for Apache Kafka in the Internet environment and uses the PLAIN mechanism to send and subscribe to messages.

Go is installed. For more information, see [Go downloads](https://golang.org/dl/).

**Note:** You cannot run kafka-confluent-go-demo on Windows.

## Prepare configurations

1.  Click [aliware-kafka-demos](https://github.com/AliwareMQ/aliware-kafka-demos). On the page that appears, click the ![code](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8358442261/p271734.png) icon and select **Download ZIP** to download the demo package and decompress it.

2.  In the decompressed demo package, find the kafka-confluent-go-demo folder and upload the folder to the /home directory in a Linux system.

3.  Log on to the Linux system, go to the /home/kafka-confluent-go-demo directory, and then modify the configuration file conf/kafka.json.

    ```
    {
      "topic": "XXX",
      "group.id": "XXX",
      "bootstrap.servers" : "XXX:XX,XXX:XX,XXX:XX",
      "security.protocol" : "sasl_ssl",
      "sasl.mechanism" : "PLAIN",
      "sasl.username" : "XXX",
      "sasl.password" : "XXX"
    }
    ```

    |Parameter|Description|Required|
    |---------|-----------|--------|
    |topic|The name of the topic that you created in the Message Queue for Apache Kafka instance. You can obtain the name of the topic on the **Topics** page in the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).|Yes|
    |group.id|The name of the consumer group that you created in the Message Queue for Apache Kafka instance. You can obtain the name of the consumer group on the **Consumer Groups** page in the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).|No**Note:** If the application runs producer.go to send messages, this parameter is optional. If the application runs consumer.go to subscribe to messages, this parameter is required. |
    |bootstrap.servers|The IP address and port of the SSL endpoint of the Message Queue for Apache Kafka instance. You can obtain the SSL endpoint in the **Basic Information** section of the **Instance Details** page in the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm).|Yes|
    |security.protocol|The protocol that is used to authenticate Simple Authentication and Security Layer \(SASL\) users. The default value is plaintext. Set the parameter to sasl\_ssl.|Yes|
    |sasl.mechanism|The mechanism that is used to send and subscribe to messages. The default value is PLAIN.|Yes|
    |sasl.username|The name of the SASL user. You can obtain the name on the **SASL Users** tab of the **Instance Details** page in the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm). **Note:** If access control list \(ACL\) is enabled for the Message Queue for Apache Kafka instance, make sure that the SASL user to be used is of the PLAIN type and is authorized to send and subscribe to messages. For more information, see [Authorize SASL users](/intl.en-US/Access control/Authorize SASL users.md).

|Yes|
    |sasl.password|The password of the SASL user. You can obtain the password on the **SASL Users** tab of the **Instance Details** page in the [Message Queue for Apache Kafka console](https://kafka.console.aliyun.com/?spm=a2c4g.11186623.2.22.6bf72638IfKzDm). **Note:** If ACL is enabled for the Message Queue for Apache Kafka instance, make sure that the SASL user to be used is of the PLAIN type and is authorized to send and subscribe to messages. For more information, see [Authorize SASL users](/intl.en-US/Access control/Authorize SASL users.md).

|Yes|


## Send messages

1.  Run the following command to run producer.go to send messages:

    ```
    go run -mod=vendor producer/producer.go
    ```

    The following sample code provides an example of producer.go:

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


## Subscribe to messages

1.  Run the following command to run consumer.go to subscribe to messages:

    ```
    go run -mod=vendor consumer/consumer.go
    ```

    The following sample code provides an example of consumer.go:

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


