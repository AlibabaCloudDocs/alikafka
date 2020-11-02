# GetInstanceList

Queries Message Queue for Apache Kafka instances in a specified region.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=GetInstanceList&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|GetInstanceList|The operation that you want to perform. Set the value to

 **GetInstanceList**. |
|RegionId|String|Yes|cn-hangzhou|The ID of the region where the instance is located. |
|OrderId|String|No|test|The ID of the order. |
|InstanceId.N|RepeatList|No|alikafka\_post-cn-mp91gnw0p\*\*\*|The ID of the instance. |
|Tag.N.Key|String|No|test|The key of the resource tag.

 -   Valid values of N: 1 to 20.
-   If this value is empty, the keys of all tags are matched.
-   The tag key can be up to 128 characters in length. It cannot start with aliyun or acs:, or contain http:// or https://. |
|Tag.N.Value|String|No|test|The value of the resource tag.

 -   Valid values of N: 1 to 20.
-   If you do not specify a tag key, you cannot specify a tag value. If this parameter is empty, the values of all tags are matched.
-   The tag value can be up to 128 characters in length. It cannot start with aliyun or acs:, or contain http:// or https://. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Code|Integer|200|The response code. The HTTP 200 code indicates that the request was successful. |
|InstanceList|Array| |The list of instances. |
|InstanceVO| | | |
|CreateTime|Long|1577961819000|The time when the instance was created. |
|DeployType|Integer|5|The deployment mode of the instance. Valid values:

 -   **4**: Internet and virtual private cloud \(VPC\) type
-   **5**: VPC type |
|DiskSize|Integer|3600|The disk size for the instance. |
|DiskType|Integer|1|The disk type for the instance. Valid values:

 -   **0:** Ultra disk
-   **1:** SSD |
|EipMax|Integer|20|The peak public traffic for the instance. |
|EndPoint|String|192.168.0.\*\*\*:9092,192.168.0. \*\*\*:9092,192.168.0. \*\*\*:9092,192.168.0. \*\*\*:9092|The default endpoint of the instance. |
|ExpiredTime|Long|1893581018000|The time when the instance expires. |
|InstanceId|String|alikafka\_pre-cn-mp919o4v\*\*\*\*|The ID of the instance. |
|IoMax|Integer|20|The peak traffic for the instance. |
|MsgRetain|Integer|72|The retention period of messages on the instance. |
|Name|String|alikafka\_post-cn-mp91gnw0p\*\*\*|The name of the instance. |
|PaidType|Integer|1|The billing method of the instance. Valid values:

 -   **0:** subscription
-   **1:** pay-as-you-go |
|RegionId|String|cn-hangzhou|The ID of the region where the instance is located. |
|SecurityGroup|String|sg-bp13wfx7kz9pkowc\*\*\*|The security group of the instance.

 -   If you deploy the instance in the Message Queue for Apache Kafka console or by calling the StartInstance operation without configuring a security group, the returned value is empty.
-   If you deploy the instance by calling the StartInstance operation with a security group configured, the returned value is the configured security group. |
|ServiceStatus|Integer|5|The status of the instance. Valid values:

 -   **0:** waiting for deployment
-   **1:** deploying
-   **5:** running
-   **15:** expired |
|SpecType|String|professional|The edition of the instance. Valid values:

 -   **professional**: Professional Edition
-   **normal**: Standard Edition |
|SslEndPoint|String|47.111.\*\*. \*\*:9093,121.40. \*\*. \*\*:9093,47.111. \*\*. \*\*:9093|The SSL endpoint of the instance. |
|Tags|Array| |The tags bound to the instance. |
|TagVO| | | |
|Key|String|test|The key of the resource tag. |
|Value|String|test|The value of the resource tag. |
|TopicNumLimit|Integer|180|The maximum number of topics for the instance. |
|UpgradeServiceDetailInfo|Array| |The upgrade information of the instance. |
|UpgradeServiceDetailInfoVO| | | |
|Current2OpenSourceVersion|String|2.2.0|The open-source Apache Kafka version to which the instance is targeted. |
|VSwitchId|String|vsw-bp1fvuw0ljd7vzmo3d\*\*\*|The ID of the vSwitch for the instance. |
|VpcId|String|vpc-bp1ojac7bv448nifjl\*\*\*|The ID of the VPC to which the instance belongs. |
|ZoneId|String|zonei|The ID of the zone where the instance is located. |
|Message|String|operation success.|The response message. |
|RequestId|String|4B6D821D-7F67-4CAA-9E13-A5A997C35\*\*\*|The ID of the request. |
|Success|Boolean|true|Indicates whether the request was successful. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=GetInstanceList
&RegionId=cn-hangzhou
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
                  <InstanceId>alikafka_pre-cn-mp919o4v****</InstanceId>
                  <SpecType>professional</SpecType>
                  <IoMax>20</IoMax>
                  <VSwitchId>vsw-bp1fvuw0ljd7vzmo3d***</VSwitchId>
                  <CreateTime>1577961819000</CreateTime>
                  <EndPoint>192.168.0. ***:9092,192.168.0. ***:9092,192.168.0. ***:9092,192.168.0. ***:9092</EndPoint>
                  <SecurityGroup>sg-bp13wfx7kz9pkowc***</SecurityGroup>
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
            </InstanceVO>
            <InstanceVO>
                  <UpgradeServiceDetailInfo>
                        <UpgradeServiceDetailInfoVO>
                              <Current2OpenSourceVersion>2.2.0</Current2OpenSourceVersion>
                        </UpgradeServiceDetailInfoVO>
                        <UpgradeServiceDetailInfoVO>
                              <Value>test</Value>
                              <Key>test</Key>
                        </UpgradeServiceDetailInfoVO>
                  </UpgradeServiceDetailInfo>
                  <Tags>
                        <TagVO>
                              <Current2OpenSourceVersion>2.2.0</Current2OpenSourceVersion>
                        </TagVO>
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
                "InstanceId": "alikafka_pre-cn-mp919o4v****",
                "SpecType": "professional",
                "IoMax": 20,
                "VSwitchId": "vsw-bp1fvuw0ljd7vzmo3d***",
                "CreateTime": 1577961819000,
                "EndPoint": "192.168.0. ***:9092,192.168.0. ***:9092,192.168.0. ***:9092,192.168.0. ***:9092",
                "SecurityGroup": "sg-bp13wfx7kz9pkowc***",
                "Name": "alikafka_post-cn-mp91gnw0p***",
                "DiskType": 1,
                "VpcId": "vpc-bp1ojac7bv448nifjl***",
                "ServiceStatus": 5,
                "PaidType": 1,
                "ExpiredTime": 1893581018000,
                "MsgRetain": 72,
                "DiskSize": 3600,
                "TopicNumLimit": 180,
                "RegionId": "cn-hangzhou"
            },
            {
                "UpgradeServiceDetailInfo": {
                    "UpgradeServiceDetailInfoVO": [
                        {
                            "Current2OpenSourceVersion": "2.2.0"
                        },
                        {
                            "Value": "test",
                            "Key": "test"
                        }
                    ]
                },
                "Tags": {
                    "TagVO": [
                        {
                            "Current2OpenSourceVersion": "2.2.0"
                        },
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

