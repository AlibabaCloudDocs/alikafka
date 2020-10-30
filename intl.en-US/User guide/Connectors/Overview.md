---
keyword: [kafka, connector]
---

# Overview

Message Queue for Apache Kafka provides fully-managed and maintenance-free connectors for synchronizing data between Message Queue for Apache Kafka and other Alibaba Cloud services. The Connector feature is in public preview. This topic describes the types, procedure, and limits of connectors, and cross-region data synchronization by using connectors.

## Types of connectors

Message Queue for Apache Kafka only provides sink connectors to export data from Message Queue for Apache Kafka to other Alibaba Cloud services. The following table lists the types of sink connectors that Message Queue for Apache Kafka supports.

|Sink connector type|Task type|Description|Documentation|
|-------------------|---------|-----------|-------------|
|FC Sink Connector|KAFKA2FC|Exports data from Message Queue for Apache Kafka to Function Compute.|[Create a Function Compute sink connector](/intl.en-US/User guide/Connectors/Create a connector/Create a Function Compute sink connector.md)|
|MaxCompute Sink Connector|KAFKA2ODPS|Exports data from Message Queue for Apache Kafka to MaxCompute.|[Create a MaxCompute sink connector](/intl.en-US/User guide/Connectors/Create a connector/Create a MaxCompute sink connector.md)|

## Procedure

Perform the following steps to use connectors:

1.  Enable the Connector feature.

    [Enable the Connector feature](/intl.en-US/User guide/Connectors/Enable the connector feature.md)

2.  Create a connector.
    -   [Create a MaxCompute sink connector](/intl.en-US/User guide/Connectors/Create a connector/Create a MaxCompute sink connector.md)
    -   [Create a Function Compute sink connector](/intl.en-US/User guide/Connectors/Create a connector/Create a Function Compute sink connector.md)
3.  Manage connectors.
    -   [View the task configurations of a connector](/intl.en-US/User guide/Connectors/View the task configurations of a connector.md)
    -   [View connector logs](/intl.en-US/User guide/Connectors/View connector logs.md)
    -   [Suspend a connector]()
    -   [Resume a connector]()
    -   [Delete a connector](/intl.en-US/User guide/Connectors/Delete a connector.md)
    -   [Modify the connector description](/intl.en-US/User guide/Connectors/Modify connector descriptions.md)

## Limits

The following table lists the limits of Message Queue for Apache Kafka on connectors.

|Item|Limit|
|----|-----|
|Quantity|3|
|Region|-   China \(Hangzhou\)
-   China \(Shanghai\)
-   China \(Beijing\)
-   China \(Zhangjiakou-Beijing Winter Olympics\)
-   China \(Hohhot\)
-   China \(Shenzhen\)
-   China \(Chengdu\)
-   China \(Hong Kong\)
-   Singapore \(Singapore\)
-   Japan \(Tokyo\) |

**Note:** To increase connectors for your instance or use connectors in more regions, submit a[ticket](https://workorder-intl.console.aliyun.com/#/ticket/add/?productId=1352) to Customer Service of Message Queue for Apache Kafka.

## Cross-region data synchronization

You must enable Internet access for a connector to synchronize data from the connector in one region to an Alibaba Cloud service in another region over the Internet. For more information, see [Enable Internet access for a connector](/intl.en-US/User guide/Connectors/Enable Internet access for a connector.md).

