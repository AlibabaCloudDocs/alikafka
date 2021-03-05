# DescribeAcls

调用DescribeAcls查询ACL。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=alikafka&api=DescribeAcls&type=RPC&version=2019-09-16)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DescribeAcls|要执行的操作。取值：

 **DescribeAcls**。 |
|AclResourceName|String|是|demo|资源名称。

 -   Topic或Consumer Group的名称。
-   支持使用星号（\*）表示所有Topic或Consumer Group的名称。 |
|AclResourceType|String|是|Topic|资源类型。取值：

 -   **Topic**
-   **Group** |
|InstanceId|String|是|alikafka\_pre-cn-v0h1cng0\*\*\*|实例ID。 |
|RegionId|String|是|cn-hangzhou|地域ID。 |
|Username|String|是|test12\*\*\*\*|用户名。 |
|AclResourcePatternType|String|否|LITERAL|匹配模式。取值：

 -   LITERAL：全匹配
-   PREFIXED：前缀匹配 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|Integer|200|状态码。返回200代表成功。 |
|KafkaAclList|Array of KafkaAclVO| |ACL列表。 |
|KafkaAclVO| | | |
|AclOperationType|String|Write|操作类型。取值：

 -   **Write**：写入
-   **Read**：读取 |
|AclResourceName|String|demo|资源名称。

 -   Topic或Consumer Group的名称。
-   支持使用星号（\*）表示所有Topic或Conusmer Group的名称。 |
|AclResourcePatternType|String|LITERAL|匹配模式。取值：

 -   **LITERAL**：全匹配
-   **PREFIXED**：前缀匹配 |
|AclResourceType|String|Topic|资源类型。取值：

 -   **Topic**
-   **Group** |
|Host|String|\*|主机。 |
|Username|String|test12\*\*\*|用户名。 |
|Message|String|operation success.|返回信息。 |
|RequestId|String|46496E38-881E-4719-A2F3-F3DA6AEA\*\*\*|请求ID。 |
|Success|Boolean|true|调用是否成功。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=DescribeAcls
&AclResourceName=demo
&AclResourceType=Topic
&InstanceId=alikafka_pre-cn-v0h1cng0***
&RegionId=cn-hangzhou
&Username=test12****
&<公共请求参数>
```

正常返回示例

`XML`格式

```
<DescribeAclsResponse>
      <RequestId>46496E38-881E-4719-A2F3-F3DA6AEA***</RequestId>
      <Message>operation success.</Message>
      <Code>200</Code>
      <Success>true</Success>
      <KafkaAclList>
            <KafkaAclVO>
                  <AclResourceName>demo</AclResourceName>
                  <Username>test12***</Username>
                  <AclResourceType>Topic</AclResourceType>
                  <AclOperationType>Write</AclOperationType>
                  <AclResourcePatternType>LITERAL</AclResourcePatternType>
                  <Host>*</Host>
            </KafkaAclVO>
      </KafkaAclList>
</DescribeAclsResponse>
```

`JSON`格式

```
{
    "RequestId": "46496E38-881E-4719-A2F3-F3DA6AEA***",
    "Message": "operation success.",
    "Code": 200,
    "Success": true,
    "KafkaAclList": {
        "KafkaAclVO": [
        {
            "AclResourceName": "demo",
            "Username": "test12***",
            "AclResourceType": "Topic",
            "AclOperationType": "Write",
            "AclResourcePatternType": "LITERAL",
            "Host": "*"
        }
        ]
    }
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/alikafka)查看更多错误码。

