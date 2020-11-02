# Cloud Monitor resource reports

You can call Cloud Monitor resource report operations to query the usage of Message Queue for Apache Kafka instances, consumer groups, and topics.

## List of operations by function

Cloud Monitor provides the following API operations.

|API|Description|
|---|-----------|
|[DescribeProjectMeta](/intl.en-US/API Reference/Monitoring data on time series metrics of cloud services/DescribeProjectMeta.md)|Queries information about a cloud service, including its description, namespace, and tags. **Note:** For more information about Message Queue for Apache Kafka, see [Message Queue for Apache Kafka information](#section_qf5_2kw_foc). |
|[DescribeMetricMetaList](/intl.en-US/API Reference/Monitoring data on time series metrics of cloud services/DescribeMetricMetaList.md)|Queries the descriptions of time series metrics that are supported in Cloud Monitor. **Note:** For information about Message Queue for Apache Kafka monitoring items, see [.](#section_i2n_kxu_nny) |
|[DescribeMetricLast](/intl.en-US/API Reference/Monitoring data on time series metrics of cloud services/DescribeMetricLast.md)|Queries the latest monitoring data of a metric.|
|[DescribeMetricList](/intl.en-US/API Reference/Monitoring data on time series metrics of cloud services/DescribeMetricList.md)|Queries the monitoring data on a time series metric of a cloud service in a specified period.|
|[DescribeMetricTop](/intl.en-US/API Reference/Monitoring data on time series metrics of cloud services/DescribeMetricTop.md)|Queries the monitoring data on a time series metric of a cloud service in a specified period.|
|[DescribeMetricData](/intl.en-US/API Reference/Monitoring data on time series metrics of cloud services/DescribeMetricData.md)|Queries the monitoring data of a time series metric.|

## RAM user authorization

For more information about how to grant Cloud Monitor permissions to Resource Access Management \(RAM\) users, see [Control permissions of RAM users](/intl.en-US/Appendix 3 account authorization/Control permissions of RAM users.md).

## Make API requests

For more information about how to call Cloud Monitor resource report operations, see [.](/intl.en-US/API Reference/Request method.md)

## Message Queue for Apache Kafka information

Cloud Monitor resource reports provide information about Message Queue for Apache Kafka resources in the following format:

```
{
    "PageSize": 30,
    "RequestId": "5AD2B98E-0E93-46FB-9790-185F338254FF",
    "PageNumber": 1,
    "Total": 1,
    "Resources": {
        "Resource": [
            {
                "Description": "Message Queue for Apache Kafka"
                "Labels": "[{\"name\":\"product\",\"value\":\"Kafka\"},{\"name\":\"productCategory\",\"value\":\"kafka\"},{\"name\":\"groupFlag\",\"value\":\"false\"},{\"name\":\"cnName\",\"value\":\"Message Queue for Apache Kafka\"},{\"name\":\"enName\",\"value\":\"MQ for Kafka\"}]",
                "Namespace": "acs_kafka"
            }
        ]
    },
    "Code": 200,
    "Success": true
}
```

## Monitoring metrics

The following table lists Message Queue for Apache Kafka monitoring metrics provided by Cloud Monitor.

|Metric|Description|Unit|Dimension|
|------|-----------|----|---------|
|instance\_disk\_capacity|The disk usage of a Message Queue for Apache Kafka instance.|%|instanceId|
|instance\_message\_input|The production traffic of a Message Queue for Apache Kafka instance.|bytes/s|instanceId|
|instance\_message\_output|The consumption traffic of a Message Queue for Apache Kafka instance.|bytes/s|instanceId|
|message\_accumulation|The total number of unconsumed messages in a consumer group.|Count|-   instanceId
-   consumerGroup |
|message\_accumulation\_onetopic|The number of unconsumed messages in a single topic for a consumer group.|Count|-   instanceId
-   consumerGroup
-   topic |
|topic\_message\_input|The production traffic of messages in a topic.|bytes/s|-   instanceId
-   topic |
|topic\_message\_output|The consumption traffic of messages in a topic.|bytes/s|-   instanceId
-   topic |

