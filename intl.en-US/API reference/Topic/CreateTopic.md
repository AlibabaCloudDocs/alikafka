# CreateTopic

Creates a topic.

-   You can send a maximum of one query per second \(QPS\).
-   The maximum number of topics that can be created on an instance depends on the specifications of the instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=CreateTopic&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|CreateTopic|The operation that you want to perform. Set the value to

**CreateTopic**. |
|InstanceId|String|Yes|alikafka\_pre-cn-mp919o4v\*\*\*\*|The ID of the Message Queue for Apache Kafka instance on which you want to create the topic. |
|RegionId|String|Yes|cn-hangzhou|The region ID of the Message Queue for Apache Kafka instance. |
|Remark|String|Yes|alikafka\_topic\_test|The description of the topic.

-   The description can contain only letters, digits, hyphens \(-\), and underscores \(\_\).
-   The description must be 3 to 64 characters in length. |
|Topic|String|Yes|alikafka\_topic\_test|The name of the topic.

-   The name can contain only letters, digits, hyphens \(-\), and underscores \(\_\).
-   The name must be 3 to 64 characters in length. Names that contain more than 64 characters are automatically truncated.
-   The name cannot be modified after the topic is created. |
|CompactTopic|Boolean|No|false|The log cleanup policy for the topic. This parameter is available when the Local Storage mode is specified for the topic. Valid values:

-   false: uses the default log cleanup policy.
-   true: uses the Apache Kafka log compaction policy. |
|PartitionNum|String|No|12|The number of partitions in the topic.

-   Valid values: 1 to 360.
-   To reduce the risk of data skew, we recommend that you set the number of partitions to a multiple of 6.
-   If you require more than 360 partitions, submit a ticket. |
|LocalTopic|Boolean|No|false|The storage engine of the topic. Valid values:

-   false: the Cloud Storage mode.
-   true: the Local Storage mode. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|Integer|200|The HTTP status code. The HTTP status code 200 indicates that the request is successful. |
|Message|String|operation success|The returned message. |
|RequestId|String|9C0F207C-77A6-43E5-991C-9D98510A\*\*\*\*|The ID of the request. |
|Success|Boolean|true|Indicates whether the request is successful. |

## Examples

Sample requests

```
http(s)://[Endpoint]/?Action=CreateTopic
&InstanceId=alikafka_pre-cn-mp919o4v****
&RegionId=cn-hangzhou
&Remark=alikafka_topic_test
&Topic=alikafka_topic_test
&<Common request parameters>
```

Sample success responses

`XML` format

```
<CreateTopicResponse>
      <RequestId>9C0F207C-77A6-43E5-991C-9D98510A****</RequestId>
      <Message>operation success</Message>
      <Code>200</Code>
      <Success>true</Success>
</CreateTopicResponse>
```

`JSON` format

```
{
    "RequestId": "9C0F207C-77A6-43E5-991C-9D98510A****",
    "Message": "operation success",
    "Code": 200,
    "Success": true
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

