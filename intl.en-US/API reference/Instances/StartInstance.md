# StartInstance

Deploys a Message Queue for Apache Kafka instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=StartInstance&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description |
|---------|----|--------|-------|------------|
|Action|String|Yes|StartInstance|The operation that you want to perform. Set the value to

 **StartInstance**. |
|DeployModule|String|Yes|vpc|The deployment mode of the instance. Valid values:

 -   **vpc:** virtual private cloud \(VPC\)
-   **eip:** Internet and VPC

 The deployment mode of the instance must be consistent with the instance type. Set this value to **vpc** if your instance type is VPC. Set this value to **eip** if your instance type is Internet and VPC. |
|InstanceId|String|Yes|alikafka\_post-cn-v0h1fgs2\*\*\*\*|The ID of the instance. |
|RegionId|String|Yes|cn-hangzhou|The ID of the region where the instance is located. |
|VpcId|String|Yes|vpc-bp1r4eg3yrxmygv\*\*\*\*|The ID of the VPC on which you want to deploy the instance. |
|VSwitchId|String|Yes|vsw-bp1j3sg5979fstnpl\*\*\*\*|The ID of the vSwitch associated with the VPC. |
|ZoneId|String|Yes|zonea|The ID of the zone where you want to deploy the instance.

 The zone ID of the instance must be the same as that of the vSwitch. |
|IsEipInner|Boolean|No|false|Specifies whether the instance supports elastic IP addresses \(EIPs\). Valid values:

 -   **true**: The instance supports EIP mode.
-   **false**: The instance does not support EIP mode.

 This parameter must be consistent with the instance type. Set the parameter to true for instances of the Internet and VPC type or to false for instances of the VPC type. |
|IsSetUserAndPassword|Boolean|No|false|Specifies whether to set a new user name and password for instance. Valid values:

 -   **true**: Set a new user name and password.
-   **false**: Do not set a new user name and password.

 This parameter is supported only for instances of the Internet and VPC type. |
|Username|String|No|username|The new user name for the instance.

 This parameter is supported only for instances of the Internet and VPC type. |
|Password|String|No|password|The new password for the instance.

 This parameter is supported only for instances of the Internet and VPC type. |
|Name|String|No|newInstanceName|The new name of the instance. |
|SecurityGroup|String|No|sg-bp13wfx7kz9pkow\*\*\*|The security group of the instance.

 If you do not specify this parameter, Message Queue for Apache Kafka automatically configures a security group for the instance. If you specify this parameter, you must create the specified security group in advance. For more information, see [Create a security group](~25468~). |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|Integer|200|The response code. The HTTP 200 code indicates that the request was successful. |
|Message|String|operation success.|The response message. |
|RequestId|String|ABA4A7FD-E10F-45C7-9774-A5236015A\*\*\*|The ID of the request. |
|Success|Boolean|true|Indicates whether the request was successful. |

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
<StartInstanceResponse>
      <Message>operation success. </Message>
      <RequestId>ABA4A7FD-E10F-45C7-9774-A5236015A***</RequestId>
      <Success>true</Success>
      <Code>200</Code>
</StartInstanceResponse>
```

`JSON` format

```
{
    "RequestId":"ABA4A7FD-E10F-45C7-9774-A5236015A***",
    "Message":"operation success.",
    "Code":"200",
    "Success":"true"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

