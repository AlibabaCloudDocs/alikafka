---
keyword: [Kafka, send and receive messages, C\#, VPCs]
---

# Use the default endpoint to send and receive messages

This topic describes how to use SDKs for C\# to connect to the default endpoint of Message Queue for Apache Kafka and send and receive messages in a virtual private cloud \(VPC\).

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
    |BootstrapServers|The default endpoint. You can obtain the default endpoint in the **Basic Information** section of the **Instance Details** page in the Message Queue for Apache Kafka console.|
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
    |BootstrapServers|The default endpoint. You can obtain the default endpoint in the **Basic Information** section of the **Instance Details** page in the Message Queue for Apache Kafka console.|
    |topic|The name of the topic. You can obtain the name of the topic on the **Topics** page in the Message Queue for Apache Kafka console.|

2.  Run the following command to consume messages:

    ```
    dotnet run consumer.cs
    ```

