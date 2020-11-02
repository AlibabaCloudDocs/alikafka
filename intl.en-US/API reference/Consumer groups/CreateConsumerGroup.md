# CreateConsumerGroup

Creates a consumer group.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=CreateConsumerGroup&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description |
|---------|----|--------|-------|------------|
|Action|String|Yes|CreateConsumerGroup|The operation that you want to perform. Set the value to

 **CreateConsumerGroup**. |
|ConsumerId|String|Yes|test|The name of the consumer group. Valid values:

 -   The name can contain only letters, digits, hyphens \(-\), and underscores \(\_\).
-   The name must be 3 to 64 characters in length. Names that contain more than 64 characters will be automatically truncated.
-   After a consumer group is created, its name cannot be modified. |
|InstanceId|String|Yes|alikafka\_post-cn-0pp1l9z8\*\*\*\*|The ID of the Message Queue for Apache Kafka instance where you want to create the consumer group. |
|RegionId|String|Yes|cn-hangzhou|The ID of the region where the instance is located. |
|Remark|String|No|test|The description of the consumer group. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|Integer|200|The response code. The HTTP 200 code indicates that the request was successful. |
|Message|String|operation success|The response message. |
|RequestId|String|E57A8862-DF68-4055-8E55-B80CB417\*\*|The ID of the request. |
|Success|Boolean|true|Indicates whether the request was successful. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=CreateConsumerGroup
&ConsumerId=test
&InstanceId=alikafka_post-cn-0pp1l9z8****
&RegionId=cn-hangzhou
&<Common request parameters>
```

Sample success responses

`XML` format

```
<CreateConsumerGroupResponse>
      <RequestId>E57A8862-DF68-4055-8E55-B80CB417**</RequestId>
      <Message>operation success</Message>
      <Code>200</Code>
      <Success>true</Success>
</CreateConsumerGroupResponse>
```

`JSON` format

```
{
    "RequestId": "E57A8862-DF68-4055-8E55-B80CB417**",
    "Message": "operation success",
    "Code": 200,
    "Success": true
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

