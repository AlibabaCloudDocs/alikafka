# CreateAcl

调用CreateAcl创建ACL。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=alikafka&api=CreateAcl&type=RPC&version=2019-09-16)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|CreateAcl|要执行的操作。取值：

 **CreateAcl**。 |
|AclOperationType|String|是|Read|操作类型。取值：

 -   **Write**：写入
-   **Read**：读取 |
|AclResourceName|String|是|X\*\*\*|资源名称。

 -   Topic或Consumer Group的名称。
-   支持使用星号（\*）表示所有Topic或Conusmer Group的名称。 |
|AclResourcePatternType|String|是|LITERAL|匹配模式。取值：

 -   **LITERAL**：全匹配
-   **PREFIXED**：前缀匹配 |
|AclResourceType|String|是|Group|资源类型。取值：

 -   **Topic**
-   **Group** |
|InstanceId|String|是|alikafka\_pre-cn-v0h1cng00\*\*\*|实例ID。 |
|RegionId|String|是|cn-hangzhou|地域ID。 |
|Username|String|是|test\*\*\*|用户名。

 支持使用星号（\*）表示所有用户名。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|Integer|200|状态码。返回200代表成功。 |
|Message|String|operation success|返回信息。 |
|RequestId|String|56729737-C428-4E1B-AC68-7A8C2D5\*\*\*\*|请求ID。 |
|Success|Boolean|true|调用是否成功。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=CreateAcl
&AclOperationType=Read
&AclResourceName=X***
&AclResourcePatternType=LITERAL
&AclResourceType=Group
&InstanceId=alikafka_pre-cn-v0h1cng00***
&RegionId=cn-hangzhou
&Username=test***
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<CreateAclResponse>
      <RequestId>56729737-C428-4E1B-AC68-7A8C2D5****</RequestId>
      <Message>operation success</Message>
      <Code>200</Code>
      <Success>true</Success>
</CreateAclResponse>
```

`JSON` 格式

```
{
    "RequestId": "56729737-C428-4E1B-AC68-7A8C2D5****",
    "Message": "operation success",
    "Code": 200,
    "Success": true
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/alikafka)查看更多错误码。

