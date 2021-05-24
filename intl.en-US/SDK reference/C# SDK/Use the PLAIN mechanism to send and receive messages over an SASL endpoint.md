# Use the PLAIN mechanism to send and receive messages over an SASL endpoint

This topic describes how to connect to Message Queue for Apache Kafka over a Simple Authentication and Security Layer \(SASL\) endpoint in a virtual private cloud \(VPC\) and use the PLAIN mechanism to send and receive messages.

.NET is installed. For more information, see [Download .NET](https://dotnet.microsoft.com/download).

## Install the C\# library

1.  Run the following command to install the C\# library:

    ```
    dotnet add package -v 1.5.2 Confluent.Kafka
    ```


## Send messages

1.  Create a message sending program named producer.cs.

    ```
    using System;
    using Confluent.Kafka;
    
    class Producer
    {
        public static void Main(string[] args)
        {
            var conf = new ProducerConfig {
                BootstrapServers = "XXX,XXX,XXX",
                SaslMechanism = SaslMechanism.Plain,
                SecurityProtocol = SecurityProtocol.SaslPlaintext,
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
    |BootstrapServers|The SASL endpoint. You can obtain the endpoint in the **Basic Information** section of the **Instance Details** page in the Message Queue for Apache Kafka console.|
    |SaslUsername|The username.    -   If ACL is disabled for the instance, you can obtain the default username on the **Instance Details** page in the Message Queue for Apache Kafka console.
    -   If ACL is enabled for the instance, make sure that the SASL user to be used is of the PLAIN type and the user is authorized to send and receive messages. For more information, see [Authorize SASL users](/intl.en-US/Access control/Authorize SASL users.md). |
    |SaslPassword|The password.    -   If ACL is disabled for the instance, you can obtain the password of the default user on the **Instance Details** page in the Message Queue for Apache Kafka console.
    -   If ACL is enabled for the instance, make sure that the SASL user to be used is of the PLAIN type and the user is authorized to send and receive messages. For more information, see [Authorize SASL users](/intl.en-US/Access control/Authorize SASL users.md). |
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
                SaslMechanism = SaslMechanism.Plain,
                SecurityProtocol = SecurityProtocol.SaslPlaintext,
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
    |BootstrapServers|The SASL endpoint. You can obtain the endpoint in the **Basic Information** section of the **Instance Details** page in the Message Queue for Apache Kafka console.|
    |SaslUsername|The username.    -   If ACL is disabled for the instance, you can obtain the default username on the **Instance Details** page in the Message Queue for Apache Kafka console.
    -   If ACL is enabled for the instance, make sure that the SASL user to be used is of the PLAIN type and the user is authorized to send and receive messages. For more information, see [Authorize SASL users](/intl.en-US/Access control/Authorize SASL users.md). |
    |SaslPassword|The password.    -   If ACL is disabled for the instance, you can obtain the password of the default user on the **Instance Details** page in the Message Queue for Apache Kafka console.
    -   If ACL is enabled for the instance, make sure that the SASL user to be used is of the PLAIN type and the user is authorized to send and receive messages. For more information, see [Authorize SASL users](/intl.en-US/Access control/Authorize SASL users.md). |
    |topic|The name of the topic. You can obtain the name of the topic on the **Topics** page in the Message Queue for Apache Kafka console.|

2.  Run the following command to consume messages:

    ```
    dotnet run consumer.cs
    ```


