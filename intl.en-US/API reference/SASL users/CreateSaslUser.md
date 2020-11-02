# CreateSaslUser

Creates a Simple Authentication and Security Layer \(SASL\) user.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=CreateSaslUser&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|CreateSaslUser|The operation that you want to perform. Set the value to

 **CreateSaslUser**. |
|InstanceId|String|Yes|alikafka\_pre-cn-v0h1cng0\*\*\*\*|The ID of the Message Queue for Apache Kafka instance. |
|Password|String|Yes|12\*\*\*|The password of the SASL user. |
|RegionId|String|Yes|cn-hangzhou|The ID of the region where the Message Queue for Apache Kafka instance is located. |
|Username|String|Yes|test\*\*\*|The username of the SASL user. |
|Type|String|No|plain|The type of the authentication mechanism. Valid values:

 -   **plain**
-   **scram**

 Default value: **plain**. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|Integer|200|The returned HTTP status code. A 200 status code indicates that the request succeeded. |
|Message|String|operation success.|The error message. |
|RequestId|String|C5CA600C-7D5A-45B5-B6DB-44FAC2C\*\*\*\*|The ID of the request. |
|Success|Boolean|true|Indicates whether the request was successful. |

## Examples

Sample request

```
http(s)://[Endpoint]/? Action=CreateSaslUser
&InstanceId=alikafka_pre-cn-v0h1cng0****
&Password=12***
&RegionId=cn-hangzhou
&Username=test***
&<Common request parameters>
```

Sample success responses

`XML` format

```
<CreateSaslUserResponse>
      <RequestId>C5CA600C-7D5A-45B5-B6DB-44FAC2C****</RequestId>
      <Message>operation success</Message>
      <Code>200</Code>
      <Success>true</Success>
</CreateSaslUserResponse>
```

`JSON` format

```
{
    "CreateSaslUserResponse": {
        "RequestId": "C5CA600C-7D5A-45B5-B6DB-44FAC2C****",
        "Message": "operation success",
        "Code": 200,
        "Success": true
    }
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

