# CreateAcl

Creates an access control list \(ACL\).

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=CreateAcl&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|CreateAcl|The operation that you want to perform. Set the value to

 **CreateAcl**. |
|AclOperationType|String|Yes|Read|The type of operation allowed by the ACL. Valid values:

 -   **Write**
-   **Read** |
|AclResourceName|String|Yes|X\*\*\*|The name of the resource.

 -   The value can be the name of a topic or consumer group.
-   You can use an asterisk \(\*\) to represent the names of all topics or consumer groups. |
|AclResourcePatternType|String|Yes|LITERAL|The matching mode. Valid values:

 -   **LITERAL:** full match
-   **PREFIXED:** prefix match |
|AclResourceType|String|Yes|Group|The type of the resource. Valid values:

 -   **Topic**
-   **Group** |
|InstanceId|String|Yes|alikafka\_pre-cn-v0h1cng00\*\*\*|The ID of the Message Queue for Apache Kafka instance that contains the resource. |
|RegionId|String|Yes|cn-hangzhou|The ID of the region where the instance is located. |
|Username|String|Yes|test\*\*\*|The name of the user.

 You can use an asterisk \(\*\) to represent all usernames. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|Integer|200|The response code. The HTTP 200 code indicates that the request was successful. |
|Message|String|operation success|The response message. |
|RequestId|String|56729737-C428-4E1B-AC68-7A8C2D5\*\*\*\*|The ID of the request. |
|Success|Boolean|true|Indicates whether the request was successful. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=CreateAcl
&AclOperationType=Read
&AclResourceName=X***
&AclResourcePatternType=LITERAL
&AclResourceType=Group
&InstanceId=alikafka_pre-cn-v0h1cng00***
&RegionId=cn-hangzhou
&Username=test***
&<Common request parameters>
```

Sample success responses

`XML` format

```
<CreateAclResponse>
      <RequestId>56729737-C428-4E1B-AC68-7A8C2D5****</RequestId>
      <Message>operation success</Message>
      <Code>200</Code>
      <Success>true</Success>
</CreateAclResponse>
```

`JSON` format

```
{
    "RequestId": "56729737-C428-4E1B-AC68-7A8C2D5****",
    "Message": "operation success",
    "Code": 200,
    "Success": true
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

