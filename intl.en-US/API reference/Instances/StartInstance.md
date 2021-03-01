# StartInstance

Deploys a Message Queue for Apache Kafka instance.

**Note:** Each user can send a maximum of two queries per second \(QPS\).

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=StartInstance&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|StartInstance|The operation that you want to perform. Set the value to **StartInstance**. |
|DeployModule|String|Yes|vpc|The deployment mode of the Message Queue for Apache Kafka instance. Valid values:

 -   **vpc**: virtual private cloud \(VPC\) type
-   **eip**: Internet and VPC type

 The deployment mode of the Message Queue for Apache Kafka instance must be consistent with its instance type. Set this value to **vpc** if your instance type is VPC. Set this value to **eip** if your instance type is Internet and VPC. |
|InstanceId|String|Yes|alikafka\_post-cn-v0h1fgs2\*\*\*\*|The ID of the Message Queue for Apache Kafka instance. |
|RegionId|String|Yes|cn-hangzhou|The region ID of the Message Queue for Apache Kafka instance. |
|VpcId|String|Yes|vpc-bp1r4eg3yrxmygv\*\*\*\*|The ID of the VPC where you want to deploy the Message Queue for Apache Kafka instance. |
|VSwitchId|String|Yes|vsw-bp1j3sg5979fstnpl\*\*\*\*|The ID of the vSwitch where you want to deploy the Message Queue for Apache Kafka instance. |
|ZoneId|String|Yes|zonea|The ID of the zone where you want to deploy the Message Queue for Apache Kafka instance.

 The zone ID of the Message Queue for Apache Kafka instance must be the same as that of the vSwitch. |
|IsEipInner|Boolean|No|false|Specifies whether the Message Queue for Apache Kafka instance supports elastic IP addresses \(EIPs\). Valid values:

 -   **true**: instances of the Internet and VPC type
-   **false**: instances of the VPC type

 This parameter must be consistent with the instance type. Set the parameter to true for instances of the Internet and VPC type or to false for instances of the VPC type. |
|IsSetUserAndPassword|Boolean|No|false|Specifies whether to set a new username and password for the Message Queue for Apache Kafka instance. Valid values:

 -   **true**: sets a new username and password.
-   **false**: does not set a new username or password.

 This parameter is supported only for instances of the Internet and VPC type. |
|Username|String|No|username|The username for the Message Queue for Apache Kafka instance.

 This parameter is supported only for instances of the Internet and VPC type. |
|Password|String|No|password|The password for the Message Queue for Apache Kafka instance.

 This parameter is supported only for instances of the Internet and VPC type. |
|Name|String|No|newInstanceName|The name of the Message Queue for Apache Kafka instance. |
|SecurityGroup|String|No|sg-bp13wfx7kz9pkow\*\*\*|The security group of the Message Queue for Apache Kafka instance.

 If you do not specify this parameter, Message Queue for Apache Kafka automatically configures a security group for your instance. If you specify this parameter, you must create the specified security group in advance. For more information, see [Create a security group](~25468~). |
|ServiceVersion|String|No|0.10.2|The version of the Message Queue for Apache Kafka instance that you want to deploy. Valid values: 0.10.2 and 2.2.0. |
|Config|String|No|\{"kafka.log.retention.hours":"33"\}|The initial configurations of the Message Queue for Apache Kafka instance that you want to deploy. The configuration information must be a valid JSON string.

 If this parameter is not specified, it is empty by default.

 **Config** supports the following parameter settings:

 -   **enable.vpc\_sasl\_ssl**: specifies whether to enable VPC transmission encryption. Valid values:
    -   **true**: enables VPC transmission encryption. If VPC transmission encryption is enabled, access control list \(ACL\) must also be enabled.
    -   **false**: disables VPC transmission encryption. This is the default value.
-   **enable.acl**: specifies whether to enable ACL. Valid values:
    -   **true**: enables ACL.
    -   **false**: disables ACL. This is the default value.
-   **kafka.log.retention.hours**: the maximum message retention period when the disk capacity is sufficient. Unit: hours. Valid values: 24 to 480. Default value: **72**. When disk usage reaches 85%, the disk capacity is insufficient, and earlier messages are deleted to ensure service availability.
-   **kafka.message.max.bytes**: the maximum size of messages that Message Queue for Apache Kafka can send and receive. Unit: bytes. Valid values: 1048576 to 10485760. Default value: **1048576**. Before you modify the configuration, make sure that the new value matches the configuration on the producer and consumer. |
|KMSKeyId|String|No|0d24xxxx-da7b-4786-b981-9a164dxxxxxx|The ID of the key that is used to encrypt disks in the same region. You can view the ID of an existing encryption key or create an encryption key in the [Key Management System \(KMS\) console](https://kms.console.aliyun.com/?spm=a2c4g.11186623.2.5.336745b8hfiU21). For more information, see [Manage keys](~~108805~~).

 If this parameter is specified, instance encryption is enabled. You cannot disable instance encryption after it is enabled. When you call this operation, the system checks whether the AliyunServiceRoleForAlikafkaInstanceEncryption service-linked role is created. If this role is not created, the system automatically creates it. For more information, see [Service-linked roles](~~190460~~). |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|Integer|200|The response code. The HTTP 200 status code indicates that the request is successful. |
|Message|String|operation success.|The response message. |
|RequestId|String|ABA4A7FD-E10F-45C7-9774-A5236015A\*\*\*|The ID of the request. |
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

