---
keyword: [release note, kafka, description]
---

# Release notes

This topic describes the new and optimized features in each release of Message Queue for Apache Kafka and provides references.

## 2021-03-24

|Feature|Description|Type|References|
|-------|-----------|----|----------|
|Elasticsearch sink connector|Elasticsearch sink connectors can be used to synchronize data from Message Queue for Apache Kafka to Elasticsearch.|New|[Create an Elasticsearch sink connector](/intl.en-US/User guide/Connectors/Create a connector/Create an Elasticsearch sink connector.md)|

## 2021-03-15

|Feature|Description|Type|References|
|-------|-----------|----|----------|
|MySQL source connector|MySQL source connectors can be used to synchronize data from ApsaraDB RDS for MySQL to Message Queue for Apache Kafka. **Note:** This feature is available in the China \(Shenzhen\), China \(Chengdu\), China \(Beijing\), China \(Hangzhou\), and China \(Shanghai\) regions.

|New|[Create a MySQL source connector](/intl.en-US/User guide/Connectors/Create a connector/Create a MySQL source connector.md)|

## 2021-03-03

|Feature|Description|Type|References|
|-------|-----------|----|----------|
|OSS sink connector|OSS sink connectors are used to synchronize data from Message Queue for Apache Kafka to Object Storage Service \(OSS\).|New|[Create an OSS sink connector](/intl.en-US/User guide/Connectors/Create a connector/Create an OSS sink connector.md)|

## 2020-12-14

|Feature|Description|Type|References|
|-------|-----------|----|----------|
|Connector|The number of concurrent consumer threads can be set for a Function Compute sink connector.|Optimized|[Create a Function Compute sink connector](/intl.en-US/User guide/Connectors/Create a connector/Create a Function Compute sink connector.md)|

## 2020-11-30

|Feature|Description|Type|References|
|-------|-----------|----|----------|
|Service-linked role|-   The service-linked role AliyunServiceRoleForAlikafkaConnector is added to implement the Function Compute sink connector feature.
-   The service-linked role AliyunServiceRoleForAlikafkaConnector is automatically created when you create a Function Compute sink connector in the Message Queue for Apache Kafka console.

|New|-   [Service-linked roles](/intl.en-US/Access control/Service-linked roles.md)
-   [Create a Function Compute sink connector](/intl.en-US/User guide/Connectors/Create a connector/Create a Function Compute sink connector.md) |
|Connector|Connectors can be tested in the Message Queue for Apache Kafka console.|New|[Test a connector](/intl.en-US/User guide/Connectors/Test a connector.md)|

## 2020-11-18

|Feature|Description|Type|References|
|-------|-----------|----|----------|
|Connector|-   Function Compute sink connector:
    -   The number of retries after a message fails to be sent can be specified.
    -   The fail and log policies are provided to handle messages that fail to be sent.
-   MaxCompute sink connector:
    -   AccessKey authentication is no longer supported.
    -   Security Token Service \(STS\) authentication is supported.
    -   Message modes such as DEFAULT, VALUE, and KEY are supported.
    -   Message formats such as TEXT, BINARY, and CSV are supported.
    -   Data granularity levels such as DAY, HOUR, and MINUTE are supported.
    -   The time zone of a Message Queue for Apache Kafka producer client can be specified.

|Optimized|-   [Create a Function Compute sink connector](/intl.en-US/User guide/Connectors/Create a connector/Create a Function Compute sink connector.md)
-   [Create a MaxCompute sink connector](/intl.en-US/User guide/Connectors/Create a connector/Create a MaxCompute sink connector.md) |
|ACL|The access control list \(ACL\) feature can be enabled in the Message Queue for Apache Kafka console. You no longer need to submit a ticket.|Optimized|[Authorize SASL users](/intl.en-US/Access control/Authorize SASL users.md)|

## 2020-10-22

|Feature|Description|Type|References|
|-------|-----------|----|----------|
|Connector|-   The connector feature can be enabled in the Message Queue for Apache Kafka console.
-   The connector feature is available in the China \(Zhangjiakou\), China \(Hohhot\), and China \(Hong Kong\) regions.
-   The connector feature can be enabled for Message Queue for Apache Kafka instances whose major version is 0.10.2 and minor version is the latest.
-   Topics and consumer groups required by a connector can be automatically created and deleted.
-   Connectors can be suspended and resumed.
-   A newly created connector enters the running state only after you manually deploy the connector.
-   The interaction design for connectors in the Message Queue for Apache Kafka console is optimized.

|Optimized|-   [Overview](/intl.en-US/User guide/Connectors/Overview.md)
-   [Enable the connector feature](/intl.en-US/User guide/Connectors/Enable the connector feature.md)
-   [Create a Function Compute sink connector](/intl.en-US/User guide/Connectors/Create a connector/Create a Function Compute sink connector.md)
-   [Create a MaxCompute sink connector](/intl.en-US/User guide/Connectors/Create a connector/Create a MaxCompute sink connector.md)
-   [Suspend a connector](/intl.en-US/User guide/Connectors/Suspend a connector.md)
-   [Resume a connector](/intl.en-US/User guide/Connectors/Resume a connector.md) |

## 2020-08-20

|Feature|Description|Type|References|
|-------|-----------|----|----------|
|GetConsumerList|The Remark parameter is added to the response parameters.|Optimized|[GetConsumerList](/intl.en-US/API reference/Consumer groups/GetConsumerList.md)|

## 2020-07-29

|Feature|Description|Type|References|
|-------|-----------|----|----------|
|Specification evaluation|Appropriate editions of Message Queue for Apache Kafka instances are recommended based on the specifications of self-managed Apache Kafka clusters.|New|[Evaluate specifications](/intl.en-US/User guide/Migration/Evaluate specifications.md)|
|Display of the migration progress|The migration progress of a self-managed Apache Kafka cluster can be viewed.|New|[View the migration progress](/intl.en-US/User guide/Migration/View the migration progress.md)|

## 2020-07-22

|Feature|Description|Type|References|
|-------|-----------|----|----------|
|GetInstanceList|New response parameters are added to indicate the IDs of zones for multi-zone deployment and the ID of the custom security group.|Optimized|[GetInstanceList](/intl.en-US/API reference/Instances/GetInstanceList.md)|

## 2020-07-15

|Feature|Description|Type|References|
|-------|-----------|----|----------|
|Security group configuration with Terraform|Terraform can be used to configure a security group when you create a Message Queue for Apache Kafka instance.|New|[alicloud\_alikafka\_instance](https://www.terraform.io/docs/providers/alicloud/r/alikafka_instance.html)|

## 2020-06-19

|Feature|Description|Type|References|
|-------|-----------|----|----------|
|Connector|Message Queue for Apache Kafka can be connected to other Alibaba Cloud services to synchronize data with the services.|New|[Overview](/intl.en-US/User guide/Connectors/Overview.md)|

## 2020-05-20

|Feature|Description|Type|References|
|-------|-----------|----|----------|
|Change of version names|The open source version is changed to the major version, and the internal version is changed to the minor version. The major version corresponds to the open source version, and the minor version is an optimized internal version of the major version.|Optimized|[Upgrade the instance version](/intl.en-US/User guide/Instances/Upgrade the instance version.md)|
|Display of the upgrade progress|The remaining time and progress of an upgrade can be viewed after an instance upgrade is initiated.|New|[Upgrade the instance version](/intl.en-US/User guide/Instances/Upgrade the instance version.md)|
|Display of topics that you can create|The number of topics that you can create can be viewed. -   Professional Edition instances: The number of topics that you can create is twice the number of topics that you purchase. For example, if you purchase 50 topics for a Professional Edition instance, you can create 100 topics.
-   Standard Edition instances: The number of topics that you can create equals the number of topics that you purchase. For example, if you purchase 50 topics for a Standard Edition instance, you can create 50 topics.

|New|None|
|Display of task execution records|Execution records can be viewed for the following tasks: upgrading instance versions, modifying message configurations, upgrading instance specifications, and enabling the ACL feature.|New|[View task execution records](/intl.en-US/User guide/Instances/View task execution records.md)|

## 2020-05-13

|Feature|Description|Type|References|
|-------|-----------|----|----------|
|Scale-out|Cluster scale-out is supported. When you upgrade the bandwidth specification of an instance, the corresponding cluster may be scaled out. After the cluster is scaled out, you must rebalance the topic traffic to evenly distribute the traffic across nodes in the cluster.|New|-   [Upgrade instance specifications](/intl.en-US/User guide/Instances/Upgrade instance specifications.md)
-   [Rebalance the topic traffic](/intl.en-US/User guide/Instances/Rebalance the topic traffic.md) |

## 2020-05-12

|Feature|Description|Type|References|
|-------|-----------|----|----------|
|Service availability in the China \(Heyuan\) and China \(Chengdu\) regions|Message Queue for Apache Kafka is available in the China \(Heyuan\) and China \(Chengdu\) regions.|New|None|

## 2020-04-08

|Feature|Description|Type|References|
|-------|-----------|----|----------|
|ACL|The ACL feature can be enabled to authorize Simple Authentication and Security Layer \(SASL\) users.|New|[Authorize SASL users](/intl.en-US/Access control/Authorize SASL users.md)|

## 2020-04-02

|Feature|Description|Type|References|
|-------|-----------|----|----------|
|Prolonged message retention period|The maximum retention period of a message on a Message Queue for Apache Kafka broker is increased from 168 hours to 480 hours.|Optimized|[Modify the message configuration](/intl.en-US/User guide/Instances/Modify the message configuration.md)|

## 2020-03-02

|Feature|Description|Type|References|
|-------|-----------|----|----------|
|GetAllowedIpList|The GetAllowedIpList operation can be called to query an IP address whitelist.|New|[GetAllowedIpList](/intl.en-US/API reference/Instances/GetAllowedIpList.md)|
|UpdateAllowedIp|The UpdateAllowedIp operation can be called to update an IP address whitelist.|New|[UpdateAllowedIp](/intl.en-US/API reference/Instances/UpdateAllowedIp.md)|

