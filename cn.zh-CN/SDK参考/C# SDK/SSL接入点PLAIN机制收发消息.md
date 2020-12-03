---
keyword: [kafka, 收发消息, c\#, 公网, ssl]
---

# SSL接入点PLAIN机制收发消息

本文介绍如何在公网环境下使用C\# SDK接入消息队列Kafka版的SSL接入点并使用PLAIN机制收发消息。

您已安装.NET。更多信息，请参见[安装.NET](https://dotnet.microsoft.com/download)。

## 安装C\#依赖库

1.  执行以下命令安装C\#依赖库。

    ```
    dotnet add package -v 1.5.2 Confluent.Kafka
    ```


## 准备配置

1.  [下载SSL根证书](https://code.aliyun.com/alikafka/aliware-kafka-demos/raw/master/kafka-php-demo/vpc-ssl/ca-cert.pem)。


## 发送消息

1.  创建发送消息程序producer.cs。

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
                Console.WriteLine(!r.Error.IsError
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

    |参数|描述|
    |--|--|
    |BootstrapServers|SSL接入点。您可在消息队列Kafka版控制台的**实例详情**页面的**基本信息**区域获取。|
    |SslCaLocation|下载的SSL根证书的路径。|
    |SaslUsername|用户名。    -   如果实例未开启ACL，您可以在消息队列Kafka版控制台的**实例详情**页面获取默认用户的用户名。
    -   如果实例已开启ACL，请确保要使用的SASL用户为PLAIN类型且已授权收发消息的权限。更多信息，请参见[SASL用户授权](/cn.zh-CN/权限控制/SASL用户授权.md)。 |
    |SaslPassword|密码。    -   如果实例未开启ACL，您可以在消息队列Kafka版控制台的**实例详情**页面获取默认用户的密码。
    -   如果实例已开启ACL，请确保要使用的SASL用户为PLAIN类型且已授权收发消息的权限。更多信息，请参见[SASL用户授权](/cn.zh-CN/权限控制/SASL用户授权.md)。 |
    |topic|Topic名称。您可在消息队列Kafka版控制台的**Topic管理**页面获取。|

2.  执行以下命令发送消息。

    ```
    dotnet run producer.cs
    ```


## 订阅消息

1.  创建订阅消息程序consumer.cs。

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

    |参数|描述|
    |--|--|
    |GroupId|Consumer Group名称。您可在消息队列Kafka版控制台的**Consumer Group管理**页面获取。|
    |BootstrapServers|SSL接入点。您可在消息队列Kafka版控制台的**实例详情**页面的**基本信息**区域获取。|
    |SslCaLocation|下载的SSL根证书的路径。|
    |SaslUsername|用户名。    -   如果实例未开启ACL，您可以在消息队列Kafka版控制台的**实例详情**页面获取默认用户的用户名。
    -   如果实例已开启ACL，请确保要使用的SASL用户为PLAIN类型且已授权收发消息的权限。更多信息，请参见[SASL用户授权](/cn.zh-CN/权限控制/SASL用户授权.md)。 |
    |SaslPassword|密码。    -   如果实例未开启ACL，您可以在消息队列Kafka版控制台的**实例详情**页面获取默认用户的密码。
    -   如果实例已开启ACL，请确保要使用的SASL用户为PLAIN类型且已授权收发消息的权限。更多信息，请参见[SASL用户授权](/cn.zh-CN/权限控制/SASL用户授权.md)。 |
    |topic|Topic名称。您可在消息队列Kafka版控制台的**Topic管理**页面获取。|

2.  执行以下命令消费消息。

    ```
    dotnet run consumer.cs
    ```


