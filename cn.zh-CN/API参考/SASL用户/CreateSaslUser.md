# CreateSaslUser

调用CreateSaslUser创建SASL用户。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=alikafka&api=CreateSaslUser&type=RPC&version=2019-09-16)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|CreateSaslUser|要执行的操作。取值：

 **CreateSaslUser**。 |
|InstanceId|String|是|alikafka\_pre-cn-v0h1cng0\*\*\*\*|实例ID。 |
|Password|String|是|12\*\*\*|密码。 |
|RegionId|String|是|cn-hangzhou|地域ID。 |
|Username|String|是|test\*\*\*|用户名。 |
|Type|String|否|plain|类型。取值：

 -   **plain**：一种简单的用户名密码校验机制。消息队列Kafka版优化了PLAIN机制，支持不重启实例的情况下动态增加SASL用户。
-   **scram**：一种用户名密码校验机制，安全性比PLAIN更高。消息队列Kafka版使用SCRAM-SHA-256。

 默认值为**plain**。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|Integer|200|状态码。返回200代表成功。 |
|Message|String|operation success.|返回消息。 |
|RequestId|String|C5CA600C-7D5A-45B5-B6DB-44FAC2C\*\*\*\*|请求ID。 |
|Success|Boolean|true|调用是否成功。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=CreateSaslUser
&InstanceId=alikafka_pre-cn-v0h1cng0****
&Password=12***
&RegionId=cn-hangzhou
&Username=test***
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<CreateSaslUserResponse>
      <RequestId>C5CA600C-7D5A-45B5-B6DB-44FAC2C****</RequestId>
      <Message>operation success</Message>
      <Code>200</Code>
      <Success>true</Success>
</CreateSaslUserResponse>
```

`JSON` 格式

```
{
    "RequestId":"C5CA600C-7D5A-45B5-B6DB-44FAC2C****",
    "Message":"operation success.",
    "Code":"200",
    "Success":"true"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/alikafka)查看更多错误码。

