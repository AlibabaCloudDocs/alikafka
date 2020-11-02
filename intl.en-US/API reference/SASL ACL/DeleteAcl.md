# DeleteAcl

Deletes an access control list \(ACL\).

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=DeleteAcl&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|DeleteAcl|The operation that you want to perform. Set the value to

 **DeleteAcl**. |
|AclOperationType|String|Yes|Write|The type of operations allowed by the ACL. Valid values:

 -   **Write**
-   **Read** |
|AclResourceName|String|Yes|demo|The name of the resource.

 -   The value can be the name of a topic or consumer group.
-   You can use an asterisk \(\*\) to represent the names of all topics or consumer groups. |
|AclResourcePatternType|String|Yes|LITERAL|The matching mode. Valid values:

 -   **LITERAL:** full match
-   **PREFIXED:** prefix match |
|AclResourceType|String|Yes|Topic|The type of the resource.

 -   **Topic**
-   **Group** |
|InstanceId|String|Yes|alikafka\_pre-cn-v0h1cng0\*\*\*\*|The ID of the Message Queue for Apache Kafka instance. |
|RegionId|String|Yes|cn-hangzhou|The ID of the region where the Message Queue for Apache Kafka instance is located. |
|Username|String|Yes|test12\*\*\*\*|The username of the Simple Authentication and Security Layer \(SASL\) user. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|Integer|200|The returned HTTP status code. A 200 status code indicates that the request succeeded. |
|Message|String|operation success|The returned message. |
|RequestId|String|B0740227-AA9A-4E14-8E9F-36ED6652\*\*\*|The ID of the request. |
|Success|Boolean|true|Indicates whether the request was successful. |

## Examples

Sample request

```
http(s)://[Endpoint]/? Action=DeleteAcl
&AclOperationType=Write
&AclResourceName=demo
&AclResourcePatternType=LITERAL
&AclResourceType=Topic
&InstanceId=alikafka_pre-cn-v0h1cng0****
&RegionId=cn-hangzhou
&Username=test12****
&<Common request parameters>
```

Sample success responses

`XML` format

```
<DeleteAclResponse>
      <RequestId>B0740227-AA9A-4E14-8E9F-36ED6652***</RequestId>
      <Message>operation success</Message>
      <Code>200</Code>
      <Success>true</Success>
</DeleteAclResponse>
```

`JSON` format

```
{
    "DeleteAclResponse": {
        "RequestId": "B0740227-AA9A-4E14-8E9F-36ED6652***",
        "Message": "operation success",
        "Code": 200,
        "Success": true
    }
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

