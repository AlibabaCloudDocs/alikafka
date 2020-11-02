# DeleteInstance

You can call this operation to delete a Message Queue for Apache Kafka instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=DeleteInstance&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|DeleteInstance|The operation that you want to perform. Set the value to DeleteInstance.|
|InstanceId|String|Yes|alikafka\_post-cn-mp919o4v\*\*\*\*|The ID of the Message Queue for Apache Kafka instance to be deleted. |
|RegionId|String|Yes|cn-hangzhou|The ID of the region where the Message Queue for Apache Kafka instance is to be deleted. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|Integer|200|The returned status code. If "200" is returned, the request is successful. |
|Message|String|operation success.|The returned message. |
|RequestId|String|ABA4A7FD-E10F-45C7-9774-A5236015\*\*\*\*|The ID of the request. |
|Success|Boolean|true|Indicates whether the request is successful. |

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
<Message>operation success. </Message>
<RequestId>ABA4A7FD-E10F-45C7-9774-A523601****</RequestId>
<Success>true</Success>
<Code>200</Code>
```

`JSON` format

```
{
    "Message":"operation success.",
    "RequestId":"ABA4A7FD-E10F-45C7-9774-A523601****",
    "Success":true,
    "Code":200
}
```

## Error codes

|HTTP status code|Error code|Error message|Description|
|----------------|----------|-------------|-----------|
|500|InternalError|An internal error occurred; please try again later.|The error message returned because an internal error has occurred. Try again later.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

