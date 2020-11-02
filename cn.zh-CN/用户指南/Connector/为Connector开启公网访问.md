---
keyword: [kafka, 公网, 跨地域]
---

# 为Connector开启公网访问

如需使Connector跨地域访问其他阿里云服务，您需要为Connector开启公网访问。本文介绍如何为Connector开启公网访问。

为Connector开启公网访问前，请确保您的消息队列Kafka版实例已开启Connector。详情请参见[开启Connector](/cn.zh-CN/用户指南/Connector/开启Connector.md)。

## 开启公网访问

为Connector开启公网访问的方案如下：

![公网访问方案](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/2016119951/p130354.png)

为Connector开启公网访问的操作流程如下：

1.  为部署了消息队列Kafka版实例的VPC 1创建NAT网关。

    详情请参见[创建NAT网关](/cn.zh-CN/用户指南/NAT网关实例/创建NAT网关.md)。

2.  为创建的NAT网关绑定弹性公网IP。

    详情请参见[绑定NAT网关](/cn.zh-CN/用户指南/绑定云资源/绑定NAT网关.md)。

3.  为VPC 1下消息队列Kafka版实例使用的交换机创建SNAT条目。

    详情请参见[创建SNAT条目](/cn.zh-CN/用户指南/SNAT（访问公网服务）/创建SNAT条目.md)。


