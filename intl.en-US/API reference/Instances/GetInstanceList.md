# GetInstanceList

Queries Message Queue for Apache Kafka instances in a specified region.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=GetInstanceList&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|GetInstanceList|The operation that you want to perform. Set the value to

 **GetInstanceList**. |
|RegionId|String|Yes|cn-hangzhou|The region ID of the instance. |
|OrderId|String|No|20816072673\*\*\*\*|The ID of the order. You can obtain it in [Order Management](https://usercenter2.aliyun.com/order/list?pageIndex=1&pageSize=20) in Alibaba Cloud User Center. |
|InstanceId.N|RepeatList|No|alikafka\_post-cn-mp91gnw0p\*\*\*|The ID of the instance. |
|Tag.N.Key|String|No|test|The key of resource tag N.

 -   Valid values of N: 1 to 20.
-   If this parameter is empty, the keys of all tags are matched.
-   The tag key can be up to 128 characters in length and cannot start with acs: or aliyun. It cannot contain http:// or https://. |
|Tag.N.Value|String|No|test|The value of resource tag N.

 -   Valid values of N: 1 to 20.
-   This parameter is empty when the Tag.N.Key parameter is empty. If this parameter is empty, the values of all tags are matched.
-   The tag value can be up to 128 characters in length and cannot start with acs: or aliyun. It cannot contain http:// or https://. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|Integer|200|The response code. The HTTP 200 status code indicates that the request is successful. |
|InstanceList|Array of InstanceVO| |The list of instances. |
|InstanceVO| | | |
|AllConfig|String|\{\\"enable.vpc\_sasl\_ssl\\":\\"false\\",\\"kafka.log.retention.hours\\":\\"66\\",\\"enable.acl\\":\\"false\\",\\"kafka.message.max.bytes\\":\\"6291456\\"\}|The current configurations of the instance that you deployed. |
|CreateTime|Long|1577961819000|The time when the instance was created. |
|DeployType|Integer|5|The deployment mode of the instance. Valid values:

 -   **4**: Internet and virtual private cloud \(VPC\) type
-   **5**: VPC type |
|DiskSize|Integer|3600|The disk capacity of the instance. |
|DiskType|Integer|1|The disk type of the instance. Valid values:

 -   **0**: Ultra disk
-   **1**: SSD |
|EipMax|Integer|20|The maximum public traffic configured for the instance. |
|EndPoint|String|192.168.0.\*\*\*:9092,192.168.0. \*\*\*:9092,192.168.0. \*\*\*:9092,192.168.0. \*\*\*:9092|The default endpoint of the instance. |
|ExpiredTime|Long|1893581018000|The time when the instance expires. |
|InstanceId|String|alikafka\_pre-cn-mp919o4v\*\*\*\*|The ID of the instance. |
|IoMax|Integer|20|The maximum traffic for the instance. |
|MsgRetain|Integer|72|The retention period of messages on the instance. |
|Name|String|alikafka\_post-cn-mp91gnw0p\*\*\*|The name of the instance. |
|PaidType|Integer|1|The billing method of the instance. Valid values:

 -   **0**: subscription
-   **1**: pay-as-you-go |
|RegionId|String|cn-hangzhou|The region ID of the instance. |
|SecurityGroup|String|sg-bp13wfx7kz9pkowc\*\*\*|The security group of the instance.

 -   If you deploy the instance by using the Message Queue for Apache Kafka console or by calling the [StartInstance](~~157786~~) operation without configuring a security group, the returned value is empty.
-   If you deploy the instance by calling the [StartInstance](~~157786~~) operation with a security group configured, the returned value is the configured security group. |
|ServiceStatus|Integer|5|The status of the instance. Valid values:

 -   **0**: pending
-   **1**: deploying
-   **5**: running
-   **15**: expired |
|SpecType|String|professional|The edition of the instance. Valid values:

 -   **professional**: Professional Edition \(High Write\)
-   **professionalForHighRead**: Professional Edition \(High Read\)
-   **normal**: Standard Edition |
|SslEndPoint|String|47.111.\*\*. \*\*:9093,121.40. \*\*. \*\*:9093,47.111. \*\*. \*\*:9093|The Secure Sockets Layer \(SSL\) endpoint of the instance. |
|Tags|Array of TagVO| |The tags bound to the instance. |
|TagVO| | | |
|Key|String|test|The key of the resource tag. |
|Value|String|test|The value of the resource tag. |
|TopicNumLimit|Integer|180|The maximum number of topics that can be created on the instance. |
|UpgradeServiceDetailInfo|Struct| |The upgrade information of the instance. |
|Current2OpenSourceVersion|String|2.2.0|The open source Apache Kafka version of the instance. |
|VSwitchId|String|vsw-bp1fvuw0ljd7vzmo3d\*\*\*|The ID of the vSwitch. |
|VpcId|String|vpc-bp1ojac7bv448nifjl\*\*\*|The ID of the VPC. |
|ZoneId|String|zonei|The ID of the zone. |
|Message|String|operation success.|The response message. |
|RequestId|String|4B6D821D-7F67-4CAA-9E13-A5A997C35\*\*\*|The ID of the request. |
|Success|Boolean|true|Indicates whether the request is successful. |

## AllConfig parameter description

|Parameter

|Type

|Valid value

|Example

|Description |
|-----------|------|-------------|---------|-------------|
|enable.vpc\_sasl\_ssl

|Boolean

|true/false

|false

|Indicates whether to enable VPC transmission encryption. If you want to enable it, you must also enable access control list \(ACL\). |
|kafka.log.retention.hours

|Integer

|24~480

|66

|The message retention period. Unit: hours. |
|enable.acl

|Boolean

|true/false

|false

|Indicates whether to enable ACL. |
|kafka.message.max.bytes

|Integer

|1048576~10485760

|6291456

|The maximum size of a message. Unit: bytes. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=GetInstanceList
&RegionId=cn-hangzhou
&RegionId=20816072673****
&InstanceId.1=alikafka_post-cn-mp91gnw0p***
&Tag.1.Key=test
&Tag.1.Value=test
&<Common request parameters>
```

Sample success responses

`XML` format

```
<GetInstanceListResponse>
        <RequestId>4B6D821D-7F67-4CAA-9E13-A5A997C35***</RequestId>
        <Message>operation success. </Message>
        <InstanceList>
              <InstanceVO>
                    <DeployType>5</DeployType>
                    <SslEndPoint>47.111. **. **:9093,121.40. **. **:9093,47.111. **. **:9093</SslEndPoint>
                    <EipMax>20</EipMax>
                    <ZoneId>zonei</ZoneId>
                    <InstanceId>alikafka_post-cn-mp91gnw0p***</InstanceId>
                    <SpecType>professional</SpecType>
                    <IoMax>20</IoMax>
                    <VSwitchId>vsw-bp1fvuw0ljd7vzmo3d***</VSwitchId>
                    <CreateTime>1577961819000</CreateTime>
                    <AllConfig>{}</AllConfig>
                    <EndPoint>192.168.0. ***:9092,192.168.0. ***:9092,192.168.0. ***:9092,192.168.0. ***:9092</EndPoint>
                    <SecurityGroup>sg-bp13wfx7kz9pkowc***</SecurityGroup>
                    <UpgradeServiceDetailInfoVO>
                          <Current2OpenSourceVersion>2.2.0</Current2OpenSourceVersion>
                    </UpgradeServiceDetailInfoVO>
                    <Name>alikafka_post-cn-mp91gnw0p***</Name>
                    <DiskType>1</DiskType>
                    <VpcId>vpc-bp1ojac7bv448nifjl***</VpcId>
                    <ServiceStatus>5</ServiceStatus>
                    <PaidType>1</PaidType>
                    <ExpiredTime>1893581018000</ExpiredTime>
                    <MsgRetain>72</MsgRetain>
                    <DiskSize>3600</DiskSize>
                    <TopicNumLimit>180</TopicNumLimit>
                    <RegionId>cn-hangzhou</RegionId>
                    <Tags>
                          <TagVO>
                                <Value>test</Value>
                                <Key>test</Key>
                          </TagVO>
                    </Tags>
              </InstanceVO>
        </InstanceList>
        <Code>200</Code>
        <Success>true</Success>
</GetInstanceListResponse>
```

`JSON` format

```
{
    "RequestId": "4B6D821D-7F67-4CAA-9E13-A5A997C35***",
    "Message": "operation success.",
    "InstanceList": {
        "InstanceVO": [
            {
                "DeployType": 5,
                "SslEndPoint": "47.111. **. **:9093,121.40. **. **:9093,47.111. **. **:9093",
                "EipMax": 20,
                "ZoneId": "zonei",
                "InstanceId": "alikafka_post-cn-mp91gnw0p***",
                "SpecType": "professional",
                "IoMax": 20,
                "VSwitchId": "vsw-bp1fvuw0ljd7vzmo3d***",
                "CreateTime": 1577961819000,
                "AllConfig": "{}",
                "EndPoint": "192.168.0. ***:9092,192.168.0. ***:9092,192.168.0. ***:9092,192.168.0. ***:9092",
                "SecurityGroup": "sg-bp13wfx7kz9pkowc***",
                "UpgradeServiceDetailInfoVO":{
                            "Current2OpenSourceVersion": "2.2.0"
                },
                "Name": "alikafka_post-cn-mp91gnw0p***",
                "DiskType": 1,
                "VpcId": "vpc-bp1ojac7bv448nifjl***",
                "ServiceStatus": 5,
                "PaidType": 1,
                "ExpiredTime": 1893581018000,
                "MsgRetain": 72,
                "DiskSize": 3600,
                "TopicNumLimit": 180,
                "RegionId": "cn-hangzhou",
                "Tags": {
                           "TagVO": [
                                    {
                                            "Value": "test",
                                            "Key": "test"
                                    }
                            ]
                }
            }
        ]
    },
    "Code": 200,
    "Success": true
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

