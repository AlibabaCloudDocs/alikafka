# C\# SDK概述

C\#客户端可以通过消息队列Kafka版提供的多种接入点接入并收发消息。

消息队列Kafka版提供以下接入点。

|项目|默认接入点|SSL接入点|SASL接入点|
|--|-----|------|-------|
|网络|VPC|公网|VPC|
|协议|PLAINTEXT|SASL\_SSL|SASL\_PLAINTEXT|
|端口|9092|9093|9094|
|SASL机制|不适用|PLAIN：一种简单的用户名密码校验机制。消息队列Kafka版优化了PLAIN机制，支持不重启实例的情况下动态增加SASL用户。|-   PLAIN：一种简单的用户名密码校验机制。消息队列Kafka版优化了PLAIN机制，支持不重启实例的情况下动态增加SASL用户。
-   SCRAM：一种用户名密码校验机制，安全性比PLAIN更高。消息队列Kafka版使用SCRAM-SHA-256。 |
|Demo|无|无|无|
|文档|[默认接入点收发消息](/cn.zh-CN/SDK参考/C# SDK/默认接入点收发消息.md)|[SSL接入点PLAIN机制收发消息](/cn.zh-CN/SDK参考/C# SDK/SSL接入点PLAIN机制收发消息.md)|-   [SASL接入点PLAIN机制收发消息](/cn.zh-CN/SDK参考/C# SDK/SASL接入点PLAIN机制收发消息.md)
-   [SASL接入点SCRAM机制收发消息](/cn.zh-CN/SDK参考/C# SDK/SASL接入点SCRAM机制收发消息.md) |

