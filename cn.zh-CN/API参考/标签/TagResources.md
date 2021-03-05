# TagResources

调用TagResources为资源绑定标签。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=alikafka&api=TagResources&type=RPC&version=2019-09-16)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|TagResources|要执行的操作。取值：

 **TagResources**。 |
|RegionId|String|是|cn-hangzhou|资源的地域ID。 |
|ResourceId.N|RepeatList|是|alikafka\_post-cn-v0h1fgs2\*\*\*\*|需要打标的资源ID 。资源ID规则：

 -   实例：instanceId
-   Topic ：Kafka\_alikafka\_instanceId\_topic
-   Consumer Group：Kafka\_alikafka\_instanceId\_consumerGroup

 例如：实例ID为alikafka\_post-cn-v0h1fgs2xxxx、Topic名称为test-topic、Consumer Group名称为test-consumer-group，则各资源ID分别为alikafka\_post-cn-v0h1fgs2xxxx、Kafka\_alikafka\_post-cn-v0h1fgs2xxxx\_test-topic、Kafka\_alikafka\_post-cn-v0h1fgs2xxxx\_test-consumer-group。 |
|ResourceType|String|是|instance|资源类型。枚举类型。取值：

 -   **INSTANCE**
-   **TOPIC**
-   **CONSUMERGROUP** |
|Tag.N.Key|String|是|FinanceDept|资源的标签键。

 -   N为1~20。
-   不允许为空。
-   最多支持128个字符，不能以aliyun和acs:开头，不能包含http://或者https://。 |
|Tag.N.Value|String|否|FinanceJoshua|资源的标签值。

 -   N为1~20。
-   可以为空。
-   最多支持128个字符，不能以aliyun和acs:开头，不能包含http://或者https://。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|C46FF5A8-C5F0-4024-8262-B16B639225A0|请求ID。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=TagResources
&RegionId=cn-hangzhou
&ResourceId.1=alikafka_post-cn-v0h1fgs2****
&ResourceType=INSTANCE
&Tag.1.Key=FinanceDept
&<公共请求参数>
```

正常返回示例

`XML`格式

```
<TagResourcesResponse>
      <RequestId>C46FF5A8-C5F0-4024-8262-B16B639225A0</RequestId>
</TagResourcesResponse>
```

`JSON`格式

```
{
    "RequestId": "C46FF5A8-C5F0-4024-8262-B16B639225A0"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/alikafka)查看更多错误码。

