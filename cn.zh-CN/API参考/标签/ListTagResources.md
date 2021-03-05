# ListTagResources

调用ListTagResources查询资源绑定的标签列表。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=alikafka&api=ListTagResources&type=RPC&version=2019-09-16)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|ListTagResources|要执行的操作。取值：

 **ListTagResources**。 |
|RegionId|String|是|cn-hangzhou|资源的地域ID。 |
|ResourceType|String|是|instance|资源类型。枚举类型。取值：

 -   **INSTANCE**
-   **TOPIC**
-   **CONSUMERGROUP** |
|ResourceId.N|RepeatList|否|alikafka\_post-cn-v0h1fgs2\*\*\*\*|打标的资源ID 。资源ID规则：

 -   实例：instanceId
-   Topic ：Kafka\_alikafka\_instanceId\_topic
-   Consumer Group：Kafka\_alikafka\_instanceId\_consumerGroup

 例如：实例ID为alikafka\_post-cn-v0h1fgs2xxxx、Topic名称为test-topic、Consumer Group名称为test-consumer-group，则各资源ID分别为alikafka\_post-cn-v0h1fgs2xxxx、Kafka\_alikafka\_post-cn-v0h1fgs2xxxx\_test-topic、Kafka\_alikafka\_post-cn-v0h1fgs2xxxx\_test-consumer-group。 |
|Tag.N.Key|String|否|FinanceDept|资源的标签键。

 -   N为1~20。
-   如果为空，则匹配所有标签键。
-   最多支持128个字符，不能以aliyun和acs:开头，不能包含http://或者https://。 |
|Tag.N.Value|String|否|FinanceJoshua|资源的标签值。

 -   N为1~20。
-   如果标签键为空，则必须为空。为空时，匹配所有标签值。
-   最多支持128个字符，不能以aliyun和acs:开头，不能包含http://或者https://。 |
|NextToken|String|否|caeba0bbb2be03f84eb48b699f0a4883|下一个查询开始的Token。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|NextToken|String|caeba0bbb2be03f84eb48b699f0a4883|下一个查询开始Token。 |
|RequestId|String|DE65F6B7-7566-4802-9007-96F2494AC5XX|请求ID。 |
|TagResources|Array of TagResource| |由资源及其标签组成的集合，包含了资源ID、资源类型和标签键值等信息。 |
|TagResource| | | |
|ResourceId|String|alikafka\_post-cn-v0h1fgs2\*\*\*\*|打标的资源ID 。资源ID规则：

 -   实例：instanceId
-   Topic ：Kafka\_alikafka\_instanceId\_topic
-   Consumer Group：Kafka\_alikafka\_instanceId\_consumerGroup

 例如：实例ID为alikafka\_post-cn-v0h1fgs2xxxx、Topic名称为test-topic、Consumer Group名称为test-consumer-group，则各资源ID分别为alikafka\_post-cn-v0h1fgs2xxxx、Kafka\_alikafka\_post-cn-v0h1fgs2xxxx\_test-topic、Kafka\_alikafka\_post-cn-v0h1fgs2xxxx\_test-consumer-group。 |
|ResourceType|String|instance|资源类型。枚举类型。取值：

 -   **Instance**
-   **Topic**
-   **Consumergroup** |
|TagKey|String|FinanceDept|标签键。 |
|TagValue|String|FinanceJoshua|标签值。 |

## 示例

请求示例

```
http(s)://[Endpoint]/?Action=ListTagResources
&RegionId=cn-hangzhou
&ResourceType=INSTANCE
&ResourceId=alikafka_post-cn-v0h1fgs2****
&<公共请求参数>
```

正常返回示例

`XML`格式

```
<ListTagResourcesResponse>
      <RequestId>DE65F6B7-7566-4802-9007-96F2494AC5XX</RequestId>
      <NextToken>caeba0bbb2be03f84eb48b699f0a4883</NextToken>
      <TagResources>
            <TagResource>
                  <ResourceId>alikafka_post-cn-v0h1fgs2****</ResourceId>
                  <TagKey>FinanceDept</TagKey>
                  <ResourceType>ALIYUN::ALIKAFKA::INSTANCE</ResourceType>
                  <TagValue>FinanceJoshua</TagValue>
            </TagResource>
      </TagResources>
</ListTagResourcesResponse>
```

`JSON`格式

```
{
    "RequestId": "DE65F6B7-7566-4802-9007-96F2494AC5XX",
    "NextToken": "caeba0bbb2be03f84eb48b699f0a4883",
    "TagResources": {
        "TagResource": [
          {
            "ResourceId": "alikafka_post-cn-v0h1fgs2****",
            "TagKey": "FinanceDept",
            "ResourceType": "ALIYUN::ALIKAFKA::INSTANCE",
            "TagValue": "FinanceJoshua"
          }  
        ]
    }
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/alikafka)查看更多错误码。

