# ModifyInstanceName

Changes the name of a Message Queue for Apache Kafka instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=ModifyInstanceName&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|ModifyInstanceName|The operation that you want to perform. Set the value to

 **ModifyInstanceName**. |
|InstanceId|String|Yes|alikafka\_post-cn-v0h1fgs2\*\*\*\*|The ID of the Message Queue for Apache Kafka instance whose name you want to change. |
|InstanceName|String|Yes|dev-test|The name of the instance. Take note of the following rules when you specify an instance name:

 -   The name can contain only letters, digits, hyphens \(-\), and underscores \(\_\).
-   The name must be 3 to 64 characters in length. Names that contain more than 64 characters will be automatically truncated. |
|RegionId|String|Yes|cn-hangzhou|The ID of the region where the instance resides. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|Integer|200|The HTTP status code. If 200 is returned, the request is successful. |
|Message|String|operation success.|The response message. |
|RequestId|String|06084011-E093-46F3-A51F-4B19A8AD\*\*\*\*|The ID of the request. |
|Success|Boolean|true|Indicates whether the request is successful. |

## Examples

Sample requests

```
http(s)://[Endpoint]/?Action=ModifyInstanceName
&InstanceId=alikafka_post-cn-v0h1fgs2****
&InstanceName=dev-test
&RegionId=cn-hangzhou
&<Common request parameters>
```

Sample success responses

`XML` format

```
<ModifyInstanceNameResponse>
      <Message>operation success.</Message>
      <RequestId>06084011-E093-46F3-A51F-4B19A8AD7A94</RequestId>
      <Success>true</Success>
      <Code>200</Code>
</ModifyInstanceNameResponse>
```

`JSON` format

```
{
    "RequestId":"06084011-E093-46F3-A51F-4B19A8AD****",
    "Message":"operation success.",
    "Code":"200",
    "Success":"true"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

