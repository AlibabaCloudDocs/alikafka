# DeleteSaslUser

Deletes a Simple Authentication and Security Layer \(SASL\) user.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=DeleteSaslUser&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description |
|---------|----|--------|-------|------------|
|Action|String|Yes|DeleteSaslUser|The operation that you want to perform. Set the value to

 **DeleteSaslUser**. |
|InstanceId|String|Yes|alikafka\_pre-cn-v0h1cng0\*\*\*\*|The ID of the Message Queue for Apache Kafka instance that contains the SASL user. |
|RegionId|String|Yes|cn-hangzhou|The ID of the region where the instance is located. |
|Username|String|Yes|test\*\*\*|The name of the SASL user that you want to delete. |
|Type|String|No|scram|The authentication mechanism. Valid values:

 -   **plain**: a simple username and password verification mechanism. Message Queue for Apache Kafka provides an improved PLAIN mechanism that allows you to dynamically add SASL users without restarting the instance.
-   **SCRAM**: a username and password verification mechanism with higher security than PLAIN. Message Queue for Apache Kafka uses SCRAM-SHA-256.

 Default value: **plain**. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|Integer|200|The response code. The HTTP 200 code indicates that the request was successful. |
|Message|String|operation success|The response message. |
|RequestId|String|3CB89F5C-CD97-4C1D-BC7C-FEDEC2F4\*\*\*\*|The ID of the request. |
|Success|Boolean|true|Indicates whether the request was successful. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=DeleteSaslUser
&InstanceId=alikafka_pre-cn-v0h1cng0****
&RegionId=cn-hangzhou
&Username=test***
&Type=scram
&<Common request parameters>
```

Sample success responses

`XML` format

```
<DeleteSaslUserResponse>
      <RequestId>3CB89F5C-CD97-4C1D-BC7C-FEDEC2F4****</RequestId>
      <Message>operation success</Message>
      <Code>200</Code>
      <Success>true</Success>
</DeleteSaslUserResponse>
```

`JSON` format

```
{
    "RequestId": "3CB89F5C-CD97-4C1D-BC7C-FEDEC2F4****",
    "Message": "operation success",
    "Code": 200,
    "Success": true
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

