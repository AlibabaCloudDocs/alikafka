# GetInstanceList {#doc_api_alikafka_GetInstanceList .reference}

调用 GetInstanceList 获取您在某地域下所购买的实例的信息。

## 调试 {#api_explorer .section}

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=alikafka&api=GetInstanceList&type=RPC&version=2018-10-15)

## 请求参数 {#parameters .section}

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|GetInstanceList|要执行的动作。取值：**GetInstanceList**

 |
|RegionId|String|是|cn-hangzhou|实例所属的地域 ID。

 |

## 返回数据 {#resultMapping .section}

|名称|类型|示例值|描述|
|--|--|---|--|
|Code|Integer|200|返回码，返回 “200” 代表成功。

 |
|InstanceList| | |实例列表信息。

 |
|CreateTime|Long|1566215995000|创建时间。

 |
|DeployType|Integer|4|部署类型。

 |
|EndPoint|String|192.168.0.212:9092,192.168.0.210:9092,192.168.0.211:9092|默认接入点。

 |
|ExpiredTime|Long|1568908800000|过期时间。

 |
|InstanceId|String|alikafka\_pre-cn-mp919o4vm006|实例 ID。

 |
|Name|String|alikafka\_pre-cn-mp919o4vm006|实例名称。

 |
|RegionId|String|cn-hangzhou|实例所属的地域 ID。

 |
|ServiceStatus|Integer|5|服务状态。

 |
|SslEndPoint|String|47.111.110.11:9093,121.40.96.141:9093,47.111.118.133:9093|SSL 接入点。

 |
|UpgradeServiceDetailInfo| | |升级服务详细信息。

 |
|Current2OpenSourceVersion|String|0.10|开源版本

 |
|VSwitchId|String|vsw-bp13rg6bcpkxofr78knbv|VSwitch ID。

 |
|VpcId|String|vpc-bp1l6hrlykj3405r7opgc|VPC ID。

 |
|Message|String|operation success.|返回信息。

 |
|RequestId|String|ABA4A7FD-E10F-45C7-9774-A523601560AC|请求 ID。

 |
|Success|Boolean|true|调用是否成功。

 |

## 示例 {#demo .section}

请求示例

``` {#request_demo}

http(s)://[Endpoint]/?Action=GetInstanceList
&RegionId=cn-hangzhou
&<公共请求参数>

```

正常返回示例

`XML` 格式

``` {#xml_return_success_demo}
<GetInstanceListResponse>
      <Message>operation success.</Message>
      <RequestId>0220FACD-4D57-4F46-BA77-AD3334989A14</RequestId>
      <Success>true</Success>
      <Code>200</Code>
      <InstanceList>
            <InstanceVO>
                  <Name>alikafka_pre-cn-mp919o4vm006</Name>
                  <DeployType>4</DeployType>
                  <CreateTime>1566215995000</CreateTime>
                  <RegionId>cn-hangzhou</RegionId>
                  <InstanceId>alikafka_pre-cn-mp919o4vm006</InstanceId>
                  <SslEndPoint>47.111.110.11:9093,121.40.96.141:9093,47.111.118.133:9093</SslEndPoint>
                  <EndPoint>192.168.0.212:9092,192.168.0.210:9092,192.168.0.211:9092</EndPoint>
                  <ExpiredTime>1568908800000</ExpiredTime>
                  <VSwitchId>vsw-bp13rg6bcpkxofr78knbv</VSwitchId>
                  <VpcId>vpc-bp1l6hrlykj3405r7opgc</VpcId>
                  <UpgradeServiceDetailInfo>
                        <Current2OpenSourceVersion>0.10</Current2OpenSourceVersion>
                  </UpgradeServiceDetailInfo>
                  <ServiceStatus>5</ServiceStatus>
            </InstanceVO>
            <InstanceVO>
                  <Name>alikafka_pre-cn-mp919nhru003</Name>
                  <DeployType>5</DeployType>
                  <CreateTime>1566186050000</CreateTime>
                  <RegionId>cn-hangzhou</RegionId>
                  <InstanceId>alikafka_pre-cn-mp919nhru003</InstanceId>
                  <SslEndPoint></SslEndPoint>
                  <EndPoint>192.168.0.207:9092,192.168.0.208:9092,192.168.0.209:9092</EndPoint>
                  <ExpiredTime>1568908800000</ExpiredTime>
                  <VSwitchId>vsw-bp13rg6bcpkxofr78knbv</VSwitchId>
                  <VpcId>vpc-bp1l6hrlykj3405r7opgc</VpcId>
                  <UpgradeServiceDetailInfo>
                        <Current2OpenSourceVersion>0.10</Current2OpenSourceVersion>
                  </UpgradeServiceDetailInfo>
                  <ServiceStatus>5</ServiceStatus>
            </InstanceVO>
      </InstanceList>
</GetInstanceListResponse>
```

`JSON` 格式

``` {#json_return_success_demo}
{
	"Message":"operation success.",
	"RequestId":"0220FACD-4D57-4F46-BA77-AD3334989A14",
	"Success":true,
	"Code":200,
	"InstanceList":{
		"InstanceVO":[
			{
				"Name":"alikafka_pre-cn-mp919o4vm006",
				"DeployType":4,
				"InstanceId":"alikafka_pre-cn-mp919o4vm006",
				"RegionId":"cn-hangzhou",
				"CreateTime":1566215995000,
				"SslEndPoint":"47.111.110.11:9093,121.40.96.141:9093,47.111.118.133:9093",
				"EndPoint":"192.168.0.212:9092,192.168.0.210:9092,192.168.0.211:9092",
				"VSwitchId":"vsw-bp13rg6bcpkxofr78knbv",
				"ExpiredTime":1568908800000,
				"VpcId":"vpc-bp1l6hrlykj3405r7opgc",
				"ServiceStatus":5,
				"UpgradeServiceDetailInfo":{
					"Current2OpenSourceVersion":"0.10"
				}
			},
			{
				"Name":"alikafka_pre-cn-mp919nhru003",
				"DeployType":5,
				"InstanceId":"alikafka_pre-cn-mp919nhru003",
				"RegionId":"cn-hangzhou",
				"CreateTime":1566186050000,
				"SslEndPoint":"",
				"EndPoint":"192.168.0.207:9092,192.168.0.208:9092,192.168.0.209:9092",
				"VSwitchId":"vsw-bp13rg6bcpkxofr78knbv",
				"ExpiredTime":1568908800000,
				"VpcId":"vpc-bp1l6hrlykj3405r7opgc",
				"ServiceStatus":5,
				"UpgradeServiceDetailInfo":{
					"Current2OpenSourceVersion":"0.10"
				}
			}
		]
	}
}
```

## 错误码 { .section}

|HttpCode|错误码|错误信息|描述|
|--------|---|----|--|
|500|InternalError|An internal error occurred; please try again later.|系统内部错误，请稍后重试|

访问[错误中心](https://error-center.aliyun.com/status/product/alikafka)查看更多错误码。

