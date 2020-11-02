# ModifyTopicRemark

Modifies the description of a topic.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=ModifyTopicRemark&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|ModifyTopicRemark|The operation that you want to perform. Set the value to

 **ModifyTopicRemark**. |
|InstanceId|String|Yes|alikafka\_post-cn-0pp1l9z8\*\*\*|The ID of the Message Queue for Apache Kafka instance that contains the topic. |
|RegionId|String|Yes|cn-hangzhou|The ID of the region where the instance is located. |
|Topic|String|Yes|alikafka\_post-cn-0pp1l9z8z\*\*\*|The name of the topic whose description you want to change. |
|Remark|String|No|testremark|The description of the topic. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|Integer|200|The response code. The HTTP 200 code indicates that the request was successful. |
|Message|String|operation success|The response message. |
|RequestId|String|DB6F1BEA-903B-4FD8-8809-46E7E9CE\*\*\*|The ID of the request. |
|Success|Boolean|true|Indicates whether the request was successful. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=ModifyTopicRemark
&InstanceId=alikafka_post-cn-0pp1l9z8***
&RegionId=cn-hangzhou
&Topic=alikafka_post-cn-0pp1l9z8z***
&<Common request parameters>
```

Sample success responses

`XML` format

```
<ModifyTopicRemarkResponse>
      <RequestId>DB6F1BEA-903B-4FD8-8809-46E7E9CE***</RequestId>
      <Message>operation success</Message>
      <Code>200</Code>
      <Success>true</Success>
</ModifyTopicRemarkResponse>
```

`JSON` format

```
{
    "RequestId":"DB6F1BEA-903B-4FD8-8809-46E7E9CE***",
    "Message":"operation success",
    "Code":"200",
    "Success":"true"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

