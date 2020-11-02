# DeleteAcl

调用DeleteAcl删除ACL。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=alikafka&api=DeleteAcl&type=RPC&version=2019-09-16)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DeleteAcl|要执行的操作。取值：

 **DeleteAcl**。 |
|AclOperationType|String|是|Write|操作类型。取值：

 -   **Write**：读取
-   **Read**：写入 |
|AclResourceName|String|是|demo|资源名称。

 -   Topic名称或Consumer Group的名称。
-   星号（\*）表示所有Topic或Consumer Group的名称。 |
|AclResourcePatternType|String|是|LITERAL|匹配模式。取值：

 -   **LITERAL**：全匹配模式
-   **PREFIXED**：前缀匹配模式 |
|AclResourceType|String|是|Topic|资源类型。

 -   **Topic**
-   **Group** |
|InstanceId|String|是|alikafka\_pre-cn-v0h1cng0\*\*\*\*|实例ID。 |
|RegionId|String|是|cn-hangzhou|地域ID。 |
|Username|String|是|test12\*\*\*\*|用户名。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|Integer|200|状态码。返回200代表成功。 |
|Message|String|operation success|返回信息。 |
|RequestId|String|B0740227-AA9A-4E14-8E9F-36ED6652\*\*\*|请求ID。 |
|Success|Boolean|true|调用是否成功。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=DeleteAcl
&AclOperationType=Write
&AclResourceName=demo
&AclResourcePatternType=LITERAL
&AclResourceType=Topic
&InstanceId=alikafka_pre-cn-v0h1cng0****
&RegionId=cn-hangzhou
&Username=test12****
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<DeleteAclResponse>
      <RequestId>B0740227-AA9A-4E14-8E9F-36ED6652***</RequestId>
      <Message>operation success</Message>
      <Code>200</Code>
      <Success>true</Success>
</DeleteAclResponse>
```

`JSON` 格式

```
{
    "RequestId": "B0740227-AA9A-4E14-8E9F-36ED6652***",
    "Message": "operation success",
    "Code": 200,
    "Success": true
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/alikafka)查看更多错误码。

