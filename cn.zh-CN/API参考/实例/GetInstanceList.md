# GetInstanceList

调用GetInstanceList查询指定地域的实例信息。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=alikafka&api=GetInstanceList&type=RPC&version=2019-09-16)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|GetInstanceList|要执行的操作。取值：

 **GetInstanceList**。 |
|RegionId|String|是|cn-hangzhou|实例的地域ID。 |
|OrderId|String|否|20816072673\*\*\*\*|订单ID。您可以在阿里云用户中心的[订单管理](https://usercenter2.aliyun.com/order/list?pageIndex=1&pageSize=20)中获取。 |
|InstanceId.N|RepeatList|否|alikafka\_post-cn-mp91gnw0p\*\*\*|实例ID。 |
|Tag.N.Key|String|否|test|资源的标签键。

 -   N为1~20。
-   如果为空，则匹配所有标签键。
-   最多支持128个字符，不能以aliyun和acs:开头，不能包含http://或者https://。 |
|Tag.N.Value|String|否|test|资源的标签值。

 -   N为1~20。
-   标签键为空时，必须为空。如果为空，匹配所有标签值。
-   最多支持128个字符，不能以aliyun和acs:开头，不能包含http://或者https://。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|Integer|200|返回码。返回200代表成功。 |
|InstanceList|Array of InstanceVO| |实例详情。 |
|InstanceVO| | | |
|AllConfig|String|\{\\"enable.vpc\_sasl\_ssl\\":\\"false\\",\\"kafka.log.retention.hours\\":\\"66\\",\\"enable.acl\\":\\"false\\",\\"kafka.message.max.bytes\\":\\"6291456\\"\}|部署的消息队列Kafka版的当前配置。 |
|CreateTime|Long|1577961819000|创建时间。 |
|DeployType|Integer|5|部署类型。取值：

 -   **4**：公网/VPC实例
-   **5**：VPC实例 |
|DiskSize|Integer|3600|磁盘容量。 |
|DiskType|Integer|1|磁盘类型。取值：

 -   **0**：高效云盘
-   **1**：SSD |
|EipMax|Integer|20|公网流量峰值。 |
|EndPoint|String|192.168.0.\*\*\*:9092,192.168.0.\*\*\*:9092,192.168.0.\*\*\*:9092,192.168.0.\*\*\*:9092|默认接入点。 |
|ExpiredTime|Long|1893581018000|到期时间。 |
|InstanceId|String|alikafka\_pre-cn-mp919o4v\*\*\*\*|实例ID。 |
|IoMax|Integer|20|流量峰值。 |
|MsgRetain|Integer|72|消息保留时长。 |
|Name|String|alikafka\_post-cn-mp91gnw0p\*\*\*|实例名称。 |
|PaidType|Integer|1|付费类型。取值：

 -   **0**：预付费
-   **1**：后付费 |
|RegionId|String|cn-hangzhou|实例的地域ID。 |
|SecurityGroup|String|sg-bp13wfx7kz9pkowc\*\*\*|实例的安全组。

 -   如果实例是通过消息队列Kafka版控制台或调用[StartInstance](~~157786~~)（调用时没有配置安全组）部署的，则返回值为空。
-   如果实例是通过调用[StartInstance](~~157786~~)（调用时配置了安全组）部署的，则返回值为配置的安全组。 |
|ServiceStatus|Integer|5|实例状态。取值：

 -   **0**：待部署
-   **1**：部署中
-   **5**：服务中
-   **15**：已过期 |
|SpecType|String|professional|实例规格。取值：

 -   **professional**：专业版（高写版）
-   **professionalForHighRead**：专业版（高读版）
-   **normal**：标准版 |
|SslEndPoint|String|47.111.\*\*.\*\*:9093,121.40.\*\*.\*\*:9093,47.111.\*\*.\*\*:9093|SSL接入点。 |
|Tags|Array of TagVO| |标签。 |
|TagVO| | | |
|Key|String|test|标签键。 |
|Value|String|test|标签值。 |
|TopicNumLimit|Integer|180|Topic最大数量。 |
|UpgradeServiceDetailInfo|Struct| |实例的升级信息。 |
|Current2OpenSourceVersion|String|2.2.0|实例的开源版本。 |
|VSwitchId|String|vsw-bp1fvuw0ljd7vzmo3d\*\*\*|VSwitch ID。 |
|VpcId|String|vpc-bp1ojac7bv448nifjl\*\*\*|VPC ID。 |
|ZoneId|String|zonei|可用区ID。 |
|Message|String|operation success.|返回信息。 |
|RequestId|String|4B6D821D-7F67-4CAA-9E13-A5A997C35\*\*\*|请求ID。 |
|Success|Boolean|true|调用是否成功。 |

## AllConfig参数说明

|名称

|类型

|取值范围

|示例值

|描述 |
|----|----|------|-----|----|
|enable.vpc\_sasl\_ssl

|Boolean

|true/false

|false

|是否开启VPC传输加密，如果要开启，必须同时开启ACL。 |
|kafka.log.retention.hours

|Integer

|24~480

|66

|消息保留时长（小时）。 |
|enable.acl

|Boolean

|true/false

|false

|是否开启ACL。 |
|kafka.message.max.bytes

|Integer

|1048576~10485760

|6291456

|最大消息大小（字节）。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=GetInstanceList
&RegionId=cn-hangzhou
&RegionId=20816072673****
&InstanceId.1=alikafka_post-cn-mp91gnw0p***
&Tag.1.Key=test
&Tag.1.Value=test
&<公共请求参数>
```

正常返回示例

`XML`格式

```
<GetInstanceListResponse>
        <RequestId>4B6D821D-7F67-4CAA-9E13-A5A997C35***</RequestId>
        <Message>operation success.</Message>
        <InstanceList>
              <InstanceVO>
                    <DeployType>5</DeployType>
                    <SslEndPoint>47.111.**.**:9093,121.40.**.**:9093,47.111.**.**:9093</SslEndPoint>
                    <EipMax>20</EipMax>
                    <ZoneId>zonei</ZoneId>
                    <InstanceId>alikafka_post-cn-mp91gnw0p***</InstanceId>
                    <SpecType>professional</SpecType>
                    <IoMax>20</IoMax>
                    <VSwitchId>vsw-bp1fvuw0ljd7vzmo3d***</VSwitchId>
                    <CreateTime>1577961819000</CreateTime>
                    <AllConfig>{}</AllConfig>
                    <EndPoint>192.168.0.***:9092,192.168.0.***:9092,192.168.0.***:9092,192.168.0.***:9092</EndPoint>
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

`JSON`格式

```
{
    "RequestId": "4B6D821D-7F67-4CAA-9E13-A5A997C35***",
    "Message": "operation success.",
    "InstanceList": {
        "InstanceVO": [
            {
                "DeployType": 5,
                "SslEndPoint": "47.111.**.**:9093,121.40.**.**:9093,47.111.**.**:9093",
                "EipMax": 20,
                "ZoneId": "zonei",
                "InstanceId": "alikafka_post-cn-mp91gnw0p***",
                "SpecType": "professional",
                "IoMax": 20,
                "VSwitchId": "vsw-bp1fvuw0ljd7vzmo3d***",
                "CreateTime": 1577961819000,
                "AllConfig": "{}",
                "EndPoint": "192.168.0.***:9092,192.168.0.***:9092,192.168.0.***:9092,192.168.0.***:9092",
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

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/alikafka)查看更多错误码。

