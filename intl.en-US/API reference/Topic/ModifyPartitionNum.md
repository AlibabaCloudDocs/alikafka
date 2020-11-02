# ModifyPartitionNum

Modifies the number of partitions for a topic.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=ModifyPartitionNum&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|ModifyPartitionNum|The operation that you want to perform. Set the value to

 **ModifyPartitionNum**. |
|AddPartitionNum|Integer|Yes|6|The number of partitions that you want to add to the topic.

 -   The value must be greater than 0.
-   To reduce the risk of data skew, we recommend that you set the number of partitions to a multiple of 6.
-   Valid values: 1 to 360.
-   If you require more than 360 partitions, [submit a ticket](https://workorder-intl.console.aliyun.com/#/ticket/add/?productId=1352). |
|InstanceId|String|Yes|alikafka\_post-cn-0pp1l9z8\*\*\*|The ID of the Message Queue for Apache Kafka instance that contains the topic. |
|RegionId|String|Yes|cn-hangzhou|The ID of the region where the instance is located. |
|Topic|String|Yes|TopicPartitionNum|The name of the topic. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|Integer|200|The response code. The HTTP 200 code indicates that the request was successful. |
|Message|String|operation success|The response message. |
|RequestId|String|B7A39AE5-0B36-4442-A304-E0885265\*\*\*|The ID of the request. |
|Success|Boolean|true|Indicates whether the request was successful. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=ModifyPartitionNum
&AddPartitionNum=6
&InstanceId=alikafka_post-cn-0pp1l9z8***
&RegionId=cn-hangzhou
&Topic=TopicPartitionNum
&<Common request parameters>
```

Sample success responses

`XML` format

```
<ModifyPartitionNumResponse>
      <RequestId>B7A39AE5-0B36-4442-A304-E0885265***</RequestId>
      <Message>operation success</Message>
      <Code>200</Code>
      <Success>true</Success>
</ModifyPartitionNumResponse>
```

`JSON` format

```
{
    "RequestId": "B7A39AE5-0B36-4442-A304-E0885265***",
    "Message": "operation success",
    "Code": 200,
    "Success": true
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

