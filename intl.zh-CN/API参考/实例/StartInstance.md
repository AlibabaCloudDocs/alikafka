# StartInstance

调用StartInstance部署实例。

**说明：** 单用户请求频率限制为2 QPS。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=alikafka&api=StartInstance&type=RPC&version=2019-09-16)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|StartInstance|要执行的操作。取值：**StartInstance**。 |
|DeployModule|String|是|vpc|部署模式。取值：

 -   **vpc**：VPC实例
-   **eip**：公网/VPC实例

 实例的部署模式必须与其类型一致。VPC实例，部署模式为**vpc**。公网/VPC实例，部署模式为**eip**。 |
|InstanceId|String|是|alikafka\_post-cn-v0h1fgs2\*\*\*\*|实例的ID。 |
|RegionId|String|是|cn-hangzhou|实例的地域ID。 |
|VpcId|String|是|vpc-bp1r4eg3yrxmygv\*\*\*\*|实例部署的VPC ID |
|VSwitchId|String|是|vsw-bp1j3sg5979fstnpl\*\*\*\*|实例部署的Vswitch ID。 |
|ZoneId|String|是|zonea|实例部署的Zone ID。

 必须为Vswitch的Zone ID。 |
|IsEipInner|Boolean|否|false|是否支持EIP 。取值：

 -   **true**：公网/VPC实例
-   **false**：VPC实例

 EIP支持必须与实例类型一致。 |
|IsSetUserAndPassword|Boolean|否|false|是否设置新的用户名和密码。取值：

 -   **true**：设置新的用户名和密码。
-   **false**：不设置新的用户名和密码。

 仅支持公网/VPC实例。 |
|Username|String|否|username|用户名。

 仅支持公网/VPC实例。 |
|Password|String|否|password|用户密码。

 仅支持公网/VPC实例。 |
|Name|String|否|newInstanceName|实例名称。 |
|SecurityGroup|String|否|sg-bp13wfx7kz9pkow\*\*\*|实例的安全组。

 不填写时，消息队列Kafka版会自动为您的实例配置安全组。如需填写，您需要先为实例创建安全组，详情请参见[创建安全组](~25468~)。 |
|ServiceVersion|String|否|0.10.2|部署的消息队列Kafka版的版本，可选值为0.10.2或2.2.0。 |
|Config|String|否|\{"kafka.log.retention.hours":"33"\}|部署的消息队列Kafka版的初始配置。配置信息必须是一个合法的JSON字符串。

 不填写时，该参数默认为空。

 **Config**目前支持的参数配置如下：

 -   **enable.vpc\_sasl\_ssl**：是否开启VPC传输加密。取值说明如下：
    -   **true**：开启VPC传输加密。如果开启，则须同时开启ACL。
    -   **false**：默认值，不开启VPC传输加密。
-   **enable.acl**：是否开启ACL。取值说明如下：
    -   **true**：开启ACL。
    -   **false**：默认值，不开启ACL。
-   **kafka.log.retention.hours**：在磁盘容量充足的情况下，消息的最长保留时间。单位：小时。取值范围\[72, 480\]，默认值**72**。磁盘容量不足（即磁盘水位达到85%）时，将提前删除旧的消息，以确保服务可用性。
-   **kafka.message.max.bytes**：消息队列Kafka版能收发的消息的最大值，单位：字节。取值范围\[1048576, 10485760\]，默认值**1048576**。修改该配置前，请确认修改值是否匹配生产和消费客户端相应配置。 |
|KMSKeyId|String|否|0d24xxxx-da7b-4786-b981-9a164dxxxxxx|同地域内的云盘加密的密钥ID。您可以在[密钥管理服务控制台](https://kms.console.aliyun.com/?spm=a2c4g.11186623.2.5.336745b8hfiU21)查看密钥ID，也可以创建新的密钥。具体操作，请参见[管理密钥](~~108805~~)。

 传入此参数表示开启实例加密（开启后无法关闭），在调用该接口时，系统会检查是否创建服务关联角色AliyunServiceRoleForAlikafkaInstanceEncryption，若未创建，则会自动创建服务关联角色。更多信息，请参见[服务关联角色](~~190460~~)。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|Integer|200|返回码。返回200代表成功。 |
|Message|String|operation success.|返回信息。 |
|RequestId|String|ABA4A7FD-E10F-45C7-9774-A5236015A\*\*\*|请求ID。 |
|Success|Boolean|true|调用是否成功。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=StartInstance
&DeployModule=vpc
&InstanceId=alikafka_post-cn-v0h1fgs2****
&RegionId=cn-hangzhou
&VpcId=vpc-bp1r4eg3yrxmygv****
&VSwitchId=vsw-bp1j3sg5979fstnpl****
&ZoneId=zonea
&<公共请求参数>
```

正常返回示例

`XML`格式

```
<StartInstanceResponse>
      <Message>operation success.</Message>
      <RequestId>ABA4A7FD-E10F-45C7-9774-A5236015A***</RequestId>
      <Success>true</Success>
      <Code>200</Code>
</StartInstanceResponse>
```

`JSON`格式

```
{
    "RequestId":"ABA4A7FD-E10F-45C7-9774-A5236015A***",
    "Message":"operation success.",
    "Code":"200",
    "Success":"true"
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/alikafka)查看更多错误码。

