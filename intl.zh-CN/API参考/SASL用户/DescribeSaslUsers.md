# DescribeSaslUsers

调用DescribeSaslUsers查询SASL用户。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=alikafka&api=DescribeSaslUsers&type=RPC&version=2019-09-16)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DescribeSaslUsers|要执行的操作。取值：

 **DescribeSaslUsers**。 |
|InstanceId|String|是|alikafka\_pre-cn-v0h1cng0\*\*\*\*|实例ID。 |
|RegionId|String|是|cn-hangzhou|地域ID。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|Integer|200|状态码。返回200代表成功。 |
|Message|String|operation success.|返回信息。 |
|RequestId|String|9E3B3592-5994-4F65-A61E-E62A77A7\*\*\*|请求ID。 |
|SaslUserList|Array| |SASL用户列表。 |
|SaslUserVO| | | |
|Password|String|123\*\*\*|密码。 |
|Type|String|scram|类型。取值：

 -   **PLAIN**： 一种简单的用户名密码校验机制。消息队列Kafka版优化了PLAIN机制，支持不重启实例的情况下动态增加SASL用户。
-   **SCRAM**：一种用户名密码校验机制，安全性比PLAIN更高。消息队列Kafka版使用SCRAM-SHA-256。

 默认值为**plain**。 |
|Username|String|test12\*\*\*|用户名。 |
|Success|Boolean|true|是否成功。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=DescribeSaslUsers
&InstanceId=alikafka_pre-cn-v0h1cng0****
&RegionId=cn-hangzhou
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<DescribeSaslUsersResponse>
      <SaslUserList>
            <SaslUserVO>
                  <Type>scram</Type>
                  <Username>test12***</Username>
                  <Password>123***</Password>
            </SaslUserVO>
      </SaslUserList>
      <RequestId>9E3B3592-5994-4F65-A61E-E62A77A7***</RequestId>
      <Message>operation success.</Message>
      <Code>200</Code>
      <Success>true</Success>
</DescribeSaslUsersResponse>
```

`JSON` 格式

```
{
    "SaslUserList": {
        "SaslUserVO": {
            "Type": "scram",
            "Username": "test12***",
            "Password": "123***"
        }
    },
    "RequestId": "9E3B3592-5994-4F65-A61E-E62A77A7***",
    "Message": "operation success.",
    "Code": 200,
    "Success": true
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/alikafka)查看更多错误码。

