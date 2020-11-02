# DescribeAcls

Queries access control lists \(ACLs\).

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=DescribeAcls&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|DescribeAcls|The operation that you want to perform. Set the value to

 **DescribeAcls**. |
|AclResourceName|String|Yes|demo|The name of the resource.

 -   The value can be the name of a topic or consumer group.
-   You can use an asterisk \(\*\) to represent the names of all topics or consumer groups. |
|AclResourceType|String|Yes|Topic|The type of the resource. Valid values:

 -   **Topic**
-   **Group** |
|InstanceId|String|Yes|alikafka\_pre-cn-v0h1cng0\*\*\*|The ID of the Message Queue for Apache Kafka instance. |
|RegionId|String|Yes|cn-hangzhou|The ID of the region where the Message Queue for Apache Kafka instance is located. |
|Username|String|Yes|test12\*\*\*\*|The username of the Simple Authentication and Security Layer \(SASL\) user. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|Integer|200|The returned HTTP status code. A 200 status code indicates that the request succeeded. |
|KafkaAclList|Array| |The list of returned ACLs. |
|KafkaAclVO| | | |
|AclOperationType|String|Write|The type of operations allowed by the ACL. |
|AclResourceName|String|demo|The name of the resource. |
|AclResourcePatternType|String|LITERAL|The matching mode. |
|AclResourceType|String|Topic|The type of the resource. |
|Host|String|\*|The host. |
|Username|String|test12\*\*\*|The username of the SASL user. |
|Message|String|operation success.|The returned message. |
|RequestId|String|46496E38-881E-4719-A2F3-F3DA6AEA\*\*\*|The ID of the request. |
|Success|Boolean|true|Indicates whether the request was successful. |

## Examples

Sample request

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
    "DescribeAclsResponse": {
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
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

