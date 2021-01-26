---
keyword: [kafka, send and subscribe to messages, c\#, internet, ssl]
---

# Send and subscribe to messages by using an SSL endpoint with PLAIN authentication

This topic describes how a C\# client uses SDK for C\# to connect to the Secure Sockets Layer \(SSL\) endpoint of a Message Queue for Apache Kafka instance over the Internet and uses the PLAIN mechanism to send and subscribe to messages.

## Prepare configurations

1.  [Download an SSL root certificate](https://code.aliyun.com/alikafka/aliware-kafka-demos/raw/master/kafka-php-demo/vpc-ssl/ca-cert.pem).


## Send messages

1.  Create a message sender named producer.cs.

    ```
    using System;
    using Confluent.Kafka;
    
    class Producer
    {
        public static void Main(string[] args)
        {
            var conf = new ProducerConfig {
                BootstrapServers = "XXX,XXX,XXX",
                SslCaLocation = "XXX/ca-cert.pem",
                SaslMechanism = SaslMechanism.Plain,
                SecurityProtocol = SecurityProtocol.SaslSsl,
                SaslUsername = "XXX",
                SaslPassword = "XXX",
                };
    
            Action<DeliveryReport<Null, string>> handler = r =>
                Console.WriteLine(! r.Error.IsError
                    ? $"Delivered message to {r.TopicPartitionOffset}"
                    : $"Delivery Error: {r.Error.Reason}");
    
            string topic ="XXX";
    
            using (var p = new ProducerBuilder<Null, string>(conf).Build())
            {
                for (int i=0; i<100; ++i)
                {
                    p.Produce(topic, new Message<Null, string> { Value = i.ToString() }, handler);
                }
                p.Flush(TimeSpan.FromSeconds(10));
            }
        }
    }
    ```

    |Parameter|Description|
    |---------|-----------|
    |BootstrapServers|The SSL endpoint of the Message Queue for Apache Kafka instance. You can obtain the SSL endpoint in the **Basic Information** section of the **Instance Details** page in the Message Queue for Apache Kafka console.|
    |SslCaLocation|\#\# The path of the SSL root certificate.|
    |SaslUsername|The name of the Simple Authentication and Security Layer \(SASL\) user.    -   If access control list \(ACL\) is disabled for the Message Queue for Apache Kafka instance, you can obtain the default username on the **Instance Details** page in the Message Queue for Apache Kafka console.
    -   If ACL is enabled for the Message Queue for Apache Kafka instance, make sure that the SASL user to be used is of the PLAIN type and that the user is authorized to send and subscribe to messages. For more information, see [Authorize SASL users](/intl.en-US/Access control/Authorize SASL users.md). |
    |SaslPassword|The password of the SASL user.    -   If ACL is disabled for the Message Queue for Apache Kafka instance, you can obtain the password of the default user on the **Instance Details** page in the Message Queue for Apache Kafka console.
    -   If ACL is enabled for the Message Queue for Apache Kafka instance, make sure that the SASL user to be used is of the PLAIN type and that the user is authorized to send and subscribe to messages. For more information, see [Authorize SASL users](/intl.en-US/Access control/Authorize SASL users.md). |
    |topic|The name of the topic. You can obtain the name of the topic on the **Topics** page in the Message Queue for Apache Kafka console.|

2.  Run the following command to send messages:

    ```
    dotnet run producer.cs
    ```


## Subscribe to messages

1.  Create a subscription program named consumer.cs.

    ```
    using System;
    using System.Threading;
    using Confluent.Kafka;
    
    class Consumer
    {
        public static void Main(string[] args)
        {
            var conf = new ConsumerConfig {
                GroupId = "XXX",
                BootstrapServers = "XXX,XXX,XXX",
                SslCaLocation = "XXX/ca-cert.pem",
                SaslMechanism = SaslMechanism.Plain,
                SecurityProtocol = SecurityProtocol.SaslSsl,
                SaslUsername = "XXX",
                SaslPassword = "XXX",
                AutoOffsetReset = AutoOffsetReset.Earliest
            };
    
            string topic = "XXX";
    
            using (var c = new ConsumerBuilder<Ignore, string>(conf).Build())
            {
                c.Subscribe(topic);
    
                CancellationTokenSource cts = new CancellationTokenSource();
                Console.CancelKeyPress += (_, e) => {
                    e.Cancel = true;
                    cts.Cancel();
                };
    
                try
                {
                    while (true)
                    {
                        try
                        {
                            var cr = c.Consume(cts.Token);
                            Console.WriteLine($"Consumed message '{cr.Value}' at: '{cr.TopicPartitionOffset}'.");
                        }
                        catch (ConsumeException e)
                        {
                            Console.WriteLine($"Error occured: {e.Error.Reason}");
                        }
                    }
                }
                catch (OperationCanceledException)
                {
                    c.Close();
                }
            }
        }
    }
    ```

    |Parameter|Description|
    |---------|-----------|
    |GroupId|The name of the consumer group. You can obtain the name of the consumer group on the **Consumer Groups** page in the Message Queue for Apache Kafka console.|
    |BootstrapServers|The SSL endpoint of the Message Queue for Apache Kafka instance. You can obtain the SSL endpoint in the **Basic Information** section of the **Instance Details** page in the Message Queue for Apache Kafka console.|
    |SslCaLocation|\#\# The path of the SSL root certificate.|
    |SaslUsername|The name of the SASL user.    -   If ACL is disabled for the Message Queue for Apache Kafka instance, you can obtain the default username on the **Instance Details** page in the Message Queue for Apache Kafka console.
    -   If ACL is enabled for the Message Queue for Apache Kafka instance, make sure that the SASL user to be used is of the PLAIN type and that the user is authorized to send and subscribe to messages. For more information, see [Authorize SASL users](/intl.en-US/Access control/Authorize SASL users.md). |
    |SaslPassword|The password of the SASL user.    -   If ACL is disabled for the Message Queue for Apache Kafka instance, you can obtain the password of the default user on the **Instance Details** page in the Message Queue for Apache Kafka console.
    -   If ACL is enabled for the Message Queue for Apache Kafka instance, make sure that the SASL user to be used is of the PLAIN type and that the user is authorized to send and subscribe to messages. For more information, see [Authorize SASL users](/intl.en-US/Access control/Authorize SASL users.md). |
    |topic|The name of the topic. You can obtain the name of the topic on the **Topics** page in the Message Queue for Apache Kafka console.|

2.  Run the following command to consume messages:

    ```
    dotnet run consumer.cs
    ```


