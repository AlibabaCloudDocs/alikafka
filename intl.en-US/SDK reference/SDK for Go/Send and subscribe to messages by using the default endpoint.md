---
keyword: [go, vpc, send and subscribe to messages, plaintext]
---

# Send and subscribe to messages by using the default endpoint

This topic describes how a Go client uses SDK for Go to connect to the default endpoint of Message Queue for Apache Kafka in a virtual private cloud \(VPC\) and send and subscribe to messages.

You have installed Go. For more information, see [Install Go](https://golang.org/dl/).

## Install the Go libraries

1.  Run the following commands to install the Go libraries:

    1.  `go get github.com/Shopify/sarama/`

    2.  `go get github.com/bsm/sarama-cluster`

2.  Run the following commands to compile the Go libraries:

    1.  `go install services`

    2.  `go install services/producer`

    3.  `go install services/consumer`


## Preparations

1.  Create a Message Queue for Apache Kafka configuration file named kafka.json.

    ```
    {
      "topics": ["XXX"],
      "servers": ["XXX","XXX","XXX"],
      "consumerGroup": "XXX"
    }
    ```

    |Parameter|Description|
    |---------|-----------|
    |topics|The name of the topic. You can obtain the name of the topic on the **Topics** page in the Message Queue for Apache Kafka console.|
    |servers|The default endpoint. You can obtain the default endpoint in the **Basic Information** section of the **Instance Details** page in the Message Queue for Apache Kafka console.|
    |consumerGroup|The name of the consumer group. You can obtain the name of the consumer group on the **Consumer Groups** page in the Message Queue for Apache Kafka console.|

2.  Create a configurator named configs.go.

    ```
    package configs
    
    import (
        "encoding/json"
        "fmt"
        "io/ioutil"
        "os"
        "path/filepath"
    )
    
    var (
        configPath string
    )
    
    func init() {
        var err error
    
        workPath, err := os.Getwd()
        if err ! = nil {
            panic(err)
        }
    
        configPath = filepath.Join(workPath, "conf")
    }
    
    // LoadJsonConfig is used to load a configuration file to config in JSON format.
    // config indicates a pointer to a struct. Otherwise, a "panic" error is thrown.
    func LoadJsonConfig(config interface{}, filename string) {
        var err error
        var decoder *json.Decoder
    
        file := OpenFile(filename)
        defer file.Close()
    
        decoder = json.NewDecoder(file)
        if err = decoder.Decode(config); err ! = nil {
            msg := fmt.Sprintf("Decode json fail for config file at %s. Error: %v", filename, err)
            panic(msg)
        }
    
        json.Marshal(config)
    }
    
    func LoadJsonFile(filename string) (cfg string) {
    
        file := OpenFile(filename)
        defer file.Close()
    
        content, err := ioutil.ReadAll(file)
        if err ! = nil {
            msg := fmt.Sprintf("Read config to string error. file at %s. Error: %v", filename, err)
            panic(msg)
        }
    
        cfg = string(content)
    
        return cfg
    }
    
    func GetFullPath(filename string) string {
        return filepath.Join(configPath, filename)
    }
    
    func OpenFile(filename string) *os.File {
        fullPath := filepath.Join(configPath, filename)
    
        var file *os.File
        var err error
    
        if file, err = os.Open(fullPath); err ! = nil {
            msg := fmt.Sprintf("Can not load config at %s. Error: %v", fullPath, err)
            panic(msg)
        }
    
        return file
    }
    ```

3.  Create a configurator named types.go.

    ```
    package configs
    
    type MqConfig struct {
        Topics     []string `json:"topics"`
        Servers    []string `json:"servers"`
        ConsumerId string   `json:"consumerGroup"`
    }
    ```


## Send messages

1.  Create a message sender named producer.go.

    ```
    import (
        "fmt"
        "services"
        "time"
        "strconv"
    
        "github.com/Shopify/sarama"
    )
    
    var cfg *configs.MqConfig
    var producer sarama.SyncProducer
    
    func init() {
    
        fmt.Print("init kafka producer, it may take a few seconds to init the connection\n")
    
        var err error
    
        cfg = &configs.MqConfig{}
        configs.LoadJsonConfig(cfg, "kafka.json")
    
        mqConfig := sarama.NewConfig()
    
        mqConfig.Producer.Return.Successes = true
        mqConfig.Version=sarama.V0_10_2_1
    
        if err = mqConfig.Validate(); err ! = nil {
            msg := fmt.Sprintf("Kafka producer config invalidate. config: %v. err: %v", *cfg, err)
            fmt.Println(msg)
            panic(msg)
        }
    
        producer, err = sarama.NewSyncProducer(cfg.Servers, mqConfig)
        if err ! = nil {
            msg := fmt.Sprintf("Kafak producer create fail. err: %v", err)
            fmt.Println(msg)
            panic(msg)
        }
    
    }
    
    func produce(topic string, key string, content string) error {
        msg := &sarama.ProducerMessage{
            Topic: topic,
            Key:   sarama.StringEncoder(key),
            Value: sarama.StringEncoder(content),
            Timestamp: time.Now(),
        }
    
        _, _, err := producer.SendMessage(msg)
        if err ! = nil {
            msg := fmt.Sprintf("Send Error topic: %v. key: %v. content: %v", topic, key, content)
            fmt.Println(msg)
            return err
        }
        fmt.Printf("Send OK topic:%s key:%s value:%s\n", topic, key, content)
    
        return nil
    }
    
    func main() {
        key := strconv.FormatInt(time.Now().UTC().UnixNano(), 10)
        value := "this is a kafka message!"
        produce(cfg.Topics[0], key, value)
    }
    ```

2.  Run the following command to send messages:

    ```
    go run producer.go
    ```


## Subscribe to messages

1.  Create a subscription program named consumer.go.

    ```
    import (
        "fmt"
        "os"
        "services"
        "os/signal"
        "github.com/Shopify/sarama"
        "github.com/bsm/sarama-cluster"
    )
    
    var cfg *configs.MqConfig
    var consumer *cluster.Consumer
    var sig chan os.Signal
    
    func init() {
        fmt.Println("init kafka consumer, it may take a few seconds...")
    
        var err error
    
        cfg := &configs.MqConfig{}
        configs.LoadJsonConfig(cfg, "kafka.json")
    
        clusterCfg := cluster.NewConfig()
    
        clusterCfg.Consumer.Return.Errors = true
        clusterCfg.Consumer.Offsets.Initial = sarama.OffsetOldest
        clusterCfg.Group.Return.Notifications = true
    
        clusterCfg.Version = sarama.V0_10_2_1
        if err = clusterCfg.Validate(); err ! = nil {
            msg := fmt.Sprintf("Kafka consumer config invalidate. config: %v. err: %v", *clusterCfg, err)
            fmt.Println(msg)
            panic(msg)
        }
    
        consumer, err = cluster.NewConsumer(cfg.Servers, cfg.ConsumerId, cfg.Topics, clusterCfg)
        if err ! = nil {
            msg := fmt.Sprintf("Create kafka consumer error: %v. config: %v", err, clusterCfg)
            fmt.Println(msg)
            panic(msg)
        }
    
        sig = make(chan os.Signal, 1)
    
    }
    
    func Start() {
        go consume()
    }
    
    func consume() {
        for {
            select {
            case msg, more := <-consumer.Messages():
                if more {
                    fmt.Printf("Partition:%d, Offset:%d, Key:%s, Value:%s Timestamp:%s\n", msg.Partition, msg.Offset, string(msg.Key), string(msg.Value), msg.Timestamp)
                    consumer.MarkOffset(msg, "")
                }
            case err, more := <-consumer.Errors():
                if more {
                    fmt.Println("Kafka consumer error: %v", err.Error())
                }
            case ntf, more := <-consumer.Notifications():
                if more {
                    fmt.Println("Kafka consumer rebalance: %v", ntf)
                }
            case <-sig:
                fmt.Errorf("Stop consumer server...")
                consumer.Close()
                return
            }
        }
    
    }
    
    func Stop(s os.Signal) {
        fmt.Println("Recived kafka consumer stop signal...")
        sig <- s
        fmt.Println("kafka consumer stopped!!!")
    }
    
    func main() {
    
        signals := make(chan os.Signal, 1)
        signal.Notify(signals, os.Interrupt)
    
        Start()
    
        select {
        case s := <-signals:
            Stop(s)
        }
    }
    ```

2.  Run the following command to consume messages:

    ```
    go run consumer.go
    ```


