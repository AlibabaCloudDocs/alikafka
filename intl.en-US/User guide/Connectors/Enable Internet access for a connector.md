---
keyword: [kafka, internet, cross-region]
---

# Enable Internet access for a connector

If you need to access other Alibaba Cloud services in other regions by using a connector, you must enable Internet access for the connector. This topic describes how to enable Internet access for a connector.

Before you enable Internet access for a connector, ensure that the connector feature is enabled in your Message Queue for Apache Kafka instance. For more information, see [Enable the connector feature](/intl.en-US/User guide/Connectors/Enable the connector feature.md).

## Enable Internet access

The following figure shows the solution for enabling Internet access for a connector.

![Solution for enabling Internet access for a connector](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3350549951/p130354.png)

Complete the following steps to enable Internet access for a connector:

1.  Create a Network Address Translation \(NAT\) gateway for virtual private cloud \(VPC\) 1 where the Message Queue for Apache Kafka instance is deployed.

    For more information, see [Create a NAT gateway]().

2.  Bind an elastic IP address \(EIP\) to the created NAT gateway.

    For more information, see [Associate an EIP with a NAT gateway](/intl.en-US/User Guide/Associate an EIP with a cloud instance/Associate an EIP with a NAT gateway.md).

3.  Create Source Network Address Translation \(SNAT\) entries for the vSwitch that is used by the Message Queue for Apache Kafka instance on VPC 1.

    For more information, see [Create a SNAT entry to access the Internet](/intl.en-US/User Guide/Create a SNAT entry to access the Internet.md).


