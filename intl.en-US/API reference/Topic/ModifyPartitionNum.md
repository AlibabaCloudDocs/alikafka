# ModifyPartitionNum

Modifies the number of partitions for a topic.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=ModifyPartitionNum&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|ModifyPartitionNum|The operation that you want to perform. Set the value to

**ModifyPartitionNum**. |
|InstanceId|String|Yes|alikafka\_post-cn-0pp1l9z8\*\*\*|The ID of the Message Queue for Apache Kafka instance. |
|RegionId|String|Yes|cn-hangzhou|The ID of the region where the Message Queue for Apache Kafka instance is located. |
|Topic|String|Yes|TopicPartitionNum|The name of the topic for which you want to add partitions. |
|AddPartitionNum|Integer|Yes|6|The number of partitions you want to add for the topic.

-   We recommend that you set the number of partitions to a multiple of 6 to reduce the risk of data skew.
-   A maximum of 48 partitions are allowed for a topic.
-   If you need to increase the quota, submit a ticket. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|Integer|200|The returned HTTP status code. A 200 status code indicates that the request succeeded. |
|Message|String|200|The returned message. |
|RequestId|String|B7A39AE5-0B36-4442-A304-E0885265\*\*\*|The ID of the request. |
|Success|Boolean|true|Indicates whether the request was successful. |

## Examples

Sample request

```
http(s)://[Endpoint]/? Action=ModifyPartitionNum
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
    "ModifyPartitionNumResponse": {
        "RequestId": "B7A39AE5-0B36-4442-A304-E0885265***",
        "Message": "operation success",
        "Code": 200,
        "Success": true
    }
}
```

## Error codes

|HttpCode|Error code|Error message|Description|
|--------|----------|-------------|-----------|
|500|InternalError|An internal error occurred; please try again later.|The error message returned because an internal error has occurred. Try again later.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

