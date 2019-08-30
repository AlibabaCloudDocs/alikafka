# CreateTopic {#doc_api_alikafka_CreateTopic .reference}

调用 CreateTopic 创建 Topic。

调用该接口创建 Topic 时，请注意：

-   单用户请求频率限制为 1 QPS。
-   每个实例下最多可创建的 Topic 数量与您所购买的实例版本相关，详情请参见[计费说明](https://help.aliyun.com/document_detail/84737.html)。

## 调试 {#api_explorer .section}

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=alikafka&api=CreateTopic&type=RPC&version=2018-10-15)

## 请求参数 {#parameters .section}

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|CreateTopic|系统规定参数。

 取值：**CreateTopic**

 |
|InstanceId|String|是|alikafka\_pre-cn-mp919o4v\*\*\*\*|实例 ID。

 可调用 [GetInstanceList](https://help.aliyun.com/document_detail/94533.html?spm=a2c4g.11186623.2.12.774c7dc8F5cWRE#concept-94533-zh) 获取。

 |
|RegionId|String|是|cn-hangzhou|实例所属的地域 ID。

 |
|Remark|String|是|alikafka\_topic\_test|Topic 的备注。

 限制 64 个字符。

 |
|Topic|String|是|alikafka\_topic\_test|Topic 的名称。

 -   只能包含字母、数字、下划线（\_）和短横线（-）。
-   长度为 3-64 个字符，多于 64 个字符将被自动截取。
-   一旦创建后不能再修改。

 |

## 返回数据 {#resultMapping .section}

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|Integer|200|返回码。

 返回 **200** 为成功。

 |
|Message|String|operation success|返回信息。

 |
|RequestId|String|9C0F207C-77A6-43E5-991C-9D98510A\*\*\*\*|请求的唯一标识 ID。

 |
|Success|Boolean|true|调用是否成功。

 |

## 示例 {#demo .section}

请求示例

``` {#request_demo}

http(s)://[Endpoint]/?Action=CreateTopic
&InstanceId=alikafka_pre-cn-mp919o4v**** 
&RegionId=cn-hangzhou
&Remark=alikafka_topic_test
&Topic=alikafka_topic_test
&<公共请求参数>

```

正常返回示例

`XML` 格式

``` {#xml_return_success_demo}
<CreateTopicResponse>
	  <Message>operation success</Message>
	  <RequestId>9C0F207C-77A6-43E5-991C-9D98510A****</RequestId>
	  <Success>true</Success>
	  <Code>200</Code>
</CreateTopicResponse>
```

`JSON` 格式

``` {#json_return_success_demo}
{
	"Message":"operation success",
	"RequestId":"9C0F207C-77A6-43E5-991C-9D98510A****",
	"Success":true,
	"Code":200
}
```

## 错误码 { .section}

访问[错误中心](https://error-center.aliyun.com/status/product/alikafka)查看更多错误码。

