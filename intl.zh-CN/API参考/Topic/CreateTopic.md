# CreateTopic

调用CreateTopic创建Topic。

-   单用户请求频率限制为1 QPS。
-   每个实例下最多可创建的Topic数量与您所购买的实例规格相关。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=alikafka&api=CreateTopic&type=RPC&version=2019-09-16)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|CreateTopic|要执行的操作。取值：

 **CreateTopic**。 |
|InstanceId|String|是|alikafka\_pre-cn-mp919o4v\*\*\*\*|实例ID。 |
|RegionId|String|是|cn-hangzhou|实例的地域ID。 |
|Remark|String|是|alikafka\_topic\_test|Topic的备注。

 -   只能包含字母、数字、下划线（\_）、短划线（-）。
-   长度为3~64字符。 |
|Topic|String|是|alikafka\_topic\_test|Topic的名称。

 -   只能包含字母、数字、下划线（\_）和短划线（-）。
-   长度限制为3~64字符，多于64个字符将被自动截取。
-   Topic名称一旦创建，将无法修改。 |
|CompactTopic|Boolean|否|false|Topic的存储引擎配置为Local存储时，会配置日志清理策略。取值：

 -   false：delete清理策略。
-   true：compact清理策略。 |
|PartitionNum|String|否|12|Topic的分区数。

 -   分区数限制1~360。
-   建议分区数是6的倍数，减少数据倾斜风险。
-   特殊需求请提交工单。 |
|LocalTopic|Boolean|否|false|Topic的存储引擎。取值：

 -   false：云存储。
-   true：Local存储。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|Integer|200|返回码。返回200代表成功。 |
|Message|String|operation success|返回信息。 |
|RequestId|String|9C0F207C-77A6-43E5-991C-9D98510A\*\*\*\*|请求的ID。 |
|Success|Boolean|true|调用是否成功。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=CreateTopic
&InstanceId=alikafka_pre-cn-mp919o4v****
&RegionId=cn-hangzhou
&Remark=alikafka_topic_test
&Topic=alikafka_topic_test
&<公共请求参数>
```

正常返回示例

`XML`格式

```
<CreateTopicResponse>
      <RequestId>9C0F207C-77A6-43E5-991C-9D98510A****</RequestId>
      <Message>operation success</Message>
      <Code>200</Code>
      <Success>true</Success>
</CreateTopicResponse>
```

`JSON`格式

```
{
    "RequestId": "9C0F207C-77A6-43E5-991C-9D98510A****",
    "Message": "operation success",
    "Code": 200,
    "Success": true
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/alikafka)查看更多错误码。

