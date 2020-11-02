# DeleteTopic

Deletes a topic.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=DeleteTopic&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description |
|---------|----|--------|-------|------------|
|Action|String|Yes|DeleteTopic|The operation that you want to perform. Set the value to

 **DeleteTopic**. |
|InstanceId|String|Yes|alikafka\_post-cn-v0h1fgs2\*\*\*\*|The ID of the Message Queue for Apache Kafka instance from which you want to delete the topic. |
|RegionId|String|Yes|cn-hangzhou|The ID of the region where the instance is located. |
|Topic|String|Yes|test|The name of the topic that you want to delete. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|Integer|200|The response code. The HTTP 200 code indicates that the request was successful. |
|Message|String|operation success.|The response message. |
|RequestId|String|06084011-E093-46F3-A51F-4B19A8AD\*\*\*\*|The ID of the request. |
|Success|Boolean|true|Indicates whether the request was successful. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=DeleteTopic
&InstanceId=alikafka_post-cn-v0h1fgs2****
&RegionId=cn-hangzhou
&Topic=test
&<Common request parameters>
```

Sample success responses

`XML` format

```
<DeleteTopic>
      <RequestId>06084011-E093-46F3-A51F-4B19A8AD****</RequestId>
      <Message>operation success. </Message>
      <Code>200</Code>
      <Success>true</Success>
</DeleteTopic>
```

`JSON` format

```
{
    "RequestId": "06084011-E093-46F3-A51F-4B19A8AD****",
    "Message": "operation success.",
    "Code": 200,
    "Success": true
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

