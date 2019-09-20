# DeleteTopic {#doc_api_alikafka_DeleteTopic .reference}

调用 DeleteTopic 删除 Topic。

## 调试 {#api_explorer .section}

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=alikafka&api=DeleteTopic&type=RPC&version=2018-10-15)

## 请求参数 {#parameters .section}

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|DeleteTopic|要执行的动作。取值：

 **DeleteTopic** |
|InstanceId|String|是|alikafka\_post-cn-mp91a44k3002|实例 ID。

 可调用 [GetInstanceList](https://help.aliyun.com/document_detail/94533.html?spm=a2c4g.11186623.2.14.539f69b1wPqd3v#concept-94533-zh) 获取。

 |
|RegionId|String|是|cn-hangzhou|实例所属的地域 ID。

 |
|Topic|String|是|Kafkatest|Topic 的名称。

 -   只能包含字母、数字、下划线（\_）和短横线（-）。
-   长度为 3-64 个字符，多于 64 个字符将被自动截取。
-   一旦创建后不能再修改。

 |

## 返回数据 {#resultMapping .section}

## 示例 {#demo .section}

请求示例

``` {#request_demo}

http(s)://[Endpoint]/?Action=DeleteTopic
&InstanceId=alikafka_post-cn-mp91a44k3002
&RegionId=cn-hangzhou
&Topic=Kafkatest
&<公共请求参数>

```

正常返回示例

`XML` 格式

``` {#xml_return_success_demo}
<DeleteTopicResponse>
	  <Message>operation success</Message>
	  <RequestId>9B618B3F-9506-4661-A211-D00C4556F928</RequestId>
	  <Success>true</Success>
	  <Code>200</Code>
</DeleteTopicResponse>
```

`JSON` 格式

``` {#json_return_success_demo}
{
	"Message":"operation success",
	"RequestId":"9B618B3F-9506-4661-A211-D00C4556F928",
	"Success":true,
	"Code":200
}
```

## 错误码 { .section}

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|500|InternalError|An internal error occurred; please try again later.|系统内部错误，请稍后重试|

访问[错误中心](https://error-center.aliyun.com/status/product/alikafka)查看更多错误码。

