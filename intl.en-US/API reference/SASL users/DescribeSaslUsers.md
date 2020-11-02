# DescribeSaslUsers

Queries Simple Authentication and Security Layer \(SASL\) users.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=DescribeSaslUsers&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|DescribeSaslUsers|The operation that you want to perform. Set the value to

 **DescribeSaslUsers**. |
|InstanceId|String|Yes|alikafka\_pre-cn-v0h1cng0\*\*\*\*|The ID of the Message Queue for Apache Kafka instance. |
|RegionId|String|Yes|cn-hangzhou|The ID of the region where the Message Queue for Apache Kafka instance is located. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|Integer|200|The returned HTTP status code. A 200 status code indicates that the request succeeded. |
|Message|String|operation success.|The returned message. |
|RequestId|String|9E3B3592-5994-4F65-A61E-E62A77A7\*\*\*|The ID of the request. |
|SaslUserList|Array| |The list of returned SASL users. |
|SaslUserVO| | | |
|Password|String|123\*\*\*|The password of the SASL user. |
|Type|String|scram|The type of the authentication mechanism. |
|Username|String|test12\*\*\*|The username of the SASL user. |
|Success|Boolean|true|Indicates whether the request was successful. |

## Examples

Sample request

```
http(s)://[Endpoint]/? Action=DescribeSaslUsers
&InstanceId=alikafka_pre-cn-v0h1cng0****
&RegionId=cn-hangzhou
&<Common request parameters>
```

Sample success responses

`XML` format

```
<DescribeSaslUsersResponse>
      <SaslUserList>
            <SaslUserVO>
                  <Username>test12***</Username>
                  <Password>123***</Password>
            </SaslUserVO>
      </SaslUserList>
      <RequestId>9E3B3592-5994-4F65-A61E-E62A77A7***</RequestId>
      <Message>operation success. </Message>
      <Code>200</Code>
      <Success>true</Success>
</DescribeSaslUsersResponse>
```

`JSON` format

```
{
    "DescribeSaslUsersResponse": {
        "SaslUserList": {
            "SaslUserVO": {
                "Username": "test12***",
                "Password": "123***"
            }
        },
        "RequestId": "9E3B3592-5994-4F65-A61E-E62A77A7***",
        "Message": "operation success.",
        "Code": 200,
        "Success": true
    }
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

