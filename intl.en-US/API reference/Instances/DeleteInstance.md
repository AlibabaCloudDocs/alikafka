# DeleteInstance

Deletes a Message Queue for Apache Kafka instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=DeleteInstance&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|DeleteInstance|The operation that you want to perform. Set the value to

 **DeleteInstance**. |
|InstanceId|String|Yes|alikafka\_post-cn-mp919o4v\*\*\*\*|The ID of the instance that you want to delete. |
|RegionId|String|Yes|cn-hangzhou|The ID of the region where the instance is located. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|Integer|200|The response code. The HTTP 200 code indicates that the request was successful. |
|Message|String|operation success.|The response message. |
|RequestId|String|ABA4A7FD-E10F-45C7-9774-A5236015\*\*\*\*|The ID of the request. |
|Success|Boolean|true|Indicates whether the request was successful. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=DeleteInstance
&InstanceId=alikafka_post-cn-mp919o4v****
&RegionId=cn-hangzhou
&<Common request parameters>
```

Sample success responses

`XML` format

```
<DeleteInstanceResponse>
      <Message>operation success. </Message>
      <RequestId>ABA4A7FD-E10F-45C7-9774-A5236015****</RequestId>
      <Success>true</Success>
      <Code>200</Code>
</DeleteInstanceResponse>
```

`JSON` format

```
{
    "RequestId":"ABA4A7FD-E10F-45C7-9774-A5236015****",
    "Message":"operation success.",
    "Code":"200",
    "Success":"true"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

