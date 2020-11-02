# GetInstanceList

You can call this operation to query Message Queue for Apache Kafka instances in a specified region.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=alikafka&api=GetInstanceList&type=RPC&version=2019-09-16)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|GetInstanceList|The operation that you want to perform. Set the value to CreateMasterSlaveServerGroup. |
|RegionId|String|Yes|cn-hangzhou|The ID of the region where you want to query Message Queue for Apache Kafka instances. |
|OrderId|String|No|test|The order ID of the Message Queue for Apache Kafka instance. |
|InstanceId.N|RepeatList|No|alikafka\_post-cn-mp91gnw0p\*\*\*|The ID of Message Queue for Apache Kafka instance N. |
|Tag.N.Key|String|No|test|The key of tag N bound to the Message Queue for Apache Kafka instance. |
|Tag.N.Value|String|No|test|The value of tag N bound to the Message Queue for Apache Kafka instance. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|Success|Boolean|true|Indicates whether the request is successful. |
|RequestId|String|4B6D821D-7F67-4CAA-9E13-A5A997C3519B|The ID of the request. |
|Code|Integer|200|The returned status code. If "200" is returned, the request is successful. |
|Message|String|operation success.|The returned message. |
|InstanceList|Array| |The returned list of Message Queue for Apache Kafka instances. |
|InstanceId|String|alikafka\_pre-cn-mp919o4v\*\*\*\*|The ID of the Message Queue for Apache Kafka instance. |
|RegionId|String|cn-hangzhou|The region ID of the Message Queue for Apache Kafka instance. |
|ServiceStatus|Integer|5|The status of the Message Queue for Apache Kafka instance. Valid values:

 -   **0:** To be deployed
-   **1:** Being deployed
-   **5:**Running
-   **15:** Expired |
|VpcId|String|vpc-bp1ojac7bv448nifjl\*\*\*|The ID of the Virtual Private cCloud \(VPC\) where the Message Queue for Apache Kafka instance is deployed. |
|VSwitchId|String|vsw-bp1fvuw0ljd7vzmo3d\*\*\*|The ID of the VSwitch associated with the VPC where the Message Queue for Apache Kafka instance is deployed. |
|EndPoint|String|192.168.0.\*\*\*:9092,192.168.0. \*\*\*:9092,192.168.0. \*\*\*:9092,192.168.0. \*\*\*:9092|The default endpoint of the Message Queue for Apache Kafka instance. |
|CreateTime|Long|1577961819000|The time when the Message Queue for Apache Kafka instance was created. |
|ExpiredTime|Long|1893581018000|The time when the Message Queue for Apache Kafka instance expires. |
|DeployType|Integer|5|The deployment mode of the Message Queue for Apache Kafka instance. Valid values:

 -   **4:**Instance of the Internet and VPC type
-   **5:**Instance of the VPC type |
|SslEndPoint|String|47.111.\*\*. \*\*:9093,121.40. \*\*. \*\*:9093,47.111. \*\*. \*\*:9093|The SSL endpoint of the Message Queue for Apache Kafka instance. |
|Name|String|alikafka\_post-cn-mp91gnw0p\*\*\*|The name of the Message Queue for Apache Kafka instance. |
|IoMax|Integer|20|The peak traffic configured for the Message Queue for Apache Kafka instance. |
|EipMax|Integer|20|The peak public traffic configured for the Message Queue for Apache Kafka instance. |
|DiskType|Integer|1|The type of the disk configured for the Message Queue for Apache Kafka instance. Valid values:

 -   **0:** Ultra disk
-   **1:** SSD |
|DiskSize|Integer|3600|The size of the disk configured for the Message Queue for Apache Kafka instance. |
|MsgRetain|Integer|72|The retention period of a message in a Message Queue for Apache Kafka instance. |
|TopicNumLimit|Integer|180|The maximum number of topics that can be configured for the Message Queue for Apache Kafka instance. |
|ZoneId|String|zonei|The zone ID of the Message Queue for Apache Kafka instance. |
|PaidType|Integer|1|The billing mode of the Message Queue for Apache Kafka instance. Valid values:

 -   **0:** Subscription
-   **1:** Pay-as-you-go |
|SpecType|String|professional|The edition of the Message Queue for Apache Kafka instance. Valid values:

 -   **professional:**Professional Edition
-   **normal:**Standard Edition |
|UpgradeServiceDetailInfo|Array| |The upgrade information of the Message Queue for Apache Kafka instance. |
|Current2OpenSourceVersion|String|2.2.0|The open-source Apache Kafka version to which the Message Queue for Apache Kafka instance is targeted. |
|Tags|Array| |The tags bound to the Message Queue for Apache Kafka instance. |
|Key|String|test|The key of the tag bound to the Message Queue for Apache Kafka instance. |
|Value|String|test|The value of the tag bound to the Message Queue for Apache Kafka instance. |

## Examples

Sample requests

```
http(s)://[Endpoint]/? Action=GetInstanceList
&RegionId=cn-hangzhou
&Tag"Key":[{"Key":"test","Value":"test"}]
&InstanceId=alikafka_post-cn-mp91gnw0p***
&<Common request parameters>
```

Sample success responses

`XML` format

```
<Message>operation success. </Message> <RequestId>99B647DF-3F59-4A1F-8C1C-8CD4EBDC738B</RequestId>
<Success>true</Success>
<Code>200</Code>
<InstanceList>
    <InstanceVO>
        <DeployType>5</DeployType>
        <SpecType>professional</SpecType>
        <PaidType>1</PaidType>
        <InstanceId>alikafka_post-cn-mp91gnw0p***</InstanceId>
        <MsgRetain>72</MsgRetain>
        <ZoneId>zonei</ZoneId>
        <IoMax>160</IoMax>
        <VSwitchId>vsw-bp1fvuw0ljd7vzmo3d***</VSwitchId>
        <VpcId>vpc-bp1ojac7bv448nifjl***</VpcId>
        <UpgradeServiceDetailInfo>
            <Current2OpenSourceVersion>2.2.0</Current2OpenSourceVersion>
        </UpgradeServiceDetailInfo>
        <ServiceStatus>5</ServiceStatus>
        <Name>alikafka_post-cn-mp91gnw0p026</Name>
        <Tags>
            <TagVO>
                <Value>test</Value>
                <Key>test</Key>
            </TagVO>
        </Tags>
        <TopicNumLimit>180</TopicNumLimit>
        <DiskSize>3600</DiskSize>
        <RegionId>cn-hangzhou</RegionId>
        <CreateTime>1577961819000</CreateTime>
        <SslEndPoint>47.111. **. **:9093,121.40. **. **:9093,47.111. **. **:9092</SslEndPoint>
        <EipMax>20</EipMax>
        <EndPoint>192.168.0. ***:9092,192.168.0. ***:9092,192.168.0. ***:9092,192.168.0. ***:9092</EndPoint>
        <ExpiredTime>1893581018000</ExpiredTime>
        <DiskType>1</DiskType>
    </InstanceVO>
</InstanceList>
```

`JSON` format

```
{
    "Message": "operation success.",
    "RequestId": "99B647DF-3F59-4A1F-8C1C-8CD4EBDC738B",
    "Success": true,
    "Code": 200,
    "InstanceList": {
        "InstanceVO": [
            {
                "DeployType": 5,
                "SpecType": "professional",
                "PaidType": 1,
                "InstanceId": "alikafka_post-cn-mp91gnw0p***",
                "MsgRetain": 72,
                "ZoneId": "zonei",
                "IoMax": 160,
                "VSwitchId": "vsw-bp1fvuw0ljd7vzmo3d***",
                "VpcId": "vpc-bp1ojac7bv448nifjl***",
                "UpgradeServiceDetailInfo": {
                    "Current2OpenSourceVersion": "2.2.0"
                },
                "ServiceStatus": 5,
                "Name": "alikafka_post-cn-mp91gnw0p026",
                "Tags": {
                    "TagVO": [
                        {
                            "Value": "test",
                            "Key": "test"
                        }
                    ]
                },
                "TopicNumLimit": 180,
                "DiskSize": 3600,
                "RegionId": "cn-hangzhou",
                "CreateTime": 1577961819000,
                "SslEndPoint": "47.111. **. **:9093,121.40. **. **:9093,47.111. **. **:9092",
                "EipMax": "20",
                "EndPoint": "192.168.0. ***:9092,192.168.0. ***:9092,192.168.0. ***:9092,192.168.0. ***:9092",
                "ExpiredTime": 1893581018000,
                "DiskType": 1
} ] } }
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/alikafka).

