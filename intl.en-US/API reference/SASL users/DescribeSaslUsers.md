# DescribeSaslUsers

Queries Simple Authentication and Security Layer \(SASL\) users.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=DescribeSaslUsers&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|DescribeSaslUsers|The operation that you want to perform. Set the value to

 **DescribeSaslUsers**. |
|InstanceId|String|Yes|alikafka\_pre-cn-v0h1cng0\*\*\*\*|The ID of the Message Queue for Apache Kafka instance whose SASL users you want to query. |
|RegionId|String|Yes|cn-hangzhou|The ID of the region where the instance resides. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|Integer|200|The HTTP status code. If 200 is returned, the request is successful. |
|Message|String|operation success.|The response message. |
|RequestId|String|9E3B3592-5994-4F65-A61E-E62A77A7\*\*\*|The ID of the request. |
|SaslUserList|Array of SaslUserVO| |The list of SASL users. |
|SaslUserVO| | | |
|Password|String|123\*\*\*|The password of the user. |
|Type|String|scram|The authentication mechanism of the user. Valid values:

 -   **plain**: a simple username and password verification mechanism. Message Queue for Apache Kafka provides an improved PLAIN mechanism that allows you to dynamically add SASL users without restarting the instance.
-   **scram**: a username and password verification mechanism with higher security than PLAIN. Message Queue for Apache Kafka uses SCRAM-SHA-256.

 Default value: **plain**. |
|Username|String|test12\*\*\*|The name of the SASL user. |
|Success|Boolean|true|Indicates whether the request is successful. |

## Examples

Sample requests

```
http(s)://[Endpoint]/?Action=DescribeSaslUsers
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
                  <Type>scram</Type>
                  <Username>test12***</Username>
                  <Password>123***</Password>
            </SaslUserVO>
      </SaslUserList>
      <RequestId>9E3B3592-5994-4F65-A61E-E62A77A7***</RequestId>
      <Message>operation success.</Message>
      <Code>200</Code>
      <Success>true</Success>
</DescribeSaslUsersResponse>
```

`JSON` format

```
{
    "SaslUserList": {
        "SaslUserVO": [
        {
            "Type": "scram",
            "Username": "test12***",
            "Password": "123***"
        }
    ]
    },
    "RequestId": "9E3B3592-5994-4F65-A61E-E62A77A7***",
    "Message": "operation success.",
    "Code": 200,
    "Success": true
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

