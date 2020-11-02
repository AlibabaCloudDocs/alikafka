# StartInstance

You can call this operation to deploy a Message Queue for Apache Kafka instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=StartInstance&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|StartInstance|The operation that you want to perform. Set the value to StartInstance. |
|DeployModule|String|Yes|vpc|The deployment mode of the Message Queue for Apache Kafka instance. Valid values:

 -   **vpc:**deployment on the Virtual Private Cloud \(VPC\)
-   **eip:**deployment on the Internet and VPC

 **Note:** The deployment mode must be consistent with the instance type of the Message Queue for Apache Kafka instance. A Message Queue for Apache Kafka instance of the VPC type is to be deployed in the **vpc** mode. A Message Queue for Apache Kafka instance of the Internet and VPC type is to be deployed in the **eip**mode. |
|InstanceId|String|Yes|alikafka\_post-cn-v0h1fgs2\*\*\*\*|The ID of the Message Queue for Apache Kafka instance to be deployed. |
|RegionId|String|Yes|cn-hangzhou|The ID of the region where the Message Queue for Apache Kafka instance is to be deployed. |
|VpcId|String|Yes|vpc-bp1r4eg3yrxmygv\*\*\*\*|The ID of the VPC where the Message Queue for Apache Kafka instance is to be deployed. |
|VSwitchId|String|Yes|vsw-bp1j3sg5979fstnpl\*\*\*\*|The ID of the VSwitch associated with the VPC where the Message Queue for Apache Kafka instance is to be deployed. |
|ZoneId|String|Yes|zonea|The ID of the zone where the Message Queue for Apache Kafka instance is to be deployed.

 The zone ID of the Message Queue for Apache Kafka instance must be the same as that of the VSwitch. |
|IsEipInner|Boolean|No|false|Specifies whether the Message Queue for Apache Kafka instance can be deployed in the eip mode, or in other words, supports elastic IP \(EIP\) addresses. Valid values:

 -   **true**: The Message Queue for Apache Kafka instance is of the Internet and VPC type, and therefore can be deployed in the eip mode.
-   **false**: The Message Queue for Apache Kafka instance is of the VPC type, and therefore cannot be deployed in the eip mode.

 **Note**: The support for the eip mode must be consistent with the instance type of the Message Queue for Apache Kafka instance. |
|IsSetUserAndPassword|Boolean|No|false|Specifies whether to set a new user name and password for the Message Queue for Apache Kafka instance. Valid values:

 -   **true**: Set a new user name and password.
-   **false**: Do not set a new user name and password.

 **Note:** This parameter only takes effect when the DeployModule parameter is set to eip. |
|Username|String|No|username|The new user name for the Message Queue for Apache Kafka instance.

 **Note:** This parameter only takes effect when the DeployModule parameter is set to eip. |
|Password|String|No|password|The new password for the Message Queue for Apache Kafka instance.

 **Note:** This parameter only takes effect when the DeployModule parameter is set to eip. |
|Name|String|No|newInstanceName|The new name of the Message Queue for Apache Kafka instance. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|Integer|200|The returned status code. If "200" is returned, the request is successful. |
|Message|String|operation success.|The returned message. |
|RequestId|String|ABA4A7FD-E10F-45C7-9774-A5236015A923|The ID of the request. |
|Success|Boolean|true|Indicates whether the request is successful. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=StartInstance
&DeployModule=vpc
&InstanceId=alikafka_post-cn-v0h1fgs2****
&RegionId=cn-hangzhou
&VpcId=vpc-bp1r4eg3yrxmygv****
&VSwitchId=vsw-bp1j3sg5979fstnpl****
&ZoneId=zonea
&<Common request parameters>
```

Sample success responses

`XML` format

```
<Message>operation success. </Message>
<RequestId>ABA4A7FD-E10F-45C7-9774-A5236015A923</RequestId>
<Success>true</Success>
<Code>200</Code>
```

`JSON` format

```
{
    "Message":"operation success.",
    "RequestId":"ABA4A7FD-E10F-45C7-9774-A5236015A923",
    "Success":true,
    "Code":200
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

