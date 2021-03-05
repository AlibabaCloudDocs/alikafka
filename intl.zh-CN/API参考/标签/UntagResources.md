# UntagResources

调用UntagResources为资源解绑标签。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=alikafka&api=UntagResources&type=RPC&version=2019-09-16)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|UntagResources|要执行的操作。取值：

 **UntagResources**。 |
|RegionId|String|是|cn-hangzhou|资源的地域ID。 |
|ResourceId.N|RepeatList|是|alikafka\_post-cn-v0h1fgs2\*\*\*\*|需要解绑的资源ID 。资源ID规则：

 -   实例：instanceId
-   Topic ：Kafka\_instanceId\_topic
-   Consumer Group：Kafka\_instanceId\_consumerGroup

 例如：需要解绑定的资源包括实例（alikafka\_post-cn-v0h1fgs2xxxx）、Topic（test-topic）、Consumer Group（test-consumer-group），则各的资源ID分别为alikafka\_post-cn-v0h1fgs2xxxx、Kafka\_alikafka\_post-cn-v0h1fgs2xxxx\_test-topic、Kafka\_alikafka\_post-cn-v0h1fgs2xxxx\_test-consumer-group。 |
|ResourceType|String|是|instance|资源类型。枚举类型，目前支持的资源类型：

 -   **INSTANCE**
-   **TOPIC**
-   **CONSUMERGROUP** |
|TagKey.N|RepeatList|否|FinanceDept|资源的标签键。

 -   N为1~20。
-   如果为空，且All为true时，匹配所有标签键。
-   最多支持128个字符，不能以aliyun和acs:开头，不能包含http://或者https://。 |
|All|Boolean|否|false|是否解除资源绑定的全部标签。TagKey.N为空时，该参数有效。默认值为**false**。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|C46FF5A8-C5F0-4024-8262-B16B639225A0|请求ID。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=UntagResources
&RegionId=cn-hangzhou
&ResourceId.1=alikafka_post-cn-v0h1fgs2****
&ResourceType=INSTANCE
&<公共请求参数>
```

正常返回示例

`XML`格式

```
<UntagResourcesResponse>
      <RequestId>C46FF5A8-C5F0-4024-8262-B16B639225A0</RequestId>
</UntagResourcesResponse>
```

`JSON`格式

```
{
    "RequestId": "C46FF5A8-C5F0-4024-8262-B16B639225A0"
}
```

## 错误码

访问[错误中心](https://error-center.alibabacloud.com/status/product/alikafka)查看更多错误码。

