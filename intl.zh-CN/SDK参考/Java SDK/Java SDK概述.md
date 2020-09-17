# Java SDK概述

Java客户端可以通过消息队列Kafka版提供的多种接入点接入并收发消息。

消息队列Kafka版提供以下接入点。

|项目|默认接入点|SASL接入点|
|--|-----|-------|
|网络|VPC|VPC|
|协议|PLAINTEXT|SASL\_PLAINTEXT|
|端口|9092|9094|
|SASL机制|不适用|-   PLAIN：一种简单的用户名密码校验机制。消息队列Kafka版优化了PLAIN机制，支持不重启实例的情况下动态增加SASL用户。
-   SCRAM：一种用户名密码校验机制，安全性比PLAIN更高。消息队列Kafka版使用SCRAM-SHA-256。 |
|Demo|[PLAINTEXT](https://code.aliyun.com/alikafka/aliware-kafka-demos/tree/master/kafka-java-demo/vpc)|-   [SASL\_PLAINTEXT/PLAIN](https://code.aliyun.com/alikafka/aliware-kafka-demos/tree/master/kafka-java-demo/vpc-9094)
-   [SASL\_PLAINTEXT/SCRAM](https://code.aliyun.com/alikafka/aliware-kafka-demos/tree/master/kafka-java-demo/vpc-9094) |
|文档|[默认接入点收发消息](/intl.zh-CN/SDK参考/Java SDK/默认接入点收发消息.md)|-   [SASL接入点PLAIN机制收发消息](/intl.zh-CN/SDK参考/Java SDK/SASL接入点PLAIN机制收发消息.md)
-   [SASL接入点SCRAM机制收发消息]() |

