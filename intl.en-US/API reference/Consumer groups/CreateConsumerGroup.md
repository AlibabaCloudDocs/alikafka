# CreateConsumerGroup

You can call this operation to create a consumer group.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=CreateConsumerGroup&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|CreateConsumerGroup|The operation that you want to perform. Set the value to CreateConsumerGroup. |
|ConsumerId|String|Yes|consumer\_group\_test|The name of the consumer group. The value of this parameter must meet the following requirements:

 -   The name can only contain letters, digits, hyphens \(-\), and underscores \(\_\).
-   The name must be 3 to 64 characters in length, and will be automatically truncated if it contains more characters.
-   The name cannot be modified after being created. |
|InstanceId|String|Yes|alikafka\_pre-cn-0pp1954n\*\*\*\*|The ID of the Message Queue for Apache Kafka instance where the consumer group is located. |
|RegionId|String|Yes|cn-hangzhou|The region ID of the Message Queue for Apache Kafka instance where the consumer group is located. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|Integer|200|The returned status code. If "**200**" is returned, the request is successful. |
|Message|String|operation success|The returned message. |
|RequestId|String|B191CC4D-B067-4508-987A-ACDA8D89\*\*\*\*|The ID of the request. |
|Success|Boolean|true|Indicates whether the request is successful. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=CreateConsumerGroup
&ConsumerId=consumer_group_test
&InstanceId=alikafka_pre-cn-0pp1954n****
&RegionId=cn-hangzhou
&<Common request parameters>
```

Sample success responses

`XML` format

```
<Message>operation success</Message>
<RequestId>B191CC4D-B067-4508-987A-ACDA8D89****</RequestId>
<Success>true</Success>
<Code>200</Code>
```

`JSON` format

```
{
    "Message":"operation success",
    "RequestId":"B191CC4D-B067-4508-987A-ACDA8D89****",
    "Success":true,
    "Code":200
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

