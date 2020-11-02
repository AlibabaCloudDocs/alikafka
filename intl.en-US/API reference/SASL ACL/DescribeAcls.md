# DescribeAcls

Queries access control lists \(ACLs\).

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=DescribeAcls&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|DescribeAcls|The operation that you want to perform. Set the value to

 **DescribeAcls**. |
|AclResourceName|String|Yes|demo|The name of the resource whose ACLs you want to query.

 -   The value can be the name of a topic or consumer group.
-   You can use an asterisk \(\*\) to represent the names of all topics or consumer groups. |
|AclResourceType|String|Yes|Topic|The type of the resource. Valid values:

 -   **Topic**
-   **Group** |
|InstanceId|String|Yes|alikafka\_pre-cn-v0h1cng0\*\*\*|The ID of the Message Queue for Apache Kafka instance that contains the resource. |
|RegionId|String|Yes|cn-hangzhou|The ID of the region where the instance is located. |
|Username|String|Yes|test12\*\*\*\*|The name of the user. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|Integer|200|The response code. The HTTP 200 code indicates that the request was successful. |
|KafkaAclList|Array| |The list of ACLs. |
|KafkaAclVO| | | |
|AclOperationType|String|Write|The type of operation allowed by the ACL. Valid values:

 -   **Write**
-   **Read** |
|AclResourceName|String|demo|The name of the resource.

 -   The value can be the name of a topic or consumer group.
-   An asterisk \(\*\) represents the names of all topics or consumer groups. |
|AclResourcePatternType|String|LITERAL|The matching mode. Valid values:

 -   **LITERAL:** full match
-   **PREFIXED:** prefix match |
|AclResourceType|String|Topic|The type of the resource. Valid values:

 -   **Topic**
-   **Group** |
|Host|String|\*|The host. |
|Username|String|test12\*\*\*|The name of the user. |
|Message|String|operation success.|The response message. |
|RequestId|String|46496E38-881E-4719-A2F3-F3DA6AEA\*\*\*|The ID of the request. |
|Success|Boolean|true|Indicates whether the request was successful. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=DescribeAcls
&AclResourceName=demo
&AclResourceType=Topic
&InstanceId=alikafka_pre-cn-v0h1cng0***
&RegionId=cn-hangzhou
&Username=test12****
&<Common request parameters>
```

Sample success responses

`XML` format

```
<DescribeAclsResponse>
      <RequestId>46496E38-881E-4719-A2F3-F3DA6AEA***</RequestId>
      <Message>operation success. </Message>
      <Code>200</Code>
      <Success>true</Success>
      <KafkaAclList>
            <KafkaAclVO>
                  <AclResourceName>demo</AclResourceName>
                  <Username>test12***</Username>
                  <AclResourceType>Topic</AclResourceType>
                  <AclOperationType>Write</AclOperationType>
                  <AclResourcePatternType>LITERAL</AclResourcePatternType>
                  <Host>*</Host>
            </KafkaAclVO>
      </KafkaAclList>
</DescribeAclsResponse>
```

`JSON` format

```
{
    "RequestId": "46496E38-881E-4719-A2F3-F3DA6AEA***",
    "Message": "operation success.",
    "Code": 200,
    "Success": true,
    "KafkaAclList": {
        "KafkaAclVO": {
            "AclResourceName": "demo",
            "Username": "test12***",
            "AclResourceType": "Topic",
            "AclOperationType": "Write",
            "AclResourcePatternType": "LITERAL",
            "Host": "*"
        }
    }
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

